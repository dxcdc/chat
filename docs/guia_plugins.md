# Guia de Plugins Recomendados (CDC Chat)

Este documento detalha os principais plugins (tanto pré-instalados quanto recomendados para instalação) para o ecossistema CDC Chat (Mattermost). Focamos na qualidade e relevância das ferramentas para melhorar a comunicação, produtividade, tomada de decisão e organização da equipe.

---

## Estrutura de Gerenciamento de Plugins

Seja instalando via **Plugin Marketplace** (Mercado de Plugins) ou fazendo o upload manual do pacote `.tar.gz` no **Gerenciamento de Plugin** (*Plugin Management*), estas extensões expandem significativamente o potencial do chat da ONG.

---

## Lista de Plugins Relevantes

---

### 1. Custom User Badges (Selos/Plaquinhas de Usuários)
*   **Repositório Oficial:** [larkox/mattermost-plugin-badges](https://github.com/larkox/mattermost-plugin-badges)
*   **Link de Download (.tar.gz):** [Mattermost Badges Releases](https://github.com/larkox/mattermost-plugin-badges/releases)
*   **Alternativa (Custom Attributes):** [mattermost-plugin-custom-attributes](https://github.com/mattermost/mattermost-plugin-custom-attributes/releases)
*   **Detalhes das Funcionalidades:** Permite ao Administrador do Sistema criar crachás ou selos (badges) coloridos personalizados (ex: `[TI]`, `[RH]`, `[ADM]`) e atribuí-los a usuários específicos. Esses selos são exibidos logo ao lado do nome do usuário em cada mensagem enviada nos canais.
*   **Por que escolher:** Essencial para identificar visualmente e de forma imediata o setor, cargo ou nível de autoridade de cada membro nas conversas, sem precisar abrir o perfil individual.
*   **Melhoria que traz:** Traz clareza organizacional para novos voluntários, permitindo que todos saibam instantaneamente quem faz parte do time de suporte, coordenação ou recursos humanos nas discussões.
*   **Exemplo Prático:** 
    *   O usuário Carlos (do time de tecnologia) envia uma mensagem. Ela aparece assim:
        > **Carlos** `[TI]` 14:30  
        > *O servidor de e-mails passou por manutenção e está online.*

---

### 2. Matterpoll (Enquetes e Votações)
*   **Repositório Oficial:** [matterpoll/matterpoll](https://github.com/matterpoll/matterpoll)
*   **Link de Download (.tar.gz):** [Matterpoll Releases](https://github.com/matterpoll/matterpoll/releases)
*   **Detalhes das Funcionalidades:** Adiciona o comando de barra `/poll` para criar enquetes interativas diretamente nos canais de texto. Os usuários votam clicando em botões e o plugin calcula e exibe as estatísticas parciais e finais em tempo real.
*   **Por que escolher:** Elimina a necessidade de contar votos manualmente ou usar ferramentas externas (como Google Forms) para decisões cotidianas da equipe.
*   **Melhoria que traz:** Agiliza a tomada de decisão democrática, aumenta a participação de voluntários em decisões organizacionais e mantém o histórico das votações centralizado no chat.
*   **Exemplo Prático:** Para decidir o local de um evento de integração da equipe, digite no canal:
    ```text
    /poll "Onde deve ser nossa próxima reunião de voluntários?" "Na Sede da ONG" "Formato Online (Jitsi)" "Parque Municipal"
    ```
    Uma enquete limpa será gerada e os usuários votarão clicando nas opções listadas.

---

### 3. Mattermost To-Do (Gerenciamento de Tarefas Pessoais e de Equipe)
*   **Repositório Oficial:** [mattermost/mattermost-plugin-todo](https://github.com/mattermost/mattermost-plugin-todo)
*   **Link de Download (.tar.gz):** [Mattermost To-Do Releases](https://github.com/mattermost/mattermost-plugin-todo/releases)
*   **Detalhes das Funcionalidades:** Cria uma lista de afazeres (To-Do List) integrada ao painel lateral. Permite adicionar tarefas a partir de mensagens enviadas por outros usuários ou criar lembretes rápidos por comandos de barra.
*   **Por que escolher:** Evita que demandas passadas informalmente no meio de conversas em canais públicos ou privados sejam esquecidas pelos membros da equipe.
*   **Melhoria que traz:** Melhora a produtividade pessoal, reduz o esquecimento de tarefas operacionais urgentes e permite delegar tarefas rápidas sem precisar abrir softwares pesados de gestão (como Trello ou ERP).
*   **Exemplo Prático:**
    *   Você lê uma mensagem no chat pedindo para atualizar os dados de uma planilha. Você clica nos três pontinhos (`...`) da mensagem e seleciona **Adicionar aos afazeres** (Add to To-Do).
    *   Ou digita diretamente no canal para criar um lembrete:
        ```text
        /todo add "Revisar logs do banco de dados cdc-ezpoint"
        ```

---

### 4. Jitsi Meet Integration (Videoconferências Gratuitas)
*   **Repositório Oficial:** [jitsi/jitsi-meet-mattermost](https://github.com/jitsi/jitsi-meet-mattermost)
*   **Link de Download (.tar.gz):** [Jitsi Meet Releases](https://github.com/jitsi/jitsi-meet-mattermost/releases)
*   **Detalhes das Funcionalidades:** Integra o servidor de chamadas de vídeo gratuito Jitsi Meet. Adiciona um botão de chamada de vídeo (ícone de câmera) no topo de todos os canais e mensagens diretas.
*   **Por que escolher:** Permite realizar chamadas de vídeo instantâneas com um clique, sem limite de tempo e sem exigir contas pagas (como Zoom ou Google Meet).
*   **Melhoria que traz:** Reduz o atrito de comunicação. Sempre que uma conversa de texto fica complexa, a equipe pode migrar instantaneamente para uma chamada de voz e compartilhamento de tela sem precisar sair do Mattermost.
*   **Exemplo Prático:**
    *   Durante um chat privado, você clica no ícone de câmera de vídeo no topo direito da tela.
    *   O Mattermost envia automaticamente uma mensagem contendo o botão **Join Meeting** (Entrar na Reunião). Ambos clicam e entram na chamada imediatamente pelo navegador ou app móvel.

---

### 5. Mattermost Calls (Chamadas de Voz Nativas - Pré-instalado)
*   **Detalhes das Funcionalidades:** Permite iniciar chamadas de voz e compartilhamento de tela diretamente de dentro de qualquer canal de forma nativa e integrada, utilizando WebRTC.
*   **Por que escolher:** Por já vir pré-instalado e integrado ao ecossistema moderno do Mattermost, ele consome menos recursos de rede e funciona diretamente sob a infraestrutura do contêiner.
*   **Melhoria que traz:** Facilita reuniões de áudio rápidas (estilo *huddles* do Slack) nos canais de setores (ex: TI ou RH) para alinhamentos diários rápidos.
*   **Exemplo Prático:**
    *   No topo direito de qualquer canal de equipe, clique no ícone de telefone para iniciar uma chamada de voz no canal. Todos os membros online verão que uma chamada está ativa e poderão entrar com um clique.

---

### 6. Mattermost Playbooks (Checklists Estruturados - Pré-instalado)
*   **Detalhes das Funcionalidades:** Fornece um painel para execução de checklists interativos, fluxos de automação de incidentes e retrospectivas estruturadas.
*   **Por que escolher:** Essencial para rotinas críticas e repetitivas que exigem passos rígidos (ex: checklist de deploy, resposta a incidentes de segurança ou integração de novos membros).
*   **Melhoria que traz:** Garante que procedimentos operacionais padrão (SOPs) sejam seguidos à risca, reduzindo erros humanos em momentos de estresse operacional ou de infraestrutura.
*   **Exemplo Prático:**
    *   Ao identificar que a VPS está fora do ar, você inicia o Playbook de **Incidente de Servidor**. O Mattermost cria um canal temporário dedicado, notifica os responsáveis e exibe um checklist passo a passo (Passo 1: Verificar disco, Passo 2: Reiniciar Docker, Passo 3: Enviar comunicado).

---

### 7. Giphy (Busca de GIFs)
*   **Repositório Oficial:** [moussetc/mattermost-plugin-giphy](https://github.com/moussetc/mattermost-plugin-giphy)
*   **Link de Download (.tar.gz):** [Giphy Releases](https://github.com/moussetc/mattermost-plugin-giphy/releases)
*   **Detalhes das Funcionalidades:** Integra a API do Giphy para compartilhamento de imagens animadas com base em palavras-chave por comandos de barra.
*   **Por que escolher:** Plugin clássico de engajamento social. Aumenta a descontração e a integração das equipes voluntárias em canais não-profissionais.
*   **Melhoria que traz:** Torna o ambiente de trabalho virtual mais leve e divertido, estimulando interações e comemorações de entregas de projetos.
*   **Exemplo Prático:**
    *   No canal `#off-topic`, digite para comemorar um projeto concluído:
        ```text
        /giphy comemorar sucesso
        ```

---

### 8. GitHub Integration (Integração com Repositórios)
*   **Repositório Oficial:** [mattermost/mattermost-plugin-github](https://github.com/mattermost/mattermost-plugin-github)
*   **Link de Download (.tar.gz):** [GitHub Integration Releases](https://github.com/mattermost/mattermost-plugin-github/releases)
*   **Detalhes das Funcionalidades:** Conecta o Mattermost aos repositórios do GitHub da organização. Permite receber alertas de Pull Requests criados, issues abertas e novos commits diretamente em canais de desenvolvimento.
*   **Por que escolher:** Centraliza o monitoramento de desenvolvimento de software da equipe técnica da ONG sem precisar ficar checando e-mails de notificação do GitHub.
*   **Melhoria que traz:** Acelera o tempo de revisão de código, mantém toda a equipe de TI alinhada com as novidades do código e ajuda na transparência das atualizações de infraestrutura.
*   **Exemplo Prático:**
    *   Sempre que um desenvolvedor abre um Pull Request no repositório `chat` da CDC, o bot envia um card estruturado no canal `#alertas-dev`:
        > **GitHub Bot**  
        > *[PR #4] "adicionar suporte a webhook no script de backup" foi aberto por @dev-cdc. Aguardando revisão.*

---

Última revisão: 2026-07-22  
Responsável pela revisão: Antigravity  
Motivo da revisão: Criação do guia técnico de plugins recomendados para o CDC Chat  
