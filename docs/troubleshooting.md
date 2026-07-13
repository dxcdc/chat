# Manual de Troubleshooting (CDC Chat)

Este documento contém guias de diagnóstico rápidos e procedimentos de mitigação para problemas recorrentes encontrados na operação do CDC Chat (Mattermost).

---

## 1. Categoria: Contêineres e Docker

### Sintoma A: Contêiner `chat-app` ou `chat-db` reiniciando em loop (Restarting)
- **Possível Causa:** Falta de espaço em disco na VPS, variáveis de ambiente configuradas de forma errada no `.env` ou volume persistente corrompido.
- **Diagnóstico:**
  1. Verifique o status detalhado dos contêineres:
     ```bash
     docker compose ps
     ```
  2. Leia os logs de erro do contêiner que está reiniciando:
     ```bash
     docker compose logs --tail=100 chat
     ```
- **Correção:**
  - Se os logs mostrarem `no space left on device`: limpe caches órfãos do Docker com `docker system prune -a --volumes` ou aumente o disco da VPS.
  - Se for erro de conexão de banco: verifique se a variável `DB_PASS` no `.env` coincide com a senha configurada no banco.

### Sintoma B: Porta `8065` já em uso (Port Collision)
- **Possível Causa:** Outro serviço na VPS está utilizando a porta do Mattermost.
- **Diagnóstico:**
  ```bash
  sudo ss -tulpn | grep 8065
  ```
- **Correção:**
  - Pare o serviço conflitante ou altere a porta externa `APP_PORT` no arquivo `.env` para uma porta livre (ex: `8066`) e reinicie o Docker Compose:
    ```bash
    docker compose down
    docker compose up -d
    ```

---

## 2. Categoria: Permissões de Acesso e Escrita

### Sintoma A: Erro de escrita em logs ou uploads (`Permission Denied` / `chown`)
- **Possível Causa:** O contêiner do Mattermost roda sob um usuário interno com UID/GID `2000` e não possui permissão de escrita nas pastas montadas do host.
- **Diagnóstico:**
  Verifique as permissões físicas dos diretórios locais:
  ```bash
  ls -la ./data/
  ```
- **Correção (Evite usar `chmod 777`):**
  Ajuste o proprietário das pastas locais para o UID `2000` (utilizado internamente pelo contêiner do Mattermost):
  ```bash
  sudo chown -R 2000:2000 ./data/config ./data/data ./data/logs ./data/plugins
  sudo chmod -R 700 ./data/config ./data/data ./data/logs ./data/plugins
  ```

---

## 3. Categoria: Banco de Dados (PostgreSQL)

### Sintoma A: Mattermost travado no boot mostrando `Waiting for Database` ou `Failed to ping database`
- **Possível Causa:** O banco de dados PostgreSQL ainda está inicializando ou a URL de conexão está inválida.
- **Diagnóstico:**
  1. Verifique se o contêiner `chat-db` está ativo e saudável:
     ```bash
     docker compose ps db
     ```
  2. Valide o ping interno de rede:
     ```bash
     docker compose exec mattermost ping db -c 3
     ```
- **Correção:**
  - Certifique-se de que a variável `MM_SQLSETTINGS_DATASOURCE` no Docker Compose está usando o formato correto: `postgres://mmuser:${DB_PASS}@db:5432/mattermost?sslmode=disable&connect_timeout=10`.
  - Verifique os logs do banco de dados:
    ```bash
    docker compose logs db
    ```

### Sintoma B: Conexões simultâneas esgotadas no PostgreSQL
- **Possível Causa:** O número de conexões ativas ultrapassou o limite do banco.
- **Diagnóstico:**
  ```bash
  docker compose exec db psql -U mmuser -d mattermost -c "SELECT count(*) FROM pg_stat_activity;"
  ```
- **Correção:**
  - Aumente o parâmetro `max_connections` no PostgreSQL ou reinicie o banco para liberar conexões presas:
    ```bash
    docker compose restart db
    ```

---

## 4. Categoria: Interface e WebSocket (Nginx Proxy)

