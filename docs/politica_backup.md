# Política de Backup e Restauração (CDC Chat)

Este documento define as regras, frequências, responsabilidades e procedimentos para backup e recuperação de desastres do ecossistema CDC Chat (Mattermost).

---

## Estratégia 3-2-1

Seguimos a metodologia clássica de redundância de dados:
- **3 Cópias dos Dados:** Manter pelo menos 1 cópia de produção ativa e 2 backups independentes.
- **2 Mídias Diferentes:** Armazenar os backups em sistemas físicos ou lógicos distintos (ex: armazenamento local na VPS e armazenamento em nuvem externa/objeto).
- **1 Cópia Offsite (Geograficamente Separada):** Pelo menos 1 cópia dos backups deve ser enviada para fora da rede física da VPS de produção para proteção contra incidentes catastróficos no provedor.

---

## Escopo do Backup

### O que deve ser incluído (Backup Obrigatório):
- **Banco de Dados (PostgreSQL):** Tabelas de posts, usuários, canais, sessões, integrações e configurações de times.
- **Arquivos Enviados (Uploads):** O diretório `./data/data` que contém todas as mídias, imagens e documentos compartilhados pelos usuários.
- **Configurações Lógicas:** O diretório `./data/config` contendo as customizações do Mattermost.
- **Configurações de Orquestração:** Arquivos `.env`, `docker-compose.yml` e arquivos do Nginx associados.

### O que NÃO deve ser incluído (Evitar Desperdício de Armazenamento):
- Caches temporários de imagens ou visualização de links.
- Logs de depuração do Docker ou do Mattermost (que são recriados constantemente).
- Imagens Docker locais e volumes órfãos.

---

## Frequência e Métricas (RPO e RTO)

- **Frequência dos Backups:** Diário (executado automaticamente às 03:00 UTC).
- **Retenção:** 30 dias para os pacotes diários; 6 meses para backups mensais consolidados.
- `<TODO: VALIDAR RPO E RTO COM O RESPONSAVEL PELO NEGOCIO>` (definir se a tolerância de perda de dados e tempo de restauração atual atende as demandas críticas).

---

## Script Automatizado de Backup (`backup.sh`)

Abaixo está o script oficial em Bash utilizado para rotinas de backup. Salve-o no host como `backup.sh`. Ele utiliza tratamento de sinal `trap` para limpar arquivos residuais em caso de erros e envia notificações ao Mattermost.

