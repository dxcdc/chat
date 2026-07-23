# Guia Oficial de Plugins e Extensões (CDC Chat)

Este documento centraliza a estrutura padronizada de documentação, finalidades, problemas resolvidos, critérios de escolha, exemplos práticos e benefícios de todas as extensões ativas e em configuração no servidor Mattermost da CDC.

---

## 📌 Resumo Geral de Status e Categorias

| Nome Oficial | ID do Plugin | Versão | Categoria | Status Atual |
| :--- | :--- | :---: | :--- | :---: |
| **Playbooks** | `playbooks` | 1.41.1 | Produtividade / Processos | 🟢 **Em Produção** |
| **Mattermost Boards** | `focalboard` | 8.0.0 | Gestão de Projetos (Kanban) | 🟢 **Em Produção** |
| **Matterpoll** | `com.github.matterpoll.matterpoll` | 1.8.0 | Produtividade / Decisão | 🟢 **Em Produção** |
| **Welcome Bot** | `com.mattermost.welcomebot` | 1.4.1 | Onboarding / Integração | 🟢 **Em Produção** |
| **Autolink** | `mattermost-autolink` | 1.4.1 | Produtividade / Links | 🟢 **Em Produção** |
| **Badges for Mattermost** | `com.mattermost.badges` | 0.2.1 | Reconhecimento / Engajamento | 🟢 **Em Produção** |
| **GIF commands (Giphy)** | `com.github.moussetc.mattermost.plugin.giphy` | 5.0.0 | Comunicação / Social | 🟢 **Em Produção** |
| **GitHub** | `github` | 2.4.0 | Integração Técnica / DevOps | 🟢 **Em Produção** |
| **Calls** | `com.mattermost.calls` | 0.29.8 | Comunicação (Voz e Tela) | 🟢 **Em Produção** |
| **Google Calendar** | `com.mattermost.gcal` | 1.4.0 | Produtividade / Agenda | 🟡 **Em Configuração (OAuth2)** |
| **Mattermost Timeline** | `ch.icorete.mattermost-timeline` | 1.5.1 | Monitoramento / Webhooks | 🧪 **Em Homologação (Piloto)** |
| **Mattermost Voice Messages**| `ch.icorete.mattermost-voice-messages` | 0.1.4 | Comunicação / Acessibilidade | 🧪 **Em Homologação (Piloto)** |
| **User Satisfaction Surveys**| `com.mattermost.nps` | 1.3.3 | Feedback / Qualidade | 🟢 **Em Produção** |

---

## 📄 Documentação Detalhada por Extensão

---

### 1. Mattermost Playbooks (`playbooks`)

1. **Identificação**
   * **Nome:** Mattermost Playbooks
   * **Categoria:** Produtividade e Gestão de Processos
   * **Status:** 🟢 Em Produção (Nativo)

2. **Finalidade**
   * Criar procedimentos repetíveis com etapas, responsáveis, prazos e acompanhamento em tempo real por meio de listas de verificação (*checklists*) interativas.

3. **Problema que resolve**
   * Processos operacionais críticos (como onboarding de colaboradores, resposta a incidentes de servidores e fechamentos financeiros) eram executados de forma descentralizada ou informal, resultando no esquecimento de etapas e na falta de registro histórico.

4. **Por que escolhemos**
   * Escolhemos o Playbooks porque ele é uma ferramenta nativa do Mattermost, totalmente integrada aos canais de texto, que elimina a necessidade de softwares externos de SOP (*Standard Operating Procedure*) e oferece visibilidade clara do progresso das tarefas sem custo adicional.

5. **Exemplo prático de uso**
   * A equipe de RH inicia o playbook **"Entrada de Novo Colaborador"**. O Mattermost cria um painel com o checklist:
     - [x] Criar e-mail institucional
     - [x] Adicionar ao Mattermost
     - [ ] Conceder acesso ao ONGSYS
     - [ ] Apresentar a Wiki
     - [ ] Realizar treinamento inicial
     À medida que o RH marca as caixas, todos os envolvidos acompanham o andamento em tempo real.