### Sintoma A: Usuários recebem alerta "Espere um momento..." ou "Connecting..." no topo do app e mensagens não chegam em tempo real
- **Possível Causa:** O Proxy Reverso Nginx no host não está configurado para encaminhar requisições WebSocket (Upgrade de conexões TCP), fazendo com que o chat caia em fallback HTTP lento.
- **Diagnóstico:**
  Verifique as ferramentas de desenvolvedor do navegador (F12 > Console) para erros de WebSocket (`WebSocket connection to 'wss://chat...' failed`).
- **Correção:**
  Garanta que a configuração do bloco do servidor Nginx no host possui as diretivas de WebSocket ativas:
  ```nginx
  server {
      server_name chat.<DOMINIO_DO_PROJETO>;

      location / {
          proxy_pass http://localhost:8065;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "upgrade";
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
          proxy_set_header X-Frame-Options SAMEORIGIN;
          proxy_buffers 256 16k;
          proxy_buffer_size 16k;
          client_max_body_size 50M;
      }
  }
  ```
  Recarregue o Nginx:
  ```bash
  sudo nginx -s reload
  ```

---

## 5. Categoria: Envio de E-mail (SMTP)

### Sintoma A: Usuários não recebem e-mails de convite ou redefinição de senha
- **Possível Causa:** Credenciais SMTP incorretas, porta bloqueada pela VPS ou criptografia incompatível.
- **Diagnóstico:**
  1. Teste a conexão SMTP a partir da VPS usando o utilitário `nc` ou `telnet`:
     ```bash
     nc -zv <SERVIDOR_SMTP> 587
     ```
  2. Verifique os logs de e-mail do contêiner:
     ```bash
     docker compose logs chat | grep -i smtp
     ```
- **Correção:**
  - Garanta que a criptografia está mapeada corretamente: `starttls` para a porta `587` ou `tls` para a porta `465`.
  - Verifique se a variável `SMTP_PASSWORD` no arquivo `.env` está atualizada.

---

## 6. Categoria: Integração e Notificações (Mattermost)

### Sintoma A: Alertas de backup não chegam no canal
- **Possível Causa:** Webhook desativado, URL inválida ou timeout de rede.
- **Diagnóstico:**
  Dispare um teste manual usando o curl:
  ```bash
  export $(grep -v '^#' .env | xargs)
  curl --fail --silent --show-error --max-time "${MATTERMOST_TIMEOUT_SECONDS:-10}" \
    -X POST -H "Content-Type: application/json" \
    -d '{"text":"[DIAGNOSTICO] Teste de envio de webhook manual."}' \
    "$MATTERMOST_WEBHOOK_URL"
  ```
- **Tabela de Respostas do Webhook:**
  - **HTTP 400 (Bad Request):** JSON de envio formatado incorretamente.
  - **HTTP 404 (Not Found):** URL do webhook expirou ou o canal foi deletado.
  - **Timeout:** Bloqueio de firewall de saída na VPS.

---

## 7. Checklist de Emergência (VPS travada)

Se o CDC Chat ficar inoperante repentinamente, execute esta sequência rápida de análise:

1. **Disco:** Verifique espaço livre com `df -h`. Se o disco estiver 100% ocupado, execute `docker system prune -a --volumes` para liberar espaço imediato de imagens antigas.
2. **Memória:** Execute `free -m`. Se a memória swap estiver esgotada, identifique processos vazando memória.
3. **Contêineres:** Verifique se os contêineres estão no ar: `docker compose ps`.
4. **Logs Recentes:** Leia as últimas linhas de log buscando erros críticos:
   ```bash
   docker compose logs --tail=100 chat
   ```
5. **Logs do PostgreSQL:** Verifique se há corrupção ou travamentos no banco:
   ```bash
   docker compose logs --tail=100 db
   ```
6. **Portas VPS:** Garanta que a porta `8065` responde localmente: `curl -I http://localhost:8065`.
7. **DNS e TLS:** Valide se o domínio responde à internet e o SSL não expirou.
8. **Comunicar Time:** Notifique no canal alternativo de suporte a falha e informe a estimativa de retorno do chat.

---

Última revisão: 2026-07-13  
Responsável pela revisão: Antigravity  
Motivo da revisão: Inicialização do manual de troubleshooting para o CDC Chat  
