# Especificação Técnica: Desenvolvimento de Plugin de Calendário Customizado (CDC Chat)

Este documento especifica a arquitetura, funcionalidades, decisões de design e guia de compilação para o desenvolvimento de um **plugin customizado de calendário** no Mattermost, criado sob medida para atender às necessidades operacionais e de governança da CDC.

---

## 📌 1. Visão Geral e Contexto

### O Problema do Plugin Oficial
O plugin oficial do Google Calendar para o Mattermost possui limitações rígidas para cenários institucionais:
- Conecta-se estritamente à agenda **`primary`** do usuário autenticado.
- Não possui comandos para selecionar agendas secundárias por ID (ex: `abc123@group.calendar.google.com`).
- A autenticação é individual por conta de usuário, em vez de permitir uma conexão centralizada por organização ou vincular agendas específicas a canais específicos.

### O Objetivo do Plugin Customizado (`cdc-calendar`)
Criar uma extensão leve em **Go** (Backend) e **React/TypeScript** (Frontend) que permita:
1. Vincular **qualquer ID de agenda** (principal ou secundária) a **qualquer canal do Mattermost**.
2. Manter uma **autenticação centralizada de serviço** (OAuth2 de Organização ou Service Account).
3. Sanitizar automaticamente dados sensíveis (remover e-mails de convidados e links privados) antes de publicar notificações em canais públicos.

---

## 🏗️ 2. Arquitetura Técnica

O plugin utiliza a estrutura oficial de desenvolvimento de plugins do Mattermost (`mattermost-plugin-starter-template`):

```text
cdc-calendar/
├── plugin.json                 # Manifesto do plugin (ID, versão, permissões)
├── server/                     # Backend em Go (API, KV Store, Cron Worker, Google API)
│   ├── main.go                 # Ponto de entrada do plugin
│   ├── plugin.go               # Implementação da interface plugin.MattermostPlugin
│   ├── command.go              # Registrador dos comandos de barra (/cdc-cal)
│   ├── google.go               # Cliente de integração com a Google Calendar API
│   └── store.go                # Abstração de persistência no KV Store do Mattermost
└── webapp/                     # Frontend em React/TypeScript (Barra lateral, botões, modais)
    ├── src/
    │   ├── index.tsx           # Registro dos componentes da UI
    │   └── components/         # Painéis e formulários do canal
    └── package.json
```

---

## ⚡ 3. Funcionalidades e Comandos de Barra (`/cdc-cal`)

O plugin registrará o comando raiz `/cdc-cal` com os seguintes subcomandos:

| Comando | Parâmetros | Descrição | Permissão |
| :--- | :--- | :--- | :---: |
| `/cdc-cal bind` | `<CALENDAR_ID>` | Vincula o ID de uma agenda do Google ao canal atual | Admin de Canal / SysAdmin |
| `/cdc-cal unbind` | — | Remove o vínculo de agenda do canal atual | Admin de Canal / SysAdmin |
| `/cdc-cal list` | — | Lista as agendas vinculadas aos canais da equipe | Todos |
| `/cdc-cal today` | — | Exibe instantaneamente no chat os eventos de hoje da agenda do canal | Todos |
| `/cdc-cal summary` | `[HH:MM]` | Configura o envio do resumo diário no canal no horário especificado | Admin de Canal |
| `/cdc-cal sync` | — | Força a sincronização imediata dos eventos do Google | Admin de Canal |

---

## 💾 4. Estrutura de Dados (KV Store do Mattermost)

Para não depender de tabelas externas no banco de dados, o plugin armazena as configurações de vínculo no **KV Store** (Key-Value Store) nativo do Mattermost:

### Chave de Vínculo por Canal: `channel_subscription_{ChannelID}`
```json
{
  "channel_id": "7x9a8b7c6d5e4f3a2b1c0d",
  "calendar_id": "c_10c3af37b1f55658fb5d8738d414ab900e0495fa2b4ec06301cd9b684b1460b6@group.calendar.google.com",
  "calendar_name": "Aniversários CDC",
  "summary_enabled": true,
  "summary_time": "08:00",
  "reminder_minutes_before": 15,
  "sanitize_private_data": true,
  "created_by": "user_id_admin",
  "updated_at": 1784770000
}
```

---

## 🔐 5. Autenticação Centralizada e Governança

### Fluxo de Autenticação Única
Em vez de cada usuário precisar autorizar sua conta pessoal:
1. O **Administrador do Sistema** conecta uma conta de serviço ou conta institucional no painel de configurações do plugin em **Console do Sistema ➔ Plugins ➔ CDC Calendar**.
2. As credenciais (OAuth2 Tokens ou Service Account Credentials JSON) são salvas criptografadas no KV Store global do plugin.
3. Todas as consultas de agendas para os canais utilizam esse token de serviço com acesso de leitura às agendas da organização.

### Regras de Sanitização de Privacidade
Ao formatar os cards de mensagens que serão publicados nos canais públicos do Mattermost, a função de sanitização em Go executa as seguintes limpezas:
- **Descrições:** Oculta descrições longas contendo senhas ou notas internas, exibindo apenas o título e horário.
- **Convidados:** Remove a exibição dos endereços de e-mail de convidados externos para proteção de dados (LGPD).
- **Links de Reunião:** Mantém apenas os links oficiais do Google Meet ou Jitsi.

---

## 🔨 6. Guia de Desenvolvimento e Build

### Pré-requisitos
- **Go** v1.21+ instalado.
- **Node.js** v18+ e **npm** v9+ instalados.
- **Docker** e ambiente de dev do Mattermost.

### Passos para Compilar o Plugin (`.tar.gz`)

1. Clonar o repositório base:
   ```bash
   git clone git@github.com:dxcdc/mattermost-plugin-cdc-calendar.git
   cd mattermost-plugin-cdc-calendar
   ```

2. Instalar dependências do Frontend (webapp):
   ```bash
   cd webapp && npm install && cd ..
   ```

3. Gerar o pacote de distribuição do plugin:
   ```bash
   make dist
   ```

4. O resultado será o arquivo compactado em:
   ```text
   dist/com.cdc.mattermost-plugin-calendar-1.0.0.tar.gz
   ```

5. Faça o upload do arquivo gerado no Mattermost em **Console do Sistema ➔ Plugins ➔ Gerenciamento de Plugin**.

---

Última revisão: 2026-07-23  
Responsável pela revisão: Antigravity  
Motivo da revisão: Criação da especificação técnica para desenvolvimento do plugin de calendário customizado CDC  