6. **Benefícios esperados**
   * Padronização rígida de processos institucionais.
   * Redução de erros por esquecimento.
   * Histórico auditável de execução de tarefas.
   * Agilidade na integração de novos colaboradores e voluntários.

---

### 2. Mattermost Boards (`focalboard`)

1. **Identificação**
   * **Nome:** Mattermost Boards (Focalboard)
   * **Categoria:** Gestão de Projetos e Tarefas
   * **Status:** 🟢 Em Produção

2. **Finalidade**
   * Permitir o gerenciamento visual de tarefas e projetos por meio de quadros no estilo Kanban integrados diretamente ao chat.

3. **Problema que resolve**
   * As demandas e projetos estavam distribuídos entre mensagens soltas no chat, planilhas avulsas e conversas privadas, dificultando saber quem era o responsável, o prazo final e em qual etapa o projeto se encontrava.

4. **Por que escolhemos**
   * Escolhemos o Mattermost Boards porque ele utiliza as mesmas contas e permissões do Mattermost, reduz a necessidade de contratar ferramentas pagas (como Trello ou Asana) e oferece visualização simples para equipes técnicas e administrativas.

5. **Exemplo prático de uso**
   * A equipe de educação cria o cartão *"Revisar apostila do ONGSYS"*, define Fernando como responsável, insere o prazo de `30/07/2026` e movimenta o cartão pelas colunas: `Planejado` ➔ `Em andamento` ➔ `Em revisão` ➔ `Concluído`.

6. **Benefícios esperados**
   * Maior visibilidade das atividades da organização.
   * Definição clara de responsáveis e prazos.
   * Redução de tarefas esquecidas ou duplicadas.
   * Centralização do histórico dos projetos institucionais.

---

### 3. Matterpoll (`com.github.matterpoll.matterpoll`)

1. **Identificação**
   * **Nome:** Matterpoll
   * **Categoria:** Produtividade e Decisão Colaborativa
   * **Status:** 🟢 Em Produção

2. **Finalidade**
   * Criar enquetes interativas com botões de votação instantânea nos canais de texto através do comando `/poll`.

3. **Problema que resolve**
   * A tomada de decisões simples (como escolher datas de reuniões ou definir prioridades da semana) gerava dezenas de mensagens desencontradas nos canais, exigindo contagem manual de votos.

4. **Por que escolhemos**
   * Escolhemos o Matterpoll por sua simplicidade de uso via comando de barra, suporte a votações anônimas, capacidade de limitar o número de escolhas por usuário e integração direta no fluxo do canal.

5. **Exemplo prático de uso**
   * A coordenação digita no canal `#geral`:  
     `/poll "Qual o melhor dia para nossa reunião de alinhamento?" "Segunda às 19h" "Quarta às 20h" "Sábado às 9h"`  
     A equipe clica nos botões e o resultado parcial é contabilizado automaticamente na própria mensagem.

6. **Benefícios esperados**
   * Agilidade na tomada de decisões da equipe.
   * Eliminação de contagens manuais de votos.
   * Aumento da participação dos voluntários nas votações.
   * Opção de sigilo em votações confidenciais.

---

### 4. Welcome Bot (`com.mattermost.welcomebot`)

1. **Identificação**
   * **Nome:** Welcome Bot
   * **Categoria:** Onboarding e Integração de Usuários
   * **Status:** 🟢 Em Produção

2. **Finalidade**
   * Recepcionar novos usuários automaticamente via mensagem privada e orientá-los em seus primeiros passos no servidor.

3. **Problema que resolve**
   * Novos voluntários e colaboradores entravam no chat e ficavam perdidos, sem saber quais canais deveriam frequentar, onde encontrar as regras da ONG ou com quem falar.

4. **Por que escolhemos**
   * Escolhemos o Welcome Bot porque ele automatiza o processo inicial de acolhimento (*onboarding*), reduz a carga de trabalho manual da equipe de RH/TI e garante que 100% dos novos usuários recebam as orientações corretas.

5. **Exemplo prático de uso**
   * Assim que a voluntária Ana cria sua conta no Mattermost, o `WelcomeBot` envia uma mensagem privada:  
     *"Olá, Ana! Seja bem-vinda ao Mattermost do CDC. Caso seja da equipe de Projetos, entre no canal ~projetos. Para dúvidas técnicas, acesse ~ajuda-ti."*

