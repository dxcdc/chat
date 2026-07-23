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

**Finalidade**  
Criar procedimentos repetíveis com etapas, responsáveis, prazos e acompanhamento em tempo real por meio de listas de verificação (checklists) interativas.

**Problema que resolve**  
Processos operacionais críticos (como onboarding de colaboradores, resposta a incidentes de servidores e fechamentos financeiros) eram executados de forma descentralizada ou informal, resultando no esquecimento de etapas e na falta de registro histórico.

**Por que escolhemos**  
Escolhemos o Playbooks porque ele é uma ferramenta nativa do Mattermost, totalmente integrada aos canais de texto, que elimina a necessidade de softwares externos de SOP (Standard Operating Procedure) e oferece visibilidade clara do progresso das tarefas sem custo adicional.

**Exemplo prático de uso**  
A equipe de RH inicia o playbook "Entrada de Novo Colaborador". O Mattermost cria um painel com o checklist:
- [x] Criar e-mail institucional
- [x] Adicionar ao Mattermost
- [ ] Conceder acesso ao ONGSYS
- [ ] Apresentar a Wiki
- [ ] Realizar treinamento inicial  
À medida que o RH marca as caixas, todos os envolvidos acompanham o andamento em tempo real.

**Benefícios esperados**  
- Padronização rígida de processos institucionais.
- Redução de erros por esquecimento.
- Histórico auditável de execução de tarefas.
- Agilidade na integração de novos colaboradores e voluntários.

---

### 2. Mattermost Boards (`focalboard`)

**Finalidade**  
Permitir o gerenciamento visual de tarefas e projetos por meio de quadros no estilo Kanban integrados diretamente ao chat.

**Problema que resolve**  
As tarefas estavam distribuídas entre mensagens, planilhas e conversas privadas, dificultando a identificação de responsáveis, prazos e andamento das atividades.

**Por que escolhemos**  
Escolhemos o Mattermost Boards porque ele utiliza os mesmos usuários e permissões do Mattermost, reduz a necessidade de uma plataforma adicional e oferece uma visualização simples para equipes técnicas e administrativas.

**Exemplo prático de uso**  
A equipe de educação cria o cartão "Revisar apostila do ONGSYS", define Fernando como responsável, informa o prazo de 30/07/2026 e movimenta o cartão pelas etapas Planejado, Em andamento, Em revisão e Concluído.

**Benefícios esperados**  
- Maior visibilidade das atividades da organização.
- Definição clara de responsáveis e prazos.
- Redução de tarefas esquecidas.
- Acompanhamento de prazos e histórico centralizado.

---

### 3. Matterpoll (`com.github.matterpoll.matterpoll`)

**Finalidade**  
Criar enquetes e pesquisas instantâneas diretamente nos canais de texto por meio do comando `/poll`.

**Problema que resolve**  
A tomada de decisões simples (como escolher datas de reuniões ou definir prioridades) gerava dezenas de mensagens desencontradas, exigindo contagem manual de votos.

**Por que escolhemos**  
Escolhemos o Matterpoll por sua simplicidade de uso via comando de barra, suporte a votações anônimas, capacidade de limitar o número de escolhas por usuário e integração direta no fluxo do chat.

**Exemplo prático de uso**  
A coordenação digita no canal `#geral`: `/poll "Qual o melhor dia para nossa reunião de alinhamento?" "Segunda às 19h" "Quarta às 20h" "Sábado às 9h"`. A equipe vota nos botões e o resultado parcial é exibido automaticamente na própria mensagem.

**Benefícios esperados**  
- Agilidade na tomada de decisões da equipe.
- Eliminação de contagens manuais de votos.
- Aumento da participação dos voluntários nas votações.
- Opção de sigilo em votações confidenciais.

---

### 4. Welcome Bot (`com.mattermost.welcomebot`)

**Finalidade**  
Recepcionar novos usuários automaticamente por mensagem direta e orientar seus primeiros passos no ambiente de trabalho virtual.

**Problema que resolve**  
Reduzir o trabalho manual de integração de colaboradores, voluntários, professores e parceiros, evitando que entrem no chat sem saber quais canais frequentar ou como buscar ajuda.

**Por que escolhemos**  
Escolhemos o Welcome Bot porque ele automatiza a recepção inicial, garante que 100% dos novos usuários recebam as orientações corretas e reduz os chamados de suporte inicial direcionados à equipe de TI.

**Exemplo prático de uso**  
Assim que a voluntária Ana cria sua conta no Mattermost, o Welcome Bot envia uma mensagem privada: "Olá, Ana! Seja bem-vinda ao Mattermost do CDC. Qual é sua área de atuação? 1. Projetos | 2. Financeiro | 3. Tecnologia | 4. Educação. De acordo com sua resposta, você será direcionada aos canais recomendados."

**Benefícios esperados**  
- Redução do trabalho manual de recepção.
- Direcionamento rápido dos usuários aos canais adequados.
- Padronização do onboarding institucional.
- Integração mais ágil de novos voluntários.

---

### 5. Autolink (`mattermost-autolink`)

