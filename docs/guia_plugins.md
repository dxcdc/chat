# Guia de Plugins e Extensões (CDC Chat)

Este documento centraliza a análise técnica, priorização, status de instalação, links oficiais e roteiros práticos dos plugins para o ecossistema do CDC Chat (Mattermost).

---

## 📌 Resumo Lógico de Status e Prioridades

| Plugin | Status Atual | Prioridade | Finalidade Principal | Link de Download / Repositório |
| :--- | :---: | :---: | :--- | :--- |
| **Playbooks** | 🟢 **Instalado/Ativo** | **Muito Alta** | Processos padronizados e checklists interativos | *Pré-instalado nativamente* |
| **Calls** | 🟢 **Instalado/Ativo** | **Muito Alta** | Chamadas de voz e tela nativas no chat | *Pré-instalado nativamente* |
| **Matterpoll (v1.8.0)** | 🟢 **Instalado/Ativo** | **Alta** | Criar enquetes e votações rápidas com `/poll` | [Matterpoll Releases](https://github.com/matterpoll/matterpoll/releases) |
| **Badges (v0.2.1)** | 🟢 **Instalado/Ativo** | **Alta** | Conceder selos de reconhecimento entre pares | [Badges Releases](https://github.com/larkox/mattermost-plugin-badges/releases) |
| **GitHub (v2.7.1)** | 🟢 **Instalado/Pronto** | **Alta** | Integrar repositórios, PRs e issues no chat | [GitHub Releases](https://github.com/mattermost/mattermost-plugin-github/releases) |
| **Boards (v8.0.0)** | 🟡 **Baixado (Pronto)** | **Alta** | Gestão visual de projetos e tarefas (Kanban) | [Boards Releases](https://github.com/mattermost/focalboard/releases) |
| **WelcomeBot (v1.4.1)** | 🟡 **Baixado (Pronto)** | **Alta** | Recepção e onboarding automático de membros | [WelcomeBot Releases](https://github.com/mattermost/mattermost-plugin-welcomebot/releases) |
| **Autolink (v1.4.1)** | 🟡 **Baixado (Pronto)** | **Alta** | Conversão de tags de texto em links clicáveis | [Autolink Releases](https://github.com/mattermost/mattermost-plugin-autolink/releases) |
| **Giphy (v5.0.0)** | 🟡 **Baixado (Pronto)** | **Média** | Busca e envio de GIFs animados no chat | [Giphy Releases](https://github.com/moussetc/mattermost-plugin-giphy/releases) |
| **Google Calendar (v1.5.0)** | 🟡 **Baixado (Pronto)** | **Média** | Centralização de agendas e notificações | [Google Calendar Releases](https://github.com/mattermost/mattermost-plugin-google-calendar/releases) |
| **Timeline (v1.5.1)** | 🟡 **Baixado (Piloto)** | **Média** | Linha do tempo lateral para logs de webhooks | [Timeline Releases](https://github.com/icoretech/mattermost-timeline/releases) |
| **Mattermost AI (v2.5.0)** | 🟡 **Baixado (Piloto)** | **Baixa** | Inteligência Artificial e buscas semânticas locais | [Agents Releases](https://github.com/mattermost/mattermost-plugin-agents/releases) |
| **Voice Messages (v0.1.4)** | 🟡 **Baixado (Piloto)** | **Baixa** | Mensagens de voz com transcrição integrada | [Voice Messages Releases](https://github.com/icoretech/mattermost-voice-messages/releases) |

---

## ⚡ 1. Plugins Instalados e em Operação

### 1.1. Playbooks (Processos e Checklists) — 🟢 `INSTALADO E ATIVO`
*   **Status:** 🟢 **Instalado e Ativo** (Pré-instalado nativamente no Mattermost).
*   **Funcionalidades:** Cria procedimentos repetíveis com etapas, responsáveis, prazos definidos e acompanhamento centralizado.
*   **Exemplo Prático (Onboarding de Novo Colaborador):**
    - [x] Criar e-mail institucional
    - [x] Adicionar ao Mattermost
    - [ ] Conceder acesso ao ONGSYS
    - [ ] Apresentar a Wiki
    - [ ] Realizar treinamento inicial

---

### 1.2. Matterpoll (Enquetes e Votações - v1.8.0) — 🟢 `INSTALADO E ATIVO`
*   **Status:** 🟢 **Instalado e Ativo** (`com.github.matterpoll.matterpoll-1.8.0.tar.gz`).
*   **Repositório Oficial:** [matterpoll/matterpoll](https://github.com/matterpoll/matterpoll)
*   **Comando:** `/poll "Pergunta?" "Opção 1" "Opção 2"`

---

### 1.3. Badges / Selos de Reconhecimento (v0.2.1) — 🟢 `INSTALADO E ATIVO`
*   **Status:** 🟢 **Instalado e Ativo** (`com.mattermost.badges-0.2.1.tar.gz`).
*   **Repositório Oficial:** [larkox/mattermost-plugin-badges](https://github.com/larkox/mattermost-plugin-badges)
*   **Comando:** `/badges grant @usuario "Nome do Selo"`

---

### 1.4. GitHub Integration (v2.7.1 Linux) — 🟢 `INSTALADO E PRONTO`
*   **Status:** 🟢 **Instalado / Pronto** (`mattermost-plugin-github-v2.7.1-linux-amd64.tar.gz`).
*   **Repositório Oficial:** [mattermost/mattermost-plugin-github](https://github.com/mattermost/mattermost-plugin-github)

---

## 🚀 2. Plugins Baixados (Prontos para Upload no Painel)

### 2.1. Boards / Focalboard (v8.0.0 Linux) — 🟡 `BAIXADO (PRONTO)`
*   **Status:** 🟡 **Baixado** (`mattermost-plugin-focalboard-v8.0.0-linux-amd64.tar.gz`).
*   **Repositório Oficial:** [mattermost/focalboard](https://github.com/mattermost/focalboard)
*   **Funcionalidades:** Gerenciador visual de tarefas no estilo Kanban integrado ao painel.

---

### 2.2. WelcomeBot (v1.4.1) — 🟡 `BAIXADO (PRONTO)`
*   **Status:** 🟡 **Baixado** (`com.mattermost.welcomebot-1.4.1.tar.gz`).
*   **Repositório Oficial:** [mattermost/mattermost-plugin-welcomebot](https://github.com/mattermost/mattermost-plugin-welcomebot)
*   **Funcionalidades:** Boas-vindas automáticas e direcionamento de novos usuários.

---

### 2.3. Autolink (v1.4.1) — 🟡 `BAIXADO (PRONTO)`
*   **Status:** 🟡 **Baixado** (`mattermost-autolink-1.4.1.tar.gz`).
*   **Repositório Oficial:** [mattermost/mattermost-plugin-autolink](https://github.com/mattermost/mattermost-plugin-autolink)
*   **Funcionalidades:** Converte códigos digitados (ex: `ONGSYS-2623`, `WIKI-145`) em links diretos.

---

### 2.4. Giphy (v5.0.0) — 🟡 `BAIXADO (PRONTO)`
*   **Status:** 🟡 **Baixado** (`com.github.moussetc.mattermost.plugin.giphy-5.0.0.tar.gz`).
*   **Repositório Oficial:** [moussetc/mattermost-plugin-giphy](https://github.com/moussetc/mattermost-plugin-giphy)
*   **Funcionalidades:** Envio de GIFs animados nos canais com `/giphy`.

---

### 2.5. Google Calendar (v1.5.0 Linux) — 🟡 `BAIXADO (PRONTO)`
*   **Status:** 🟡 **Baixado** (`mattermost-plugin-google-calendar-v1.5.0-linux-amd64.tar.gz`).
*   **Repositório Oficial:** [mattermost/mattermost-plugin-google-calendar](https://github.com/mattermost/mattermost-plugin-google-calendar)
*   **Funcionalidades:** Notificações de reuniões e sincronização da agenda institucional.

---

## 🧪 3. Plugins de Laboratório Baixados

### 3.1. Timeline (v1.5.1) — 🟡 `BAIXADO (PILOTO)`
*   **Status:** 🟡 **Baixado** (`ch.icorete.mattermost-timeline-1.5.1.tar.gz`).
*   **Repositório Oficial:** [icoretech/mattermost-timeline](https://github.com/icoretech/mattermost-timeline)

---

### 3.2. Mattermost AI / Agents (v2.5.0) — 🟡 `BAIXADO (PILOTO)`
*   **Status:** 🟡 **Baixado** (`mattermost-ai-2.5.0-rc2+076aa4e7.tar.gz`).
*   **Repositório Oficial:** [mattermost/mattermost-plugin-agents](https://github.com/mattermost/mattermost-plugin-agents)

---

### 3.3. Voice Messages (v0.1.4) — 🟡 `BAIXADO (PILOTO)`
*   **Status:** 🟡 **Baixado** (`ch.icorete.mattermost-voice-messages-0.1.4.tar.gz`).
*   **Repositório Oficial:** [icoretech/mattermost-voice-messages](https://github.com/icoretech/mattermost-voice-messages)

---

Última revisão: 2026-07-23  
Responsável pela revisão: Antigravity  
Motivo da revisão: Atualização do guia com a confirmação de todos os pacotes .tar.gz baixados e prontos para upload  