6. **Benefícios esperados**
   * Redução do tempo de adaptação de novos voluntários.
   * Padronização da mensagem de recepção institucional.
   * Direcionamento correto aos canais correspondentes.
   * Redução de chamados de suporte inicial para a TI.

---

### 5. Autolink (`mattermost-autolink`)

1. **Identificação**
   * **Nome:** Autolink
   * **Categoria:** Produtividade e Links de Sistemas
   * **Status:** 🟢 Em Produção

2. **Finalidade**
   * Converter automaticamente termos, códigos ou padrões de texto digitados nas mensagens em links clicáveis apontando para os sistemas da ONG.

3. **Problema que resolve**
   * Os usuários mencionavam códigos de chamados, requisições ou páginas da Wiki nas mensagens, mas era necessário copiar o código, abrir o sistema em outra aba e pesquisar manualmente.

4. **Por que escolhemos**
   * Escolhemos o Autolink por sua alta eficiência baseada em Expressões Regulares (Regex), permitindo conectar o chat ao ONGSYS, à Wiki e aos dashboards sem necessitar de integrações complexas por código.

5. **Exemplo prático de uso**
   * Um usuário digita na conversa: *"A requisição ONGSYS-2623 foi atualizada"*.  
     O Autolink identifica a palavra `ONGSYS-2623` e a transforma instantaneamente em um link clicável que abre a requisição direta no painel do ONGSYS.

6. **Benefícios esperados**
   * Economia de tempo na navegação entre o chat e os sistemas internos.
   * Redução de erros de digitação ao procurar dados em outros sistemas.
   * Conectividade entre a documentação da Wiki e as conversas da equipe.

---

### 6. Badges for Mattermost (`com.mattermost.badges`)

1. **Identificação**
   * **Nome:** Badges for Mattermost
   * **Categoria:** Reconhecimento e Engajamento de Equipe
   * **Status:** 🟢 Em Produção

2. **Finalidade**
   * Permitir que os membros da equipe concedam selos e medalhas virtuais de agradecimento e reconhecimento uns aos outros.

3. **Problema que resolve**
   * Falta de mecanismos simples para valorizar e reconhecer o esforço de voluntários e colaboradores por ajudas pontuais ou destaques em entregas.

4. **Por que escolhemos**
   * Escolhemos o Badges porque estimula uma cultura de gratidão e reconhecimento contínuo entre os pares (*peer-to-peer*), aumentando a retenção e motivação da equipe voluntária.

5. **Exemplo prático de uso**
   * Após resolver uma dúvida complexa de um colega, o usuário digita:  
     `/badges grant @carlos "Salvou o Dia"`  
     O selo *"Salvou o Dia"* é registrado e exibido na lista de conquistas do perfil de Carlos.

6. **Benefícios esperados**
   * Fortalecimento do clima organizacional.
   * Estímulo ao espírito de cooperação entre os membros.
   * Valorização transparente das contribuições dos voluntários.

---

### 7. GIF commands / Giphy (`com.github.moussetc.mattermost.plugin.giphy`)

1. **Identificação**
   * **Nome:** GIF commands (Giphy)
   * **Categoria:** Comunicação e Descontração
   * **Status:** 🟢 Em Produção

2. **Finalidade**
   * Permitir a busca e o envio de GIFs animados no chat por meio do comando `/giphy`.

3. **Problema que resolve**
   * O ambiente de trabalho virtual pode se tornar excessivamente rígido e impessoal sem recursos visuais de expressão e celebração informal.

4. **Por que escolhemos**
   * Escolhemos o plugin da Giphy por ser a solução padrão e mais estável de integração de GIFs, contando com filtro de conteúdo seguro e chave de API oficial gratuita.

5. **Exemplo prático de uso**
   * No canal `#off-topic`, para comemorar o sucesso de uma campanha de arrecadação, o coordenador digita:  
     `/giphy comemoração`  
     O plugin busca e envia um GIF festivo na conversa.

6. **Benefícios esperados**
   * Humanização do espaço virtual de trabalho.
   * Maior engajamento dos membros em momentos de celebração.
   * Ambiente mais descontraído nos canais informais.

