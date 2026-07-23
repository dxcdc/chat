# Guia de Plugins e Extensões (CDC Chat)

Este documento centraliza a análise técnica, priorização, links oficiais e roteiros práticos dos plugins recomendados para o ecossistema do CDC Chat (Mattermost). Focamos na qualidade, relevância operacional e melhoria de processos para a equipe da ONG.

---

## 📌 Resumo Lógico de Prioridades

| Plugin | Prioridade | Finalidade Principal | Link de Download / Repositório |
| :--- | :---: | :--- | :--- |
| **Playbooks** | **Muito Alta** | Processos padronizados e checklists interativos | *Pré-instalado nativamente* |
| **Boards (Kanban)** | **Alta** | Gestão visual de projetos e tarefas | [Boards Releases](https://github.com/mattermost/focalboard/releases) |
| **WelcomeBot** | **Alta** | Recepção e onboarding automático de membros | [WelcomeBot Releases](https://github.com/mattermost/mattermost-plugin-welcomebot/releases) |
| **Autolink** | **Alta** | Conversão de tags de texto em links clicáveis | [Autolink Releases](https://github.com/mattermost/mattermost-plugin-autolink/releases) |
| **Google Calendar** | **Média** | Centralização de agendas e notificações de reuniões | [Google Calendar Releases](https://github.com/mattermost/mattermost-plugin-google-calendar/releases) |
| **Timeline** | **Média (Piloto)** | Linha do tempo lateral para logs de webhooks | [Timeline Releases](https://github.com/icoretech/mattermost-timeline/releases) |
| **Mattermost Agents** | **Baixa (Piloto)** | Inteligência Artificial e buscas semânticas locais | [Agents Releases](https://github.com/mattermost/mattermost-plugin-agents/releases) |
| **Voice Messages** | **Baixa (Piloto)** | Mensagens de voz com transcrição integrada | [Voice Messages Releases](https://github.com/icoretech/mattermost-voice-messages/releases) |

---

## ⚡ Plugins de Prioridade Muito Alta / Alta

### 1. Playbooks (Processos e Checklists)
*   **Repositório:** *Instalado por padrão no Mattermost.*
*   **Funcionalidades:** Cria procedimentos repetíveis com etapas, responsáveis, prazos definidos e acompanhamento centralizado. Permite acionar checklists automatizados a partir de eventos ou palavras-chave no chat.
*   **Por que escolher:** É a ferramenta com maior potencial de transformação interna. Padroniza rotinas manuais críticas em roteiros acionáveis para qualquer voluntário seguir.
*   **Melhoria que traz:** Organiza e documenta os fluxos operacionais, reduzindo a necessidade de supervisão direta e evitando o esquecimento de etapas cruciais em processos complexos.
*   **Exemplo Prático (Onboarding de Novo Colaborador):**
    *   Ao ativar o playbook de entrada de colaborador, a equipe de RH recebe a seguinte lista interativa:
        - [x] Criar e-mail institucional
        - [x] Adicionar ao Mattermost
        - [ ] Conceder acesso ao ONGSYS
        - [ ] Apresentar a Wiki
        - [ ] Realizar treinamento inicial
*   **Outros Casos de Uso Recomendados:** Organização de eventos, publicação de cursos no Moodle, atendimento de chamados, implantação de sistemas, resposta a indisponibilidade de servidores (comunicação de incidentes) e fechamento financeiro mensal.

---

### 2. Boards (Gerenciamento Visual Kanban)
*   **Repositório Oficial:** [mattermost/focalboard](https://github.com/mattermost/focalboard)
*   **Funcionalidades:** Oferece um gerenciador visual de tarefas no estilo Kanban integrado diretamente na barra de navegação do Mattermost. Permite gerenciar cartões, delegar responsáveis, definir prazos e acompanhar o status de projetos.
*   **Por que escolher:** Facilita a colaboração de múltiplos departamentos (tecnologia, comunicação, financeiro, RH) na organização de suas metas semanais sem sair da ferramenta de comunicação.
*   **Melhoria que traz:** Traz clareza sobre "quem está fazendo o que" e o progresso das entregas, diminuindo o ruído de reuniões de status.
*   **Exemplo Prático:** Um cartão de tarefa no quadro do setor de comunicação:
    *   **Título:** Criar apostila do ONGSYS
    *   **Responsável:** Fernando
    *   **Prazo:** 30/07
    *   **Situação:** `[Em revisão]`
> [!WARNING]
> **Compatibilidade de Versão:** É crucial baixar a versão exata compatível com o seu servidor Mattermost. Por exemplo, o Boards v9.3.1 exige o Mattermost Server v11.9 ou superior. Certifique-se de validar sua versão antes do upload.

---

### 3. WelcomeBot (Recepção Automatizada)
*   **Repositório Oficial:** [mattermost/mattermost-plugin-welcomebot](https://github.com/mattermost/mattermost-plugin-welcomebot)
*   **Funcionalidades:** Envia uma mensagem direta de boas-vindas customizada a todo usuário que entra no servidor ou em canais específicos.
*   **Por que escolher:** Reduz o trabalho manual de integração de novos voluntários, parceiros ou alunos, direcionando-os aos canais certos instantaneamente.
*   **Melhoria que traz:** Automatiza a recepção e o direcionamento inicial (onboarding) dos usuários do sistema, melhorando a experiência inicial do usuário.
*   **Exemplo Prático:**
    > **WelcomeBot** 09:00  
    > *Olá, Ana! Seja bem-vinda ao Mattermost do CDC.*  
    > *Qual é a sua área de atuação?*  
    > *1. Projetos* | *2. Financeiro* | *3. Tecnologia* | *4. Educação*  
    > *(De acordo com a sua escolha, o bot te guiará para os canais corretos).*

---

### 4. Autolink (Filtro de Links de Sistemas)
*   **Repositório Oficial:** [mattermost/mattermost-plugin-autolink](https://github.com/mattermost/mattermost-plugin-autolink)
*   **Funcionalidades:** Analisa todas as mensagens em busca de termos e códigos pré-definidos (via expressões regulares) e os converte de forma transparente em links clicáveis para os sistemas correspondentes.
*   **Por que escolher:** Conecta de forma limpa o Mattermost aos demais sistemas da CDC (Wiki, ONGSYS, chamados de suporte), economizando tempo de busca manual de URLs.
*   **Melhoria que traz:** Facilita a navegação entre o chat e os painéis administrativos, agilizando fluxos de trabalho e suporte.
*   **Exemplos de Mapeamento:**
    *   `ONGSYS-2623` ➔ Abre a requisição correspondente no painel do ONGSYS.
    *   `WIKI-145` ➔ Abre a página de documentação correspondente na Wiki.
    *   `CHAMADO-381` ➔ Abre a tela do chamado correspondente no sistema de suporte.
    *   `PROJETO-028` ➔ Abre a página do projeto.
*   **Exemplo Prático na Conversa:**
    *   Um usuário envia: *"A requisição ONGSYS-2623 foi devolvida para correção."*
    *   O código `ONGSYS-2623` é automaticamente transformado em um link azul clicável apontando diretamente para a requisição no sistema.

---

## 📅 Plugins de Prioridade Média (Produtividade e Monitoramento)

### 5. Google Calendar (Agenda Centralizada)
*   **Repositório Oficial:** [mattermost/mattermost-plugin-google-calendar](https://github.com/mattermost/mattermost-plugin-google-calendar)
*   **Funcionalidades:** Conecta o calendário institucional do Google Workspace/Gmail de cada usuário ao Mattermost. Envia resumos diários, alertas de reuniões de última hora e altera o status para "Não Perturbe" automaticamente durante eventos.
*   **Por que escolher:** Como a CDC utiliza os serviços Google, essa integração unifica a comunicação com a rotina de reuniões diárias da equipe.
*   **Sinais de Qualidade:** Desenvolvimento muito ativo com mais de 56 estrelas, 41 forks e liberação recente de atualizações (versão v1.5.0 lançada em fevereiro de 2026 com automação de segurança).
*   **Exemplo Prático:**
    > **Calendar Bot** 08:00  
    > **Agenda de Hoje:**  
    > * 09:00 — Reunião da equipe financeira  
    > * 14:00 — Treinamento do ONGSYS  
    > * 16:30 — Acompanhamento do projeto Educação para Todos

---

### 6. Timeline (Linha de Eventos Lateral)
*   **Repositório Oficial:** [icoretech/mattermost-timeline](https://github.com/icoretech/mattermost-timeline)
*   **Funcionalidades:** Recebe eventos via webhooks de monitoramento (backups, status de servidores, fluxos de chamados) e monta uma linha do tempo organizada na barra lateral do Mattermost, mantendo os canais limpos de mensagens automatizadas (spam de logs).
*   **Por que escolher:** Excelente para pilotos de automação e equipes de infraestrutura acompanharem o andamento dos processos sem poluir os chats normais.
*   **Sinais de Qualidade:** Releases assinadas com chaves GPG, suporte a atualizações de eventos existentes no mesmo card (evitando duplicação) e versão v1.5.1 lançada em junho de 2026.
*   **Exemplo Prático aplicado ao ONGSYS:**
    *   Visualização rápida na barra lateral:
        > **Requisição 2623 — Calçado de Segurança**  
        > * 09:12 — Requisição criada  
        > * 09:40 — Enviada para aprovação  
        > * 10:05 — Aprovada pela coordenação  
        > * 10:30 — Devolvida para correção  
        > * 11:15 — Lançamentos corrigidos  
        > * 11:50 — Encaminhada ao financeiro

---

## 🧪 Plugins de Prioridade Baixa (Laboratório e Inovação)

### 7. Mattermost Agents (Inteligência Artificial Integrada)
*   **Repositório Oficial:** [mattermost/mattermost-plugin-agents](https://github.com/mattermost/mattermost-plugin-agents)
*   **Funcionalidades:** Conecta modelos de inteligência artificial (locais ou de nuvem) diretamente ao Mattermost para fins de resumo de conversas, assistência em redações e buscas semânticas.
*   **Por que escolher:** Altíssimo potencial para criar um assistente de documentação inteligente que tira dúvidas sobre o funcionamento do ONGSYS e da Wiki da ONG.
*   **Requisitos Técnicos:** Mattermost Server v10.0 ou superior, banco de dados PostgreSQL com a extensão **`pgvector`** ativada para buscas semânticas de vetor.
*   **Sinais de Qualidade:** Mais de 230 estrelas, 90 forks, 72 versões publicadas e desenvolvimento extremamente ativo.

---

### 8. Voice Messages (Mensagens de Voz com Transcrição)
*   **Repositório Oficial:** [icoretech/mattermost-voice-messages](https://github.com/icoretech/mattermost-voice-messages)
*   **Funcionalidades:** Permite gravar mensagens de áudio pelo navegador ou aplicativo desktop e enviá-las em canais ou threads. Ele realiza a **transcrição de áudio para texto localmente** usando o navegador, sem enviar o arquivo para APIs de nuvem externas.
*   **Por que escolher:** Acessibilidade para usuários com dificuldades de digitação ou equipes operando remotamente via celular.
*   **Limitações:** O aplicativo móvel nativo consegue apenas reproduzir as mensagens de áudio gravadas, mas o botão de gravação não aparece nativamente nos celulares.
> [!IMPORTANT]
> **Condições para Instalação (Recomendação de Governança):**
> Para evitar problemas de sobrecarga de disco e acessibilidade na VPS, o plugin só deve ser instalado caso:
> 1. A transcrição automática em português esteja habilitada e com modelo de áudio leve.
> 2. Haja uma orientação interna de gravar apenas áudios curtos.
> 3. Seja feito o acompanhamento mensal do armazenamento do volume `./data/data` no host.
> 4. Futuramente se limite a duração dos áudios a um máximo de 2 a 3 minutos nas configurações do plugin.

---

## 🔎 Nota Importante sobre Selos (Badges vs. Custom Attributes)

Com base nas limitações de ferramentas da comunidade para o Mattermost, é fundamental entender a diferença entre os dois plugins de atributos:

1.  **`mattermost-plugin-badges` (Selos de Reconhecimento):** Foi desenvolvido para a dinâmica de gratidão e premiação entre membros (ganhar medalhas como *"MVP"*, *"Mentor"*, *"Ajudou a Equipe"*). Os selos aparecem no perfil do usuário, mas **não** fixam uma identificação de setor (como `[TI]`) permanentemente ao lado do nick do usuário em todas as mensagens do chat.
2.  **`mattermost-plugin-custom-attributes` (Atributos Personalizados):** Permite criar campos administrativos (como Unidade, Setor e Cargo) que aparecem no cartão do perfil do usuário quando clicamos nele. No entanto, por padrão do Mattermost, essas informações também **não** são injetadas diretamente na linha da mensagem ao lado do nome do usuário.

> [!TIP]
> **Solução Recomendada:** Para exibir identificações como **`Carlos [TI]`** ou **`Ana [RH]`** diretamente ao lado de cada mensagem nas salas de chat de forma nativa e sem precisar programar, configure e utilize os campos **First Name (Nome)** ou **Nickname (Apelido)** nas configurações de perfil do usuário (conforme as instruções do [Manual de Troubleshooting](file:///home/vier/Documentos/Code/CDC/chat/docs/troubleshooting.md)).

---

Última revisão: 2026-07-23  
Responsável pela revisão: Antigravity  
Motivo da revisão: Atualização do guia de plugins com prioridades detalhadas, links oficiais e limitações técnicas  
