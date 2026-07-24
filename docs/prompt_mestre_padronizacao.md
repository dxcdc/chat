# Prompt Mestre de Padronização de Repositórios (CDC)

Este documento contém o **Prompt Mestre de Diagnóstico e Padronização**, projetado para corrigir falhas e replicar a infraestrutura, documentação padronizada e automações do **CDC Chat (`dxcdc/chat`)** em qualquer outro repositório da organização CDC (ex: *Postal*, *Wiki*, *ERP*, *Moodle*, *BKP Rclone*, *Vaultwarden*, *Links*, *Transporte*, etc.).

---

## 📋 Prompt Mestre de Padronização (Copiar e Colar)

Copie e cole todo o bloco de código abaixo no chat de desenvolvimento do repositório que estiver apresentando falhas:

```markdown
Por favor, execute um diagnóstico completo e padronize este repositório para seguir exatamente a arquitetura, estrutura de documentação e automações de CI/CD estabelecidas como padrão oficial na CDC (modelo dxcdc/chat).

Realize as seguintes etapas de correção e padronização:

### 1. Infraestrutura e Arquivos de Raiz:
- Verifique se o `docker-compose.yml` utiliza apenas imagens pinadas em versões estáveis (nunca utilize a tag `:latest`).
- Crie ou corrija o `.env.example` contendo o mapa completo de variáveis sem expor credenciais reais.
- Crie ou corrija o `.gitignore` para bloquear diretórios de dados (`data/`), arquivos de ambiente (`.env`), chaves GPG e logs.
- Crie um `README.md` completo com diagramas de arquitetura Mermaid, instruções de deployment e links para a documentação em `docs/`.

### 2. Documentação Técnica Obrigatória (`docs/`):
Crie e padronize os arquivos Markdown na pasta `docs/`:
- `diretrizes_documentacao.md`: Regras de documentação e tabela de estrutura.
- `estrategia_execucao.md`: Fluxos de Gitflow, ambientes e rollback.
- `migration_guide.md`: Guia de migração e diagnósticos SSH.
- `ajuda_infra.md`: Arquitetura física/virtual, contêineres e isolamento de rede.
- `postmortem.md`: Modelo sem culpabilização para análise de incidentes.
- `troubleshooting.md`: Diagnóstico e solução de problemas recorrentes.
- `politica_backup.md`: Política de backup (3-2-1), retenção, criptografia GPG e restore.
- `prompt_ia.md`: Contexto e regras de comportamento para assistentes de IA.
- `issues_planejamento.md`: Inventário com o backlog de tarefas e checklists do projeto.
- `guia_automacao_github.md`: Guia de automação do projeto com o GitHub Actions.

### 3. Automação de GitHub Actions (`.github/workflows/`):
Crie os arquivos de fluxo do GitHub Actions:
- `.github/workflows/automatizar_issues.yml`:
  - Permissões: `contents: read` e `issues: write`.
  - Token: `GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}`.
  - Utiliza `gh issue list --search` para evitar duplicar tarefas.
  - Executa `gh issue create` para publicar as tarefas planejadas com checklists interativos.
- `.github/workflows/auto_merge_pr.yml`:
  - Permissões: `contents: write` e `pull-requests: write`.
  - Disparo: `pull_request_target` (eventos: `opened`, `synchronize`, `reopened`).
  - Executa `gh pr review "$PR_URL" --approve` e `gh pr merge "$PR_URL" --merge --delete-branch` para fusão automática.

### 4. Validação e Git Push:
- Valide a sintaxe de qualquer script bash utilizando `bash -n`.
- Adicione todos os arquivos criados e corrigidos ao Git (`git add .`).
- Faça o commit com mensagem clara e execute o `git push` para a branch `main` para acionar a criação automática de Issues e workflows no GitHub.
```

---

## 🔍 Por que este repositório (`dxcdc/chat`) funciona sem falhas?

O repositório `dxcdc/chat` tornou-se a referência oficial da ONG porque atende a **4 pilares críticos de resiliência**:

1. **Permissões Explícitas no YAML:** Os workflows contêm o bloco `permissions:` explícito (`issues: write`, `pull-requests: write`, `contents: read`), evitando falhas de permissão quando a Action roda sob o `GITHUB_TOKEN`.
2. **Prevenção de Duplicatas com `gh issue list`:** Antes de tentar criar uma tarefa, o robô faz uma consulta à API do GitHub via CLI para checar se a tarefa já existe.
3. **Isolamento de Erros em Scripts:** Erros em notificações secundárias de webhook não interrompem a execução de scripts de infraestrutura ou backups principais (uso de `set -Eeuo pipefail` com `trap`).
4. **Resolução de Conflitos OAuth2:** As URIs de redirecionamento contêm os caminhos exatos (ex: `/oauth2/complete`) casando 100% com a URL do site (`https://`).

---

Última revisão: 2026-07-24  
Responsável pela revisão: Antigravity  
Motivo da revisão: Disponibilização do Prompt Mestre de Padronização para correção e réplica em outros projetos da CDC  