---

### 8. GitHub (`github`)

1. **Identificação**
   * **Nome:** GitHub Integration
   * **Categoria:** Integração Técnica e DevOps (ChatOps)
   * **Status:** 🟢 Em Produção

2. **Finalidade**
   * Conectar os repositórios de código da CDC no GitHub ao Mattermost, enviando notificações de Pull Requests, issues e permitindo o gerenciamento de revisões de código pelo chat.

3. **Problema que resolve**
   * A equipe de TI precisava checar constantemente a caixa de e-mails ou a interface do GitHub para saber se havia novos bugs relatados ou códigos aguardando revisão.

4. **Por que escolhemos**
   * Escolhemos o plugin oficial do GitHub por ser seguro, nativo e permitir ações de *ChatOps* (como listar pendências pessoais com `/github todo` e criar issues a partir de mensagens enviadas no chat).

5. **Exemplo prático de uso**
   * Quando um desenvolvedor abre um Pull Request no repositório `chat`, o bot envia um aviso formatado no canal `#alertas-dev`. O responsável clica no link, revisa e aprova o código sem perder o contexto do chat.

6. **Benefícios esperados**
   * Agilidade na revisão de código e correções de bugs.
   * Centralização dos alertas de infraestrutura e desenvolvimento.
   * Redução do tempo de resposta da equipe de TI.

---

### 9. Calls (`com.mattermost.calls`)

1. **Identificação**
   * **Nome:** Mattermost Calls
   * **Categoria:** Comunicação de Voz e Compartilhamento de Tela
   * **Status:** 🟢 Em Produção (Nativo)

2. **Finalidade**
   * Iniciar chamadas de voz e compartilhamento de tela diretamente de dentro de qualquer canal ou mensagem direta.

3. **Problema que resolve**
   * Quando uma conversa por texto ficava complexa, a equipe precisava gerar um link externo de Zoom ou Google Meet, fazer login e enviar no chat, causando perda de tempo.

4. **Por que escolhemos**
   * Escolhemos o Calls por ser nativo, leve, utilizar tecnologia WebRTC e não exigir o pagamento de licenças de salas de reunião externas para conversas rápidas.

5. **Exemplo prático de uso**
   * Durante um alinhamento no canal do setor financeiro, o coordenador clica no ícone de telefone no topo da tela. Todos os membros do canal recebem o convite e entram na chamada de áudio instantaneamente.

6. **Benefícios esperados**
   * Resolução rápida de dúvidas sem sair do Mattermost.
   * Economia de custos com ferramentas externas de videoconferência.
   * Facilidade de compartilhamento de tela entre colaboradores.

---

### 10. Google Calendar (`com.mattermost.gcal`)

1. **Identificação**
   * **Nome:** Google Calendar Integration
   * **Categoria:** Produtividade e Gestão de Agenda
   * **Status:** 🟡 Em Configuração (Aguardando Credenciais OAuth2)

2. **Finalidade**
   * Sincronizar a agenda do Google Workspace dos colaboradores com o Mattermost, enviando lembretes de reuniões e alterando o status pessoal automaticamente para "Em Reunião".

3. **Problema que resolve**
   * Membros da equipe frequentemente perdiam o horário de reuniões institucionais ou eram interrompidos por mensagens no chat enquanto estavam em chamadas com parceiros externos.

4. **Por que escolhemos**
   * Escolhemos a integração com o Google Calendar porque a organização já utiliza a suíte do Google Workspace, tornando natural centralizar os avisos da agenda dentro do chat corporativo.

5. **Exemplo prático de uso**
   * Dez minutos antes de uma reunião de diretoria agendada no Google Calendar, o bot envia um lembrete privado para o usuário com o link da sala. Simultaneamente, seu status no chat muda para *"Em Reunião - Não Perturbe"*.

6. **Benefícios esperados**
   * Redução de atrasos em reuniões agendadas.
   * Proteção da concentração dos usuários durante compromissos.
   * Visibilidade automática da disponibilidade dos membros.

> [!WARNING]
> **Pendente de Configuração:** Este plugin requer o cadastro das credenciais de cliente `Client ID` e `Client Secret` do console Google Cloud (OAuth2) nas configurações do painel para concluir sua ativação.

