# Guia Técnico de Infraestrutura (CDC Chat)

Este documento detalha o desenho técnico, arquitetura de rede, segurança e configurações lógicas do projeto CDC Chat (Mattermost).

---

## Arquitetura atual

A arquitetura do CDC Chat é executada sob contêineres Docker isolados, estruturada da seguinte forma:

- **Aplicação (Mattermost):** Plataforma web escrita em Go/React, rodando sob a imagem oficial `mattermost/mattermost-team-edition:9.11`.
- **Proxy Reverso (Host Externo):** Um servidor Nginx (ou Nginx Proxy Manager) rodando no host gerencia o tráfego externo HTTPS na porta 443, realiza o descarregamento SSL/TLS (Offloading) e encaminha o tráfego HTTP local para a porta 8065 do contêiner do Mattermost.
- **Banco de Dados:** PostgreSQL (versão `16.4-alpine`) para armazenamento persistente das tabelas de posts, canais e usuários.
- **Isolamento e Comunicação:** Contêineres compartilham uma rede de ponte Docker interna privada (`chat-net`).
- **Persistência física:** Os dados do PostgreSQL são salvos localmente em `./data/db`, as configurações em `./data/config`, e os anexos de conversas em `./data/data` no sistema de arquivos do host.

---

## Containers

Abaixo está o arquivo `docker-compose.yml` de referência completo do projeto:

```yaml
version: "3.8"

services:
  db:
    image: postgres:16.4-alpine
    container_name: chat-db
    restart: always
    environment:
      POSTGRES_USER: mmuser
      POSTGRES_PASSWORD: ${DB_PASS}
      POSTGRES_DB: mattermost
    volumes:
      - ./data/db:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U mmuser -d mattermost"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - chat-net

  mattermost:
    image: mattermost/mattermost-team-edition:9.11
    container_name: chat-app
    restart: always
    depends_on:
      db:
        condition: service_healthy
    environment:
      # Configuração do Banco
      MM_SQLSETTINGS_DRIVERNAME: postgres
      MM_SQLSETTINGS_DATASOURCE: postgres://mmuser:${DB_PASS}@db:5432/mattermost?sslmode=disable&connect_timeout=10
      # Configurações Gerais
      MM_SERVICESETTINGS_SITEURL: ${APP_URL}
      MM_SERVICESETTINGS_LISTENADDRESS: :8065
      # Integração de E-mail
      MM_EMAILSETTINGS_ENABLESMTPCONNECTION: "true"
      MM_EMAILSETTINGS_SMTPSERVER: ${SMTP_HOST}
      MM_EMAILSETTINGS_SMTPPORT: ${SMTP_PORT}
      MM_EMAILSETTINGS_SMTPUSERNAME: ${SMTP_USER}
      MM_EMAILSETTINGS_SMTPPASSWORD: ${SMTP_PASSWORD}
      MM_EMAILSETTINGS_CONNECTIONSECURITY: ${SMTP_ENCRYPTION}
      MM_EMAILSETTINGS_FEEDBACKNAME: ${SMTP_FROM_NAME}
      MM_EMAILSETTINGS_FEEDBACKEMAIL: ${SMTP_FROM_ADDRESS}
    ports:
      - "8065:8065"
    volumes:
      - ./data/config:/mattermost/config
      - ./data/data:/mattermost/data
      - ./data/logs:/mattermost/logs
      - ./data/plugins:/mattermost/plugins
    networks:
      - chat-net

networks:
  chat-net:
    driver: bridge
```

---

## Isolamento de rede

Para mitigar superfícies de ataque, implementamos o isolamento físico/virtual das portas:
- **Banco de Dados Isolado:** O PostgreSQL (`db`) **NÃO** expõe portas públicas no host (não possui a diretiva `ports` no docker-compose). Ele comunica-se exclusivamente com a aplicação Mattermost através da rede bridge interna `chat-net`.
- **Acesso Limitado:** Apenas o container `mattermost` expõe a porta `8065` localmente no host. O Proxy Reverso do host intercepta o domínio externo e redireciona localmente para a porta `8065`.

---

## `.env.example`

O arquivo `.env.example` define a estrutura necessária das variáveis de ambiente. Siga as diretrizes de segurança:
- Nunca comite o arquivo `.env` preenchido com dados reais.
- Mantenha o `.env` protegido com permissão mínima de leitura no host (`chmod 600 .env`).
- Execute a cópia inicial do template usando:
  ```bash
  cp .env.example .env
  ```

---

## Modelo genérico do `.env.example`

O template oficial do projeto está estruturado em `.env.example` na raiz:

