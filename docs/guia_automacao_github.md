# Guia de Automação do GitHub e Prompt Universal (CDC)

Este documento descreve como funciona a automação da criação de **GitHub Issues** via **GitHub Actions** e disponibiliza o **Prompt Universal** para replicar essa mesma automação em qualquer outro repositório da organização CDC (ex: *Wiki*, *BKP Rclone*, *ERP*, *Moodle*, etc.).

---

## ⚙️ Como Funciona a Automação no GitHub Actions

A automação fica localizada no arquivo `.github/workflows/automatizar_issues.yml` de cada repositório e funciona da seguinte forma:

```text
Commit / Push no Git ──> GitHub Actions dispara ──> Lê os arquivos em docs/ ──> Executa 'gh issue create' ──> Issues abertas automaticamente no GitHub Web
```

### Principais Recursos da Automação:
1. **Disparo Automático:** Executa sempre que arquivos em `docs/` ou no próprio workflow sofrem alterações.
2. **Execução Manual (`workflow_dispatch`):** Permite rodar a automação a qualquer momento com um clique na aba **Actions** do GitHub.
3. **Prevenção de Duplicatas:** O script consulta `gh issue list --search` antes de criar a tarefa, garantindo que não abrirá Issues duplicadas em novos commits.
4. **Checklists Interativos:** Converte itens `- [ ]` em caixas de seleção clicáveis na aba *Issues* do repositório.

---

## 📋 Prompt Universal para Replicar em Outros Repositórios

Ao abrir um chat de desenvolvimento em qualquer outro repositório da CDC (ex: `BKP Rclone` ou `wiki`), copie e cole a instrução abaixo para que a IA configure a automação idêntica para o projeto:

```markdown
Por favor, analise a documentação e as tarefas pendentes deste projeto e crie uma automação completa de GitHub Issues via GitHub Actions.

Requisitos da automação:
1. Crie o arquivo de fluxo `.github/workflows/automatizar_issues.yml` configurado para disparar no `push` da branch `main` e no `workflow_dispatch`.
2. Configure as permissões do workflow para `issues: write` e `contents: read` utilizando a variável `GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}`.
3. No script em bash do workflow, utilize a ferramenta GitHub CLI (`gh issue create`) para criar automaticamente no GitHub as Issues principais deste repositório, contendo:
   - Títulos claros com identificadores (ex: [FEAT], [CONFIG], [ARCH], [BUG], [DOCS]).
   - Rótulos (labels) adequados ao tipo de tarefa.
   - Critérios de Aceite organizados em caixas de verificação (checklists `- [ ]`).
   - Links apontando para os documentos de referência em `docs/`.
4. Adicione uma verificação prévia usando `gh issue list --search` para evitar que a mesma Issue seja criada repetidamente em novos commits.
5. Adicione as alterações ao Git, faça o commit e o `git push` para a branch `main` para que o GitHub inicie a criação automática das Issues na aba "Issues" do repositório.
```

---

## 🔒 Permissões Necessárias no Repositório

Para que o GitHub Actions consiga criar as Issues automaticamente, certifique-se de que a configuração do repositório permite escrita por workflows:

1. No GitHub, vá em **Settings** ➔ **Actions** ➔ **General**.
2. Na seção **Workflow permissions**, selecione:  
   `Read and write permissions`
3. Marque a caixa **Allow GitHub Actions to create and approve pull requests**.
4. Clique em **Save**.

---

Última revisão: 2026-07-24  
Responsável pela revisão: Antigravity  
Motivo da revisão: Documentação do padrão de automação de Issues e disponibilização do Prompt Universal  
