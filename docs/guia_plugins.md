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
| **GitHub (v2.7.1)** | 🟡 **Em Configuração** | **Alta** | Integrar repositórios, PRs e issues no chat | [GitHub Releases](https://github.com/mattermost/mattermost-plugin-github/releases) |
| **Boards (Kanban)** | ⚪ **Pendente** | **Alta** | Gestão visual de projetos e tarefas | [Boards Releases](https://github.com/mattermost/focalboard/releases) |
| **WelcomeBot** | ⚪ **Pendente** | **Alta** | Recepção e onboarding automático de membros | [WelcomeBot Releases](https://github.com/mattermost/mattermost-plugin-welcomebot/releases) |
| **Autolink** | ⚪ **Pendente** | **Alta** | Conversão de tags de texto em links clicáveis | [Autolink Releases](https://github.com/mattermost/mattermost-plugin-autolink/releases) |
| **Google Calendar** | ⚪ **Pendente** | **Média** | Centralização de agendas e notificações | [Google Calendar Releases](https://github.com/mattermost/mattermost-plugin-google-calendar/releases) |
| **Timeline** | 🧪 **Piloto** | **Média** | Linha do tempo lateral para logs de webhooks | [Timeline Releases](https://github.com/icoretech/mattermost-timeline/releases) |
| **Mattermost Agents** | 🧪 **Piloto** | **Baixa** | Inteligência Artificial e buscas semânticas locais | [Agents Releases](https://github.com/mattermost/mattermost-plugin-agents/releases) |
| **Voice Messages** | 🧪 **Piloto** | **Baixa** | Mensagens de voz com transcrição integrada | [Voice Messages Releases](https://github.com/icoretech/mattermost-voice-messages/releases) |

---

## ⚡ 1. Plugins Instalados e em Operação

### 1.1. Playbooks (Processos e Checklists) — 🟢 `INSTALADO E ATIVO`
*   **Status:** 🟢 **Instalado e Ativo** (Pré-instalado nativamente no Mattermost).
*   **Funcionalidades:** Cria procedimentos repetíveis com etapas, responsáveis, prazos definidos e acompanhamento centralizado. Permite acionar checklists automatizados a partir de eventos ou palavras-chave no chat.
*   **Por que escolher:** É a ferramenta com maior potencial de transformação interna. Padroniza rotinas manuais críticas em roteiros acionáveis para qualquer voluntário seguir.
*   **Exemplo Prático (Onboarding de Novo Colaborador):**
    *   Ao ativar o playbook de entrada de colaborador, a equipe de RH recebe a seguinte lista interativa:
        - [x] Criar e-mail institucional
        - [x] Adicionar ao Mattermost
        - [ ] Conceder acesso ao ONGSYS
        - [ ] Apresentar a Wiki
        - [ ] Realizar treinamento inicial

---

### 1.2. Matterpoll (Enquetes e Votações - v1.8.0) — 🟢 `INSTALADO E ATIVO`
*   **Status:** 🟢 **Instalado e Ativo** (Arquivo `com.github.matterpoll.matterpoll-1.8.0.tar.gz`).
*   **Repositório Oficial:** [matterpoll/matterpoll](https://github.com/matterpoll/matterpoll)
*   **Funcionalidades:** Adiciona o comando de barra `/poll` para criar enquetes interativas nos canais. Os usuários votam clicando em botões e o plugin calcula os resultados em tempo real.
*   **Parâmetros Avançados:**
    *   `/poll "Pergunta?" "Opção 1" "Opção 2" --anonymous` (Votação Anônima).
    *   `/poll "Escolha duas opções" "A" "B" "C" --votes=2` (Limita 2 votos por pessoa).
*   **Exemplo Prático:**
    ```text
    /poll "Onde deve ser nossa próxima reunião de voluntários?" "Na Sede da ONG" "Formato Online (Jitsi)" "Parque Municipal"
    ```

---

### 1.3. Badges / Selos de Reconhecimento (v0.2.1) — 🟢 `INSTALADO E ATIVO`
*   **Status:** 🟢 **Instalado e Ativo** (Arquivo `com.mattermost.badges-0.2.1.tar.gz`).
*   **Repositório Oficial:** [larkox/mattermost-plugin-badges](https://github.com/larkox/mattermost-plugin-badges)
*   **Funcionalidades:** Permite aos membros da equipe conceder selos e medalhas de agradecimento e reconhecimento entre si.
*   **Comandos:**
    *   `/badges grant @usuario "Nome do Selo"` ➔ Concede um selo de reconhecimento para um colega.
    *   `/badges my` ➔ Exibe a lista de selos acumulados no perfil do usuário.

---

### 1.4. GitHub Integration (v2.7.1 - Linux) — 🟡 `EM INSTALAÇÃO / CONFIGURAÇÃO`
*   **Status:** 🟡 **Baixado / Em Configuração** (Arquivo `mattermost-plugin-github-v2.7.1-linux-amd64.tar.gz`).
*   **Repositório Oficial:** [mattermost/mattermost-plugin-github](https://github.com/mattermost/mattermost-plugin-github)
*   **Funcionalidades:** Conecta os repositórios do GitHub aos canais. Permite receber avisos de Pull Requests, issues e commits, além de gerenciar tarefas pendentes com o comando `/github todo`.

---

## 🚀 2. Plugins Pendentes de Instalação (Recomendados)

### 2.1. Boards (Gerenciamento Visual Kanban) — ⚪ `PENDENTE`
*   **Status:** ⚪ **Pendente de Instalação**.
*   **Repositório Oficial:** [mattermost/focalboard](https://github.com/mattermost/focalboard)
*   **Funcionalidades:** Gerenciador visual de tarefas no estilo Kanban integrado diretamente na navegação do Mattermost.
> [!WARNING]
> **Compatibilidade:** Baixar a versão exata compatível com o seu servidor Mattermost.

---

### 2.2. WelcomeBot (Recepção Automatizada) — ⚪ `PENDENTE`
*   **Status:** ⚪ **Pendente de Instalação**.
*   **Repositório Oficial:** [mattermost/mattermost-plugin-welcomebot](https://github.com/mattermost/mattermost-plugin-welcomebot)
*   **Funcionalidades:** Mensagens automáticas de boas-vindas e direcionamento inicial de novos membros.

---

### 2.3. Autolink (Filtro de Links de Sistemas) — ⚪ `PENDENTE`
*   **Status:** ⚪ **Pendente de Instalação**.
*   **Repositório Oficial:** [mattermost/mattermost-plugin-autolink](https://github.com/mattermost/mattermost-plugin-autolink)
*   **Funcionalidades:** Converte códigos digitados (ex: `ONGSYS-2623`, `WIKI-145`) em links clicáveis para os sistemas.

---

### 2.4. Google Calendar (Agenda Centralizada) — ⚪ `PENDENTE`
*   **Status:** ⚪ **Pendente de Instalação**.
*   **Repositório Oficial:** [mattermost/mattermost-plugin-google-calendar](https://github.com/mattermost/mattermost-plugin-google-calendar)
*   **Funcionalidades:** Sincroniza reuniões, resumos diários da agenda e altera status para "Não Perturbe" automaticamente.

---

## 🧪 3. Plugins de Laboratório / Piloto

### 3.1. Timeline (Linha de Eventos Lateral) — 🧪 `PILOTO`
*   **Status:** 🧪 **Fase Piloto / Testes**.
*   **Repositório Oficial:** [icoretech/mattermost-timeline](https://github.com/icoretech/mattermost-timeline)
*   **Funcionalidades:** Agrupa logs de webhooks em uma linha do tempo lateral sem poluir os canais de texto.

---

### 3.2. Mattermost Agents (Inteligência Artificial Integrada) — 🧪 `PILOTO`
*   **Status:** 🧪 **Fase Piloto / Requer pgvector**.
*   **Repositório Oficial:** [mattermost/mattermost-plugin-agents](https://github.com/mattermost/mattermost-plugin-agents)
*   **Funcionalidades:** Resumo de threads/canais e transcrição de reuniões. Opera em conjunto com o Dify (ver [prompt_ia.md](file:///home/vier/Documentos/Code/CDC/chat/docs/prompt_ia.md)).

---

### 3.3. Voice Messages (Mensagens de Voz com Transcrição) — 🧪 `PILOTO`
*   **Status:** 🧪 **Fase Piloto / Restrições de Disco**.
*   **Repositório Oficial:** [icoretech/mattermost-voice-messages](https://github.com/icoretech/mattermost-voice-messages)
*   **Funcionalidades:** Gravação de áudio no estilo WhatsApp com transcrição de voz para texto no navegador.

---

## 🔎 Nota de Governança sobre a Exibição de Setores (`Carlos [TI]`)

Reiterando as diretrizes técnicas de exibição de identificação visual nas mensagens do chat:
1. O plugin `badges` (instalado) serve para premiação/gratidão entre os colaboradores.
2. O plugin `custom-attributes` adiciona informações ao cartão de perfil.
3. Para exibir o setor permanentemente ao lado de cada mensagem enviada no chat (formato `Carlos [TI]`), edite os campos **First Name (Nome)** ou **Nickname (Apelido)** do usuário em **Configurações ➔ Perfil** (conforme [troubleshooting.md](file:///home/vier/Documentos/Code/CDC/chat/docs/troubleshooting.md)).

---

Última revisão: 2026-07-23  
Responsável pela revisão: Antigravity  
Motivo da revisão: Marcação explícita do status de instalação dos plugins (Instalados, Configuração, Pendentes e Piloto)  