```env
# Ambiente da aplicação
APP_ENV=development
APP_PORT=8065
APP_URL=http://localhost:8065
MATTERMOST_IMAGE_TAG=9.11

# Banco de dados
DB_HOST=db
DB_PORT=5432
DB_NAME=mattermost
DB_USER=mmuser
DB_PASS=<DB_PASSWORD>

# E-mail (Opcional no Mattermost)
SMTP_HOST=<SERVIDOR_SMTP>
SMTP_PORT=587
SMTP_USER=<USUARIO_SMTP>
SMTP_PASSWORD=<SMTP_PASSWORD>
SMTP_ENCRYPTION=starttls
SMTP_FROM_ADDRESS=<EMAIL_REMETENTE>
SMTP_FROM_NAME=<NOME_DO_PROJETO>

# Mattermost
MATTERMOST_ENABLED=false
MATTERMOST_WEBHOOK_URL=<MATTERMOST_WEBHOOK_URL>
MATTERMOST_CHANNEL=<CANAL_DE_ALERTAS>
MATTERMOST_USERNAME=ChatBot
MATTERMOST_ICON_URL=<URL_PUBLICA_DO_ICONE>
MATTERMOST_TIMEOUT_SECONDS=10
```

---

## Integração com Mattermost

A integração para envio de alertas opera com as seguintes especificações técnicas:
- **Proteção do Webhook:** A variável `MATTERMOST_WEBHOOK_URL` contém o hash privado de integração e deve ser tratada como segredo de segurança máxima. Nunca a adicione em arquivos Git, capturas de tela ou logs de execução.
- **Timeout e Retentativas:** Configurado com tempo limite de resposta de 10 segundos (`MATTERMOST_TIMEOUT_SECONDS=10`) para evitar que scripts operacionais ou de backup fiquem travados (hanging) caso o Mattermost apresente indisponibilidade.
- **Tolerância a Falhas:** Caso a notificação do Mattermost falhe ou o serviço esteja offline, a execução principal (ex: rotina de backup) **deve prosseguir** e gerar o backup local normalmente, apenas registrando o erro do webhook nos arquivos de log.

---

## Teste do Mattermost

Para verificar a conectividade do webhook do Mattermost a partir do host sem expor a URL no histórico do shell Bash:

1. Carregue as variáveis a partir do `.env`:
   ```bash
   export $(grep -v '^#' .env | xargs)
   ```
2. Dispare o comando de teste usando a variável:
   ```bash
   curl --fail --silent --show-error --max-time "${MATTERMOST_TIMEOUT_SECONDS:-10}" \
     -X POST -H "Content-Type: application/json" \
     -d '{"text":"[INFO] Teste de integracao do CDC Chat com o Mattermost realizado com sucesso."}' \
     "$MATTERMOST_WEBHOOK_URL"
   ```

---

## DNS e serviços externos

Abaixo estão os apontamentos de DNS necessários para a operação do CDC Chat na VPS:

| Tipo | Nome | Destino ou valor | TTL | Finalidade |
| :--- | :--- | :--- | :--- | :--- |
| `A` | `chat.<DOMINIO_DO_PROJETO>` | `<IP_DA_VPS_PRODUÇÃO>` | 3600 | Aponta o domínio público do Mattermost para a VPS. |
| `CNAME` | `www.chat.<DOMINIO_DO_PROJETO>` | `chat.<DOMINIO_DO_PROJETO>` | 3600 | Redirecionamento alternativo padrão de tráfego. |

---

## Portas

Abaixo está o mapeamento lógico e físico das portas do serviço:

| Serviço | Porta interna | Porta externa | Protocolo | Exposição |
| :--- | :--- | :--- | :--- | :--- |
| `chat-app` | 8065 | 8065 | TCP | Exposta localmente no host VPS. |
| `chat-db` | 5432 | - | TCP | Exclusiva da rede bridge interna `chat-net`. |
| Nginx (Host) | - | 80 / 443 | TCP | Acessível publicamente na Internet. |

---

## Inicialização e encerramento

Abaixo estão os comandos administrativos essenciais para gerenciamento dos containers:

- **Iniciar serviços em segundo plano (e compilar imagem local):**
  ```bash
  docker compose up -d
  ```
- **Parar os serviços (sem destruir volumes ou dados):**
  ```bash
  docker compose stop
  ```
- **Parar e remover os contêineres e redes associadas:**
  ```bash
  docker compose down
  ```
- **Visualizar status dos serviços e status de saúde (healthcheck):**
  ```bash
  docker compose ps
  ```
- **Acompanhar os logs do Mattermost em tempo real:**
  ```bash
  docker compose logs -f mattermost
  ```
- **Acompanhar os logs do PostgreSQL em tempo real:**
  ```bash
  docker compose logs -f db
  ```

---

Última revisão: 2026-07-13  
Responsável pela revisão: Antigravity  
Motivo da revisão: Inicialização do arquivo de ajuda de infraestrutura para o Mattermost  