**Finalidade**  
Transformar códigos e padrões de texto escritos nas mensagens em links automáticos e clicáveis.

**Problema que resolve**  
Os usuários mencionavam códigos de chamados, requisições ou páginas da Wiki nas conversas, mas precisavam copiar o código, abrir o sistema em outra aba e pesquisar manualmente o endereço correto.

**Por que escolhemos**  
Escolhemos o Autolink por sua alta eficiência baseada em regras de expressão regular, sendo muito útil para conectar o Mattermost aos seus sistemas, documentos, chamados e páginas da Wiki sem exigir integrações pesadas por código.

**Exemplo prático de uso**  
Um usuário digita na conversa: "A requisição ONGSYS-2623 foi devolvida para correção". O código `ONGSYS-2623` é automaticamente convertido em um link clicável, evitando que as pessoas procurem manualmente o endereço correto no sistema.

**Benefícios esperados**  
- Economia de tempo na navegação entre o chat e os sistemas internos.
- Acesso instantâneo a chamados, requisições e documentações.
- Redução de erros ao buscar informações em outros painéis.
- Integração transparente com o ecossistema da ONG.

---

### 6. Badges for Mattermost (`com.mattermost.badges`)

**Finalidade**  
Permitir a concessão de medalhas e selos virtuais de reconhecimento entre os membros da equipe.

**Problema que resolve**  
Falta de mecanismos simples para valorizar e reconhecer o esforço de voluntários e colaboradores por ajudas pontuais ou destaques em entregas do cotidiano.

**Por que escolhemos**  
Escolhemos o Badges porque ele foi criado principalmente para reconhecimento entre pessoas, permitindo conceder medalhas como "Colaborador", "Mentor", "Ajudou a equipe" ou "Especialista", estimulando a cultura de gratidão no ambiente de trabalho.

**Exemplo prático de uso**  
Após resolver uma dúvida complexa de um colega, o usuário digita: `/badges grant @carlos "Salvou o Dia"`. O selo é registrado e exibido na lista de conquistas do perfil de Carlos.

**Benefícios esperados**  
- Fortalecimento do clima organizacional.
- Valorização transparente das contribuições dos voluntários.
- Estímulo ao espírito de cooperação entre as equipes.

---

### 7. GIF commands / Giphy (`com.github.moussetc.mattermost.plugin.giphy`)

**Finalidade**  
Permitir a busca e o envio de GIFs animados nas conversas por meio de comandos de barra.

**Problema que resolve**  
O ambiente de trabalho virtual pode se tornar excessivamente rígido e impessoal sem recursos visuais de expressão e celebração informal.

**Por que escolhemos**  
Escolhemos o plugin da Giphy por ser a solução padrão e mais estável para busca de GIFs, contando com filtros de conteúdo e chave de API pública oficial.

**Exemplo prático de uso**  
No canal `#off-topic`, para comemorar o sucesso de uma campanha de arrecadação, o coordenador digita: `/giphy comemoração`. O plugin busca e envia um GIF festivo na conversa.

**Benefícios esperados**  
- Humanização do espaço virtual de trabalho.
- Maior engajamento dos membros em momentos de celebração.
- Ambiente mais descontraído nos canais informais.

---

### 8. GitHub (`github`)

**Finalidade**  
Conectar os repositórios de código da CDC no GitHub ao Mattermost, enviando notificações de Pull Requests, issues e permitindo o gerenciamento de revisões de código pelo chat.

**Problema que resolve**  
A equipe de TI precisava checar constantemente a caixa de e-mails ou a interface do GitHub para saber se havia novos bugs relatados ou códigos aguardando revisão.

**Por que escolhemos**  
Escolhemos o plugin oficial do GitHub por ser seguro, nativo e permitir ações de ChatOps (como listar pendências pessoais com `/github todo` e criar issues a partir de mensagens enviadas no chat).

**Exemplo prático de uso**  
Quando um desenvolvedor abre um Pull Request no repositório `chat`, o bot envia um aviso formatado no canal `#alertas-dev`. O responsável clica no link, revisa e aprova o código sem perder o contexto do chat.

**Benefícios esperados**  
- Agilidade na revisão de código e correções de bugs.
- Centralização dos alertas de infraestrutura e desenvolvimento.
- Redução do tempo de resposta da equipe de TI.

---

### 9. Calls (`com.mattermost.calls`)

**Finalidade**  
Iniciar chamadas de voz e compartilhamento de tela diretamente de dentro de qualquer canal ou mensagem direta.

**Problema que resolve**  
Quando uma conversa por texto ficava complexa, a equipe precisava gerar um link externo de Zoom ou Google Meet, fazer login e enviar no chat, causando perda de tempo.

**Por que escolhemos**  
Escolhemos o Calls por ser nativo, leve, utilizar tecnologia WebRTC e não exigir o pagamento de licenças de salas de reunião externas para conversas rápidas.

**Exemplo prático de uso**  
Durante um alinhamento no canal do setor financeiro, o coordenador clica no ícone de telefone no topo da tela. Todos os membros do canal recebem o convite e entram na chamada de áudio instantaneamente.

