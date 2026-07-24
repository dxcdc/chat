# Planejamento de Issues do GitHub (CDC Chat)

Este documento contém os modelos estruturados de **GitHub Issues** prontos para serem criados no repositório `dxcdc/chat`. Eles servem para organizar o backlog de tarefas técnicas, desenvolvimento de plugins, configurações de segurança e homologação da arquitetura.

---

## 📋 Lista de Issues para o GitHub

---

### Issue 1: `[FEAT]` Desenvolvimento do Plugin Customizado de Calendário CDC (`cdc-calendar`)

**Título da Issue:** `[FEAT] Desenvolvimento do plugin customizado cdc-calendar (Go + React)`  
**Rótulos (Labels):** `enhancement`, `feature`, `plugin`, `priority:high`

#### 📝 Descrição:
Desenvolver o plugin de calendário customizado para o Mattermost que permita vincular qualquer ID de agenda secundária do Google a canais específicos do chat, superando as limitações do plugin oficial.

#### ✅ Critérios de Aceite (Checklist):
- [ ] Inicializar o projeto a partir do `mattermost-plugin-starter-template`.
- [ ] Implementar o comando de barra `/cdc-cal bind <CALENDAR_ID>` para vincular agendas a canais.
- [ ] Persistir os vínculos no KV Store do Mattermost usando a chave `channel_subscription_{ChannelID}`.
- [ ] Implementar a autenticação centralizada por conta institucional / Service Account.
- [ ] Implementar a rotina de sanitização em Go (remover e-mails de convidados e descrições privadas para conformidade com a LGPD).
- [ ] Criar o worker em Go para envio de resumos diários no canal agendado.
- [ ] Testar a compilação com `make dist` e validar a instalação do pacote `.tar.gz`.

#### 📚 Documento de Referência:
- [desenvolvimento_plugin_calendario.md](file:///home/vier/Documentos/Code/CDC/chat/docs/desenvolvimento_plugin_calendario.md)

---

### Issue 2: `[ARCH]` Homologação da Arquitetura Híbrida de IA (Dify + Mattermost Agents)

**Título da Issue:** `[ARCH] Implantação da arquitetura híbrida de IA com Dify e Mattermost Agents`  
**Rótulos (Labels):** `architecture`, `ai`, `dify`, `priority:medium`

#### 📝 Descrição:
Implantar a infraestrutura de inteligência artificial da CDC dividindo os papéis entre o Dify (orquestrador de conhecimento da Wiki) e o Mattermost Agents (ferramentas nativas do chat).

#### ✅ Critérios de Aceite (Checklist):
- [ ] Garantir que o Dify seja a fonte única de verdade para a base vetorial em Markdown da Wiki.
- [ ] Configurar o bot `@cdc-conhecimento` via Webhook/API do Dify para responder sobre regras institucionais e ONGSYS.
- [ ] Configurar o *Mattermost Agents* em ambiente de homologação focado exclusivamente em: resumo de threads, resumo de canais e extração de tarefas.
- [ ] Testar a integração do Dify como servidor MCP (*Model Context Protocol*) conectado ao Mattermost Agents.
- [ ] Validar que não há duplicação de chamadas de modelos ou respostas concorrentes nos canais.

#### 📚 Documento de Referência:
- [prompt_ia.md](file:///home/vier/Documentos/Code/CDC/chat/docs/prompt_ia.md)

---

### Issue 3: `[CONFIG]` Conclusão da Configuração OAuth2 do Google Calendar Oficial

**Título da Issue:** `[CONFIG] Finalizar integração OAuth2 e permissões do Google Calendar`  
**Rótulos (Labels):** `configuration`, `integrations`, `priority:medium`

#### 📝 Descrição:
Finalizar o cadastro das credenciais de cliente OAuth2 no Google Cloud Console e validar o fluxo de autenticação e notificações no Mattermost.

#### ✅ Critérios de Aceite (Checklist):
- [ ] Cadastrar a URI de redirecionamento autorizada: `https://chat.cdc.org.br/plugins/com.mattermost.gcal/oauth2/complete`.
- [ ] Garantir a ativação da **People API** e da **Google Calendar API** no projeto do Google Cloud.
- [ ] Executar `/gcal connect` e autenticar com a conta institucional da CDC.
- [ ] Testar o comando `/gcal summary` e a exibição de compromissos no chat.

#### 📚 Documento de Referência:
- [guia_plugins.md](file:///home/vier/Documentos/Code/CDC/chat/docs/guia_plugins.md#10-google-calendar-commattermostgcal)

---

### Issue 4: `[ONBOARDING]` Automação de Recepção com WelcomeBot e Playbooks Institucionais

**Título da Issue:** `[ONBOARDING] Configuração de rotinas automáticas de recepção de voluntários`  
**Rótulos (Labels):** `process`, `onboarding`, `documentation`

#### 📝 Descrição:
Configurar a mensagem padrão do WelcomeBot e os checklists do Playbooks para otimizar o onboarding de novos voluntários e colaboradores.

#### ✅ Critérios de Aceite (Checklist):
- [ ] Configurar a mensagem direta de boas-vindas do `WelcomeBot` com direcionamento para os canais `#geral`, `#projetos` e `#tecnologia`.
- [ ] Ativar o Playbook **"Entrada de Novo Colaborador"** com a lista de verificação padrão.
- [ ] Ativar o Playbook **"Resposta a Indisponibilidade de Servidores"** para a equipe de TI.
- [ ] Validar o funcionamento dos checklists em um teste com conta de homologação.

#### 📚 Documento de Referência:
- [guia_plugins.md](file:///home/vier/Documentos/Code/CDC/chat/docs/guia_plugins.md)

---

Última revisão: 2026-07-23  
Responsável pela revisão: Antigravity  
Motivo da revisão: Criação do inventário de Issues padronizadas do GitHub para acompanhamento das tarefas do CDC Chat  
