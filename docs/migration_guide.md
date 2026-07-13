# Guia de Migração e Acesso Seguro (CDC Chat)

Este documento orienta a equipe de infraestrutura no acesso SSH seguro, diagnósticos rápidos de servidores VPS e no procedimento passo a passo de migração de ambiente para o CDC Chat (Mattermost).

---

## Acesso SSH seguro

O acesso ao servidor VPS que hospeda o CDC Chat deve ser feito exclusivamente por autenticação de chaves criptográficas de segurança alta.

### 1. Geração da chave ED25519
No seu computador local, gere um par de chaves usando o algoritmo ED25519 (protegido obrigatoriamente por uma senha forte/passphrase):
```bash
ssh-keygen -t ed25519 -C "admin@dxcdc.org"
```

### 2. Cópia da chave pública para a VPS
Envie a chave pública para o arquivo `authorized_keys` da VPS de destino:
```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub -p <SSH_PORT> <SSH_USER>@<SSH_HOST>
```

### 3. Configuração de atalhos locais (`~/.ssh/config`)
Simplifique o acesso configurando o atalho em seu arquivo local `~/.ssh/config`:
```text
Host cdc-chat
    HostName <SSH_HOST>
    User <SSH_USER>
    Port <SSH_PORT>
    IdentityFile ~/.ssh/id_ed25519
```
Após configurar, conecte-se usando apenas:
```bash
ssh cdc-chat
```

### 4. Recomendação de segurança da VPS (`/etc/ssh/sshd_config`)
Para blindar o servidor contra ataques de força bruta, altere o arquivo de configuração do SSH na VPS:
- Desative login por senha: `PasswordAuthentication no`
- Desative login direto do root: `PermitRootLogin no`
- Mude a porta padrão do SSH (porta 22) para uma porta alta customizada.
- Reinicie o serviço:
```bash
sudo systemctl restart sshd
```

---

## Diagnóstico em modo somente leitura

Antes de realizar qualquer intervenção ou migração, execute os comandos não destrutivos abaixo para inventariar o status da VPS:

### 1. Sistema e Recursos
- **Sistema Operacional:** `uname -a` ou `cat /etc/os-release`
- **Uso de Memória:** `free -h`
- **Uso de CPU e Processos:** `top -b -n 1 | head -n 30` ou `htop` (se instalado)
- **Espaço em Disco:** `df -h`
- **Uso de Inodes (Esgotamento de arquivos pequenos):** `df -i`

### 2. Rede e Portas
- **Portas Abertas e Conexões:** `sudo ss -tulpn`
- **Serviços Ativos:** `sudo systemctl list-units --type=service --state=running`
- **Resolução DNS Externa:** `nslookup github.com`
- **Conectividade de Rede de Saída com o Mattermost:**
  `curl -I --max-time 10 https://mattermost.com`

### 3. Docker e Serviços
- **Contêineres Ativos e Status de Saúde:** `docker compose ps` ou `docker ps -a`
- **Redes Docker Existentes:** `docker network ls`
- **Volumes Persistentes Docker:** `docker volume ls`
- **Logs Recentes do Mattermost:** `docker compose logs --tail=100 chat`
- **Logs Recentes do PostgreSQL:** `docker compose logs --tail=100 db`

---

## Preparação para migração

Antes da migração do Mattermost para uma nova VPS, cumpra o seguinte checklist de preparação:
- [ ] Anunciar a janela de manutenção com 48h de antecedência.
- [ ] Bloquear novos cadastros ou postagens (congelamento lógico) configurando o Mattermost em modo de manutenção ou alterando as permissões de rede.
- [ ] Verificar se a VPS de destino possui os requisitos mínimos de recursos instalados (Docker, Docker Compose, Nginx).
- [ ] Certificar que há espaço em disco na origem para gerar o dump do banco de dados e compactação dos arquivos.
- [ ] Confirmar que o webhook de alertas de migração está configurado.

---

## Exportação do banco de dados

O Mattermost do CDC Chat utiliza PostgreSQL. Execute a exportação de forma segura sem expor a senha no console bash:

1. Carregue as variáveis de ambiente em memória:
   ```bash
   export $(grep -v '^#' .env | xargs)
   ```
2. Realize o dump do banco PostgreSQL de dentro do container diretamente para um arquivo gzip no host:
   ```bash
   docker compose exec -e PGPASSWORD="$DB_PASS" db pg_dump -U "$DB_USER" -d "$DB_NAME" -h localhost -F c -b | gzip > "db_chat_$(date +%Y%m%d_%H%M%S).sql.gz"
   ```

---

## Compactação e transferência

O Mattermost possui volumes pesados de mídia (arquivos enviados em canais). Transfira os dados usando compressão e sincronização eficiente:

### 1. Compactação de Arquivos e Dumps
Compacte a pasta de anexo e os arquivos do Mattermost de forma estruturada:
```bash
tar -czf files_chat_$(date +%Y%m%d_%H%M%S).tar.gz ./data/data docker-compose.yml .env.example
```

### 2. Geração de Checksum SHA-256
Gere a assinatura hash dos arquivos compactados para assegurar que não haja corrupção durante a transferência:
```bash
sha256sum db_chat_*.sql.gz files_chat_*.tar.gz > manifest_checksums.txt
```

### 3. Transferência segura (rsync)
Utilize `rsync` para transferir os dados da VPS antiga (origem) para a nova (destino) preservando permissões e exibindo progresso:
```bash
rsync -avzP -e "ssh -p <SSH_PORT>" db_chat_*.sql.gz files_chat_*.tar.gz manifest_checksums.txt <SSH_USER>@<SSH_HOST>:/home/vier/migration/
```

### 4. Validação na Nova VPS
Após a conclusão da transferência, acesse a nova VPS e valide se os arquivos chegaram intactos:
```bash
sha256sum -c manifest_checksums.txt
```

---

## Comunicação da migração (Mattermost)

Utilize estes modelos estruturados para comunicação durante o processo:

### Início da janela
```text
[MIGRAÇÃO EM ANDAMENTO] - CDC Chat (Mattermost)
Ambiente: VPS Produção
Janela: Início do backup e desligamento lógico dos serviços. O chat ficará temporariamente indisponível.
```

### Conclusão e Sucesso
```text
[MIGRAÇÃO CONCLUÍDA] - CDC Chat (Mattermost)
Ambiente: VPS Produção
Status: Todos os dados transferidos e validados na nova VPS. Serviço online em https://chat.<DOMINIO_DO_PROJETO>.
```

---

## Validação pós-migração

Execute as validações obrigatórias antes de liberar o sistema para uso geral:
- [ ] **Check de Contêineres:** Execute `docker compose ps` na nova VPS e garanta que todos os contêineres estão `healthy`.
- [ ] **Restauração de Banco:** Verifique se as contagens de posts e usuários batem com os números da VPS antiga.
- [ ] **Permissões de Arquivos:** Garanta que `./data/data` possui permissões de escrita para o container Mattermost (`chown -R 2000:2000 ./data`).
- [ ] **Teste de Upload:** Acesse o Mattermost com uma conta de teste, envie uma mensagem e faça upload de uma imagem em um canal privado.
- [ ] **Conexão e Certificados:** Verifique se o acesso HTTPS via Nginx Proxy Manager está ativo e sem avisos de certificado TLS inválido.
- [ ] **Logs:** Acompanhe `docker compose logs -f` por 10 minutos para certificar que o Mattermost não está gerando exceptions em loop.

---

Última revisão: 2026-07-13  
Responsável pela revisão: Antigravity  
Motivo da revisão: Inicialização do guia de migração e acessos para o Mattermost  