---

### 11. Mattermost Timeline (`ch.icorete.mattermost-timeline`)

1. **Identificação**
   * **Nome:** Mattermost Timeline
   * **Categoria:** Monitoramento e Notificações de Infraestrutura
   * **Status:** 🧪 Em Homologação (Piloto)

2. **Finalidade**
   * Agrupar eventos de webhooks externos em uma linha do tempo animada exibida na barra lateral direita do Mattermost.

3. **Problema que resolve**
   * Notificações automáticas de servidores (como status de backups, deploys e alertas de monitoramento) poluíam a rolagem dos canais de texto principais.

4. **Por que escolhemos**
   * Escolhemos o Timeline porque ele permite atualizar o status do mesmo evento no mesmo card (evitando duplicação de mensagens) e isola os alertas técnicos em um painel lateral retrátil.

5. **Exemplo prático de uso**
   * Durante a execução do script de backup, o Timeline mostra um card na barra lateral:  
     `09:00 - Backup Iniciado` ➔ `09:12 - Compactado (8.0 MB)` ➔ `09:15 - Enviado ao Google Drive com Sucesso`.

6. **Benefícios esperados**
   * Canais de texto limpos de spams de sistemas.
   * Acompanhamento visual e cronológico de automações.
   * Atualização em tempo real de status de incidentes.

---

### 12. Mattermost Voice Messages (`ch.icorete.mattermost-voice-messages`)

1. **Identificação**
   * **Nome:** Mattermost Voice Messages
   * **Categoria:** Comunicação e Acessibilidade
   * **Status:** 🧪 Em Homologação (Piloto)

2. **Finalidade**
   * Permitir a gravação, envio e reprodução de mensagens de voz nos canais e conversas privadas, com transcrição automática de áudio em texto no navegador.

3. **Problema que resolve**
   * Colaboradores em atividades de campo ou com dificuldades de digitação demoravam para responder aos alinhamentos operacionais urgentes.

4. **Por que escolhemos**
   * Escolhemos o Voice Messages porque ele inclui a transcrição local do áudio diretamente pelo navegador, garantindo que o conteúdo falado também fique registrado em formato de texto pesquisável.

5. **Exemplo prático de uso**
   * Um voluntário na rua grava um áudio de 30 segundos relatando a entrega de suprimentos. Quem recebe pode escolher ouvir o áudio ou ler a transcrição em texto gerada abaixo do tocador.

6. **Benefícios esperados**
   * Agilidade na comunicação de equipes externas.
   * Acessibilidade para membros com restrições de digitação.
   * Manutenção da busca de texto através das transcrições.

---

### 13. User Satisfaction Surveys (`com.mattermost.nps`)

1. **Identificação**
   * **Nome:** User Satisfaction Surveys (NPS)
   * **Categoria:** Feedback e Qualidade de Serviço
   * **Status:** 🟢 Em Produção (Nativo)

2. **Finalidade**
   * Coletar pontuações de satisfação dos usuários e feedbacks qualitativos trimestrais sobre a experiência de uso da plataforma.

3. **Problema que resolve**
   * Dificuldade de saber se os colaboradores e voluntários estão adaptados ao chat ou se estão enfrentando problemas de usabilidade no dia a dia.

4. **Por que escolhemos**
   * Escolhemos este plugin nativo por ser a solução recomendada para medir o índice de aprovação da ferramenta sem a necessidade de pesquisas externas de formulários.

5. **Exemplo prático de uso**
   * Uma vez por trimestre, o sistema exibe uma pergunta discreta ao usuário: *"De 0 a 10, o quanto você recomendaria o Mattermost para a comunicação de nossa equipe?"*, com um campo para sugestões.

6. **Benefícios esperados**
   * Diagnóstico contínuo da satisfação da equipe.
   * Identificação rápida de pontos de dor dos usuários.
   * Dados para orientar melhorias na infraestrutura do chat.

---

Última revisão: 2026-07-23  
Responsável pela revisão: Antigravity  
Motivo da revisão: Reestruturação completa do documento adotando o padrão estrito de 6 seções para todos os 13 plugins ativos/configurados no painel  
