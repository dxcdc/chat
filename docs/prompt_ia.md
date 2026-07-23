# Contexto de Inteligência Artificial e Governança (CDC Chat)

Este documento estabelece o contexto técnico unificado, a arquitetura de coexistência entre plataformas de IA e as regras de comportamento para assistentes de inteligência artificial no repositório CDC Chat.

---

## 🏗️ Arquitetura Geral de Inteligência Artificial da CDC

A organização adota uma abordagem híbrida e bem delimitada para evitar sobreposição de responsabilidades entre as plataformas de IA:

```text
                       ┌─────────────────────────┐
                       │       Mattermost        │
                       │ canais, usuários, chats │
                       └───────────┬─────────────┘
                                   │
                 ┌─────────────────┴─────────────────┐
                 │                                   │
                 ▼                                   ▼
    ┌────────────────────────┐          ┌────────────────────────┐
    │   Mattermost Agents    │          │          Dify          │
    │                        │          │                        │
    │ Resume canais          │          │ Consulta Wiki Markdown │
    │ Resume threads         │          │ Executa RAG            │
    │ Transcreve reuniões    │          │ Aplica fluxos          │
    │ Pesquisa conversas     │          │ Controla prompts       │
    │ Respeita permissões    │          │ Controla modelos       │
    └────────────────────────┘          └───────────┬────────────┘
                                                   │
                                                   ▼
                                         ┌──────────────────┐
                                         │ Gemini ou outro  │
                                         │ provedor de LLM  │
                                         └──────────────────┘
```

### Divisão Clara de Responsabilidades

| Dimensão | Dify (Cérebro Central Institucional) | Mattermost Agents (Interface Nativa do Chat) |
| :--- | :--- | :--- |
| **Papel Principal** | Orquestrador central de conhecimento, RAG e fluxos. | Camada nativa de interação e ferramentas contextuais do chat. |
| **Base de Conhecimento** | Documentos institucionais, Wiki Markdown, PDFs, APIs. | Mensagens, canais, threads e arquivos anexos do Mattermost. |
| **Histórico e Memória** | Variáveis de sessão do Chatflow, RAG e memória persistente. | Contexto da thread ativa e histórico do canal sob demanda. |
| **Modelos & Credenciais** | Centralizados (Gemini, OpenAI, Anthropic, Ollama/Local). | Consome serviços expostos ou encaminha via MCP/API. |
| **Segurança e Regras** | Prompts institucionais rígidos, reranqueamento e guardrails. | Permissões nativas de acesso a canais e equipes do Mattermost. |

---

## ⚖️ Prevenção de Conflitos Arquiteturais

Para garantir a coerência dos dados e evitar custos duplicados ou respostas divergentes, a arquitetura obedece às seguintes diretrizes estritas:

1. **Fonte Única de Verdade (Single Source of Truth):**
   * A documentação oficial (Wiki Markdown, processos do ONGSYS e políticas) é indexada **exclusivamente no Dify**.
   * O Mattermost Agents **não** duplica a indexação da Wiki, atuando apenas na pesquisa semântica do histórico de conversas dos canais.
2. **Gestão Única de Credenciais e Consumo:**
   * O Dify centraliza as chaves de API dos provedores de LLM (como Gemini), limites de concorrência e logs de execução.
3. **Identidade e Nomes de Bots Claros nos Canais:**
   * `@cdc-conhecimento`: Bot conectado ao Dify para responder sobre documentações, regras da ONG e ONGSYS.
   * `@assistente-mattermost`: Bot do Mattermost Agents para resumir threads, extrair tarefas e resumir canais.
   * `@suporte-ti`: Agente para execução de rotinas e scripts autorizados.
4. **Ponte de Integração via MCP (Model Context Protocol):**
   * Quando o Mattermost Agents precisar responder sobre conhecimento institucional, ele **não** consulta um banco duplicado: ele chama a aplicação do Dify através do protocolo **MCP**.

---

## 🚀 Roteiro de Adopção em Etapas

*   **Etapa 1 — Conexão Direta (Em Operação):**  
    `Mattermost ──> Webhook/Bot ──> Dify (RAG da Wiki) ──> Provedor LLM (Gemini)`  
    Atende perguntas institucionais com o Dify como centro de controle.
*   **Etapa 2 — Ativação de Recursos Nativos no Mattermost:**  
    Instalação do *Mattermost Agents* em ambiente de homologação focado estritamente em: resumo de threads, resumo de canais, extração de tarefas e transcrição de reuniões.
*   **Etapa 3 — Integração Avançada via MCP:**  
    Publicação da aplicação do Dify como servidor MCP e integração com o *Mattermost Agents*, oferecendo a experiência de chat nativa conectada ao conhecimento central do Dify.

---

## 🔒 Restrições Obrigatórias para Assistentes de IA

- **Sem `:latest` em Imagens:** Nunca utilize a tag `:latest` em imagens de contêineres (`postgres:16.4-alpine`, `mattermost/mattermost-team-edition:9.11`).
- **Segurança e Sanitização:** Jamais expor senhas reais, tokens de API ou URLs de webhooks reais do Mattermost em códigos, scripts, README ou documentos.
- **Resiliência a Falhas:** Erros no envio de notificações ou webhooks de IA **nunca** devem paralisar rotinas críticas de infraestrutura ou scripts de backup.
- **Isolamento de Banco:** O PostgreSQL opera unicamente em rede privada interna (`chat-net`), sem portas expostas diretamente no host.

---

## 🛠️ Prompts Rápidos Operacionais (Receitas Reutilizáveis)

### Receita 1: Diagnóstico de Erro em Contêiner
- **Contexto:** Um contêiner está falhando ao subir ou apresentando loop de reinicialização.
- **Objetivo:** Identificar a causa raiz sem alterar dados.
- **Comandos:** `docker compose ps` e `docker compose logs --tail=100 chat`.
- **Documentos:** [troubleshooting.md](file:///home/vier/Documentos/Code/CDC/chat/docs/troubleshooting.md).

---

### Receita 2: Execução de Backup Automatizado
- **Contexto:** Necessidade de rodar o backup diário criptografado com GPG.
- **Comando:** `./scripts/backup_run.sh` ou `./scripts/backup_run.sh "Chat - Mattermost"`.
- **Documentos:** [politica_backup.md](file:///home/vier/Documentos/Code/CDC/chat/docs/politica_backup.md).

---

### Receita 3: Habilitação de Recursos do Mattermost via CLI
- **Contexto:** Configurar opções de upload ou mercado de plugins.
- **Comando:** `docker exec -it <CONTAINER_APP> mmctl config set PluginSettings.EnableUploads true --local`.
- **Documentos:** [guia_plugins.md](file:///home/vier/Documentos/Code/CDC/chat/docs/guia_plugins.md).

---

Última revisão: 2026-07-23  
Responsável pela revisão: Antigravity  
Motivo da revisão: Incorporação da arquitetura de convivência entre Dify e Mattermost Agents  