**Benefícios esperados**  
- Resolução rápida de dúvidas sem sair do Mattermost.
- Economia de custos com ferramentas externas de videoconferência.
- Facilidade de compartilhamento de tela entre colaboradores.

---

### 10. Google Calendar (`com.mattermost.gcal`)

**Finalidade**  
Sincronizar a agenda do Google Workspace dos colaboradores com o Mattermost, enviando lembretes de reuniões e alterando o status pessoal automaticamente para "Em Reunião".

**Problema que resolve**  
Membros da equipe frequentemente perdiam o horário de reuniões institucionais ou eram interrompidos por mensagens no chat enquanto estavam em chamadas com parceiros externos.

**Por que escolhemos**  
Escolhemos a integração com o Google Calendar porque a organização já utiliza a suíte do Google Workspace, tornando natural centralizar os avisos da agenda dentro do chat corporativo.

**Exemplo prático de uso**  
O bot envia o resumo da agenda do dia: "09:00 — Reunião da equipe financeira | 14:00 — Treinamento do ONGSYS | 16:30 — Acompanhamento do projeto Educação para Todos". Durante cada compromisso, o status do usuário é alterado para Ausente/Não Perturbe.

**Benefícios esperados**  
- Redução de atrasos em reuniões agendadas.
- Proteção da concentração dos usuários durante compromissos.
- Visibilidade automática da disponibilidade dos membros.

> [!WARNING]
> **Pendente de Configuração:** Este plugin requer o cadastro das credenciais de cliente `Client ID` e `Client Secret` do console Google Cloud (OAuth2) nas configurações do painel para concluir sua ativação.

---

### 11. Mattermost Timeline (`ch.icorete.mattermost-timeline`)

**Finalidade**  
Receber eventos por webhooks e montar uma linha do tempo animada na barra lateral direita do Mattermost.

**Problema que resolve**  
Notificações automáticas de sistemas (como status de backups, deploys e alertas de monitoramento) poluíam a rolagem dos canais de texto principais.

**Por que escolhemos**  
Escolhemos o Timeline porque seu objetivo é impedir que mensagens automáticas encham os canais, mantendo esses eventos organizados em uma linha do tempo lateral com cards atualizáveis.

**Exemplo prático de uso**  
Linha do tempo de uma requisição no ONGSYS:  
`09:12 — Requisição criada` ➔ `09:40 — Enviada para aprovação` ➔ `10:05 — Aprovada pela coordenação` ➔ `10:30 — Devolvida para correção` ➔ `11:15 — Lançamentos corrigidos` ➔ `11:50 — Encaminhada ao financeiro`.

**Benefícios esperados**  
- Canais de texto limpos de spams de sistemas.
- Acompanhamento visual e cronológico de automações e processos.
- Atualização em tempo real de status de incidentes e finanças.

---

### 12. Mattermost Voice Messages (`ch.icorete.mattermost-voice-messages`)

**Finalidade**  
Permitir a gravação, envio e reprodução de mensagens de voz nos canais e conversas privadas, com transcrição de áudio em texto no navegador.

**Problema que resolve**  
Colaboradores em atividades externas ou com dificuldades de digitação demoravam para responder aos alinhamentos operacionais urgentes.

**Por que escolhemos**  
Escolhemos o Voice Messages porque ele permite gravar pelo navegador ou desktop, ouvir antes de enviar, alterar a velocidade de reprodução e transcrever o áudio localmente sem enviar para serviços pagos na nuvem.

**Exemplo prático de uso**  
Um voluntário em atividade externa grava uma mensagem de voz rápida de 30 segundos. Quem recebe pode ouvir o áudio ou ler a transcrição em texto gerada automaticamente abaixo do player.

**Benefícios esperados**  
- Agilidade na comunicação de equipes externas.
- Acessibilidade para membros com dificuldades de digitação.
- Manutenção do registro textual através das transcrições automáticas.

---

### 13. User Satisfaction Surveys (`com.mattermost.nps`)

**Finalidade**  
Coletar pontuações de satisfação dos usuários e feedbacks qualitativos trimestrais sobre a experiência de uso da plataforma.

**Problema que resolve**  
Dificuldade de saber se os colaboradores e voluntários estão adaptados ao chat ou se estão enfrentando problemas de usabilidade no dia a dia.

**Por que escolhemos**  
Escolhemos este plugin nativo por ser a solução recomendada para medir o índice de aprovação da ferramenta sem a necessidade de pesquisas externas de formulários.

**Exemplo prático de uso**  
Uma vez por trimestre, o sistema exibe uma pergunta discreta ao usuário: "De 0 a 10, o quanto você recomendaria o Mattermost para a comunicação de nossa equipe?", com um campo para sugestões.

**Benefícios esperados**  
- Diagnóstico contínuo da satisfação da equipe.
- Identificação rápida de pontos de dor dos usuários.
- Dados para orientar melhorias na infraestrutura do chat.

---

Última revisão: 2026-07-23  
Responsável pela revisão: Antigravity  
Motivo da revisão: Remoção de linhas de status redundantes no corpo e consolidação de Finalidade, Problema que resolve e Por que escolhemos em 3 parágrafos diretos  