```bash
#!/usr/bin/env bash

# Configura regras estritas do shell para falhar rapidamente em caso de erros
set -Eeuo pipefail

# Obtém o diretório do script
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" &>/dev/null && pwd)"

# Define variáveis locais temporárias
TMP_DIR="/tmp/chat_backup_$$"
DATE_SUFFIX="$(date +%Y%m%d_%H%M%S)"
BACKUP_FILENAME="backup_chat_${DATE_SUFFIX}.tar.gz"

# Função executada em caso de erros ou sinal de saída (Limpeza garantida)
cleanup() {
    local exit_code=$?
    echo "[INFO] Executando rotina de limpeza temporaria..."
    rm -rf "${TMP_DIR}"
    echo "[INFO] Limpeza concluida. Codigo de saida: ${exit_code}"
    exit "${exit_code}"
}
trap cleanup EXIT SIGINT SIGTERM

# Envio de alertas ao Mattermost
notify_mattermost() {
    local message="$1"
    if [ "${MATTERMOST_ENABLED:-false}" = "true" ] && [ -n "${MATTERMOST_WEBHOOK_URL:-}" ]; then
        echo "[INFO] Enviando notificacao ao Mattermost..."
        curl --fail --silent --show-error --max-time "${MATTERMOST_TIMEOUT_SECONDS:-10}" \
            -X POST -H "Content-Type: application/json" \
            -d "{\"text\": \"${message}\"}" \
            "${MATTERMOST_WEBHOOK_URL}" || echo "[WARNING] Falha ao enviar alerta ao Mattermost. O backup continuou."
    fi
}

echo "[INFO] Iniciando rotina de backup do CDC Chat..."

# 1. Carregar configurações
if [ -f "${SCRIPT_DIR}/.env" ]; then
    # Exporta variáveis sem incluir comentários
    export $(grep -v '^#' "${SCRIPT_DIR}/.env" | xargs)
else
    echo "[ERROR] Arquivo .env nao localizado no diretorio do script."
    exit 1
fi

# Valida variáveis cruciais
if [ -z "${DB_PASS:-}" ] || [ -z "${BACKUP_DESTINATION:-}" ]; then
    echo "[ERROR] Variaveis obrigatorias DB_PASS ou BACKUP_DESTINATION nao definidas."
    exit 1
fi

# 2. Criar diretório temporário
mkdir -p "${TMP_DIR}"
mkdir -p "${BACKUP_DESTINATION}"

# Dispara alerta de início de backup
notify_mattermost "[INFO] Rotina de backup do CDC Chat iniciada para o ambiente ${APP_ENV:-production}."

# 3. Exportar banco PostgreSQL de forma segura (sem expor senha no CLI)
echo "[INFO] Executando dump do banco de dados..."
docker compose exec -e PGPASSWORD="${DB_PASS}" db pg_dump -U mmuser -h localhost -d mattermost -F c -b -f /var/lib/postgresql/data/mattermost_temp.backup

# Move o backup de dentro do volume para nossa pasta temporária
mv "${SCRIPT_DIR}/data/db/mattermost_temp.backup" "${TMP_DIR}/mattermost.backup"

# 4. Compactar dump, anexos de mídia e configurações lógicas
echo "[INFO] Compactando arquivos (banco e anexos)..."
tar -czf "${TMP_DIR}/${BACKUP_FILENAME}" \
    -C "${TMP_DIR}" mattermost.backup \
    -C "${SCRIPT_DIR}" .env.example docker-compose.yml data/data data/config

# 5. Criptografar arquivo compactado (GPG Simétrico)
# Nota: Requer arquivo de passphrase configurado no host
if [ -f "${GPG_PASSPHRASE_FILE:-}" ]; then
    echo "[INFO] Criptografando pacote de backup..."
    gpg --batch --yes --passphrase-file "${GPG_PASSPHRASE_FILE}" \
        -c -o "${BACKUP_DESTINATION}/${BACKUP_FILENAME}.gpg" "${TMP_DIR}/${BACKUP_FILENAME}"
    
    # Gera checksum SHA-256 do arquivo criptografado final
    sha256sum "${BACKUP_DESTINATION}/${BACKUP_FILENAME}.gpg" > "${BACKUP_DESTINATION}/${BACKUP_FILENAME}.gpg.sha256"
    
    # 6. Aplicar política de retenção (remover arquivos mais antigos que X dias)
    echo "[INFO] Aplicando retencao de backups..."
    find "${BACKUP_DESTINATION}" -name "backup_chat_*.gpg*" -mtime +"${BACKUP_RETENTION_DAYS:-30}" -delete
    
    # Notifica sucesso
    FILE_SIZE=$(du -sh "${BACKUP_DESTINATION}/${BACKUP_FILENAME}.gpg" | cut -f1)
    notify_mattermost "[OK] Backup do CDC Chat concluido com sucesso!\n- Arquivo: \`${BACKUP_FILENAME}.gpg\`\n- Tamanho: \`${FILE_SIZE}\`\n- Destino: \`${BACKUP_DESTINATION}\`"
else
    echo "[ERROR] Arquivo de passphrase do GPG nao encontrado. Criptografia falhou."
    notify_mattermost "[CRITICAL] Backup do CDC Chat falhou: erro na etapa de criptografia GPG."
    exit 1
fi

echo "[INFO] Processo de backup finalizado com sucesso."
```

---

## Arquivo de Configuração do Backup

O script de backup lê diretamente o arquivo `.env` do diretório do projeto. Seguem as variáveis necessárias para a execução:
```env
BACKUP_PROJECT_NAME=cdc-chat
BACKUP_ENVIRONMENT=production
BACKUP_DESTINATION=/home/vier/backups/chat
BACKUP_RETENTION_DAYS=30
GPG_PASSPHRASE_FILE=/home/vier/.gpg_passphrase
```

