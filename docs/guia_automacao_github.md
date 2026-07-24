# Guia de Automação do GitHub e Prompts Universais (CDC)

Este documento descreve como funcionam as automações de **GitHub Issues** e de **Aprovação e Fusão Automática de Pull Requests (Auto-Merge PRs)** via **GitHub Actions**, disponibilizando os **Prompts Universais** para replicar esses fluxos em qualquer repositório da organização CDC.

---

## ⚙️ 1. Automações Ativas no Repositório

### 1.1. Automação de Issues (`.github/workflows/automatizar_issues.yml`)
- **Finalidade:** Lê os arquivos em `docs/` e abre as Issues automaticamente na aba *Issues* do GitHub com títulos, labels e checklists interativos.
- **Evita Duplicatas:** Verifica se a tarefa já foi criada antes de abrir uma nova.

### 1.2. Automação de Auto-Merge de PRs (`.github/workflows/auto_merge_pr.yml`)
- **Finalidade:** Sempre que um Pull Request é aberto ou atualizado no repositório, o robô aprova o PR (`gh pr review --approve`) e faz a fusão automática (`gh pr merge --delete-branch`), apagando a branch temporária após a conclusão.

---

## 📋 2. Prompts Universais para Replicar em Outros Repositórios

Ao abrir um chat de desenvolvimento em qualquer outro repositório da CDC (ex: *Wiki*, *BKP Rclone*, *Postal*, *Moodle*, *ERP*, etc.), copie e cole as instruções abaixo:

### Prompt 1: Criar Automação de Issues
```markdown
Por favor, analise a documentação e as tarefas pendentes deste projeto e crie uma automação completa de GitHub Issues via GitHub Actions.

Requisitos da automação:
1. Crie o arquivo de fluxo `.github/workflows/automatizar_issues.yml` configurado para disparar no `push` da branch `main` e no `workflow_dispatch`.
2. Configure as permissões do workflow para `issues: write` e `contents: read` utilizando a variável `GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}`.
3. No script em bash do workflow, utilize a ferramenta GitHub CLI (`gh issue create`) para criar automaticamente no GitHub as Issues principais deste repositório com títulos, labels e checklists.
4. Adicione uma verificação prévia usando `gh issue list --search` para evitar duplicação de Issues.
5. Adicione as alterações ao Git, faça o commit e o `git push` para a branch `main`.
```

### Prompt 2: Criar Automação de Auto-Merge de PRs
```markdown
Por favor, crie uma automação de aprovação e fusão automática de Pull Requests (Auto-Merge) via GitHub Actions neste repositório.

Requisitos da automação:
1. Crie o arquivo de fluxo `.github/workflows/auto_merge_pr.yml` configurado para disparar em `pull_request_target` (eventos: `opened`, `synchronize`, `reopened`).
2. Configure as permissões do workflow para `contents: write` e `pull-requests: write` utilizando `GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}`.
3. No script em bash, utilize a ferramenta GitHub CLI:
   - `gh pr review "$PR_URL" --approve` para aprovar o PR automaticamente.
   - `gh pr merge "$PR_URL" --merge --delete-branch` para fundir o PR e apagar a branch temporária.
4. Adicione as alterações ao Git, faça o commit e o `git push` para a branch `main`.
```

---

## 🔒 3. Permissões Globais Necessárias na Organização

Para que as automações de Issues e Auto-Merge de PRs funcionem em todos os repositórios da organização:

### A. Liberar Permissão de Leitura e Escrita Globais:
1. Acesse: **`https://github.com/organizations/dxcdc/settings/actions`** (ou nas configurações individuais do repositório).
2. Na seção **Workflow permissions**, selecione: **Read and write permissions**.
3. Marque a opção: **Allow GitHub Actions to create and approve pull requests**.
4. Clique em **Save**.

### B. Liberar Auto-Merge nos Repositórios:
1. Nas configurações do repositório (**Settings ➔ General**), role até a seção **Pull Requests**.
2. Marque a caixinha: **`Allow auto-merge`**.
3. Marque a caixinha: **`Automatically delete head branches`**.

---

Última revisão: 2026-07-24  
Responsável pela revisão: Antigravity  
Motivo da revisão: Adição do fluxo de Auto-Merge de Pull Requests e disponibilização do Prompt Universal  