---

## Segredos e Criptografia

- **Passphrase do GPG:** A chave simétrica para criptografar os backups deve ser salva em `/home/vier/.gpg_passphrase` no host (fora do diretório versionado pelo Git).
- **Proteção do Arquivo de Passphrase:** O arquivo deve pertencer exclusivamente ao usuário executor do script e ter permissões restritas:
  ```bash
  chmod 600 /home/vier/.gpg_passphrase
  ```

---

## Alertas no Mattermost (Tipos e Formato)

Os alertas ajudam a identificar falhas operacionais rápidas. Formato padrão enviado:

- **Alerta de Falha Crítica:**
  ```text
  [CRITICAL] Falha na execução de backup do CDC Chat!
  Ambiente: Produção
  Etapa: Criptografia GPG dos arquivos compactados.
  Status: Arquivo de passphrase não localizado na VPS.
  ```
- **Alerta de Sucesso:**
  ```text
  [OK] Backup do CDC Chat concluido com sucesso!
  - Arquivo: backup_chat_20260713_030000.tar.gz.gpg
  - Tamanho: 4.2GB
  - Destino: /home/vier/backups/chat
  ```

---

## Roteiro de Restauração (Restore Guide)

Caso ocorra um desastre no Mattermost, siga estes passos na VPS:

### Passo 1: Preparar ambiente limpo
Suba a infraestrutura Docker limpa do Mattermost (conforme `docker-compose.yml` da raiz).

### Passo 2: Localizar e descriptografar o backup
1. Identifique o backup desejado e execute a descriptografia informando a passphrase:
   ```bash
   gpg --batch --passphrase-file /home/vier/.gpg_passphrase \
       -d -o backup_recuperado.tar.gz /home/vier/backups/chat/backup_chat_20260713_030000.tar.gz.gpg
   ```
2. Descompacte o pacote descriptografado:
   ```bash
   tar -xvzf backup_recuperado.tar.gz
   ```

### Passo 3: Restaurar arquivos lógicos e anexos
Substitua as pastas locais do projeto pelas pastas descompactadas:
```bash
cp -Rp data/data/* ./data/data/
cp -Rp data/config/* ./data/config/
```

### Passo 4: Restaurar o Banco de Dados PostgreSQL
1. Pare o contêiner do Mattermost para evitar novas conexões:
   ```bash
   docker compose stop mattermost
   ```
2. Mova o arquivo de backup de banco para a pasta montada do banco PostgreSQL:
   ```bash
   mv mattermost.backup ./data/db/mattermost.backup
   ```
3. Execute a restauração do schema e dados usando o `pg_restore`:
   ```bash
   docker compose exec db pg_restore -U mmuser -d mattermost --clean /var/lib/postgresql/data/mattermost_temp.backup
   ```
4. Limpe o arquivo de backup temporário:
   ```bash
   rm -f ./data/db/mattermost.backup
   ```

### Passo 5: Inicializar serviços
1. Suba novamente o contêiner do Mattermost:
   ```bash
   docker compose start mattermost
   ```
2. Verifique os logs para garantir o restabelecimento:
   ```bash
   docker compose logs -f mattermost
   ```

---

## Validação pós-restore

- [ ] **Acesso:** Faça login na web com uma conta de teste.
- [ ] **Mídias:** Verifique se as imagens antigas em canais carregam corretamente.
- [ ] **Integridade:** Execute uma contagem de posts nas últimas 24h para verificar perda.
- [ ] **Logs:** Acompanhe `docker compose logs` por 5 minutos para validar.

---

## Teste periódico de restauração

Realize testes de restore trimestralmente em ambientes isolados (Sandbox) e registre abaixo:

| Data do Teste | Backup Testado | Tempo de Restore | Status | Responsável |
| :--- | :--- | :--- | :--- | :--- |
| `AAAA-MM-DD` | `backup_chat_AAAAMMDD.gpg` | `X min` | `[ ] Sucesso | [ ] Falha` | `Nome` |

---

Última revisão: 2026-07-13  
Responsável pela revisão: Antigravity  
Motivo da revisão: Inicialização da política de backup e restore do CDC Chat  
