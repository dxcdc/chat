# Diretrizes de Análise de Incidentes (Postmortem - CDC Chat)

Este documento orienta a realização de análises pós-incidentes (Postmortem) de forma colaborativa e sem culpabilização (*blameless*). 

O foco de um Postmortem é entender **o que** falhou nos sistemas e processos, e não **quem** errou. O objetivo final é melhorar a estabilidade, a redundância, os alertas de monitoramento e evitar recorrências de falhas no CDC Chat (Mattermost).

---

# Template Oficial de Postmortem

## 1. Identificação do Incidente

*   **Título do Incidente:** `[SINTOMA] — Falha no serviço de chat`
*   **ID do Incidente:** `#MM-YYYYMMDD-X`
*   **Data do Incidente:** `AAAA-MM-DD`
*   **Ambiente Afetado:** `[ ] Desenvolvimento | [ ] Homologação | [ ] Produção`
*   **Sistemas Impactados:** `[ ] Banco PostgreSQL | [ ] Servidor Mattermost | [ ] Proxy Nginx | [ ] Backups`
*   **Responsáveis pela Análise:** `Nome / Equipe`
*   **Severidade:** `[ ] Baixa | [ ] Média | [ ] Alta | [ ] Crítica`
*   **Duração da Indisponibilidade:** `X horas e Y minutos`
*   **Status Atual:** `[ ] Resolvido | [ ] Mitigado (Aguardando Correção Definitiva)`
*   **Canal do Mattermost para Coordenação:** `<MATTERMOST_CHANNEL>`

---

## 2. Resumo Executivo
*Descreva de forma breve (1 parágrafo) o que aconteceu, o impacto imediato percebido pelos usuários, a janela de duração da falha, a forma de detecção e como o ambiente foi normalizado.*

---

## 3. Sintomas e Comportamento
*Quais foram os sinais do erro? Descreva alertas gerados pelo monitoramento, mensagens recebidas via webhook no canal do Mattermost, falha de ping HTTP ou reclamações enviadas pela equipe técnica.*

---

## 4. Impacto Detalhado
*   **Usuários Afetados:** `Número aproximado ou percentual de usuários afetados.`
*   **Serviços Indisponíveis:** `Quais recursos ficaram inativos (ex: envio de mensagens, uploads de anexos, login, alertas)?`
*   **Risco ou Perda de Dados:** `Houve corrupção de tabelas, exclusão acidental ou perda de mensagens?`
*   **Impacto Operacional:** `Como isso afetou o funcionamento diário da ONG?`

---

## 5. Linha do Tempo (Timeline)
*Registre a sequência cronológica dos acontecimentos importantes durante o incidente. Inclua os fusos horários de referência.*

| Horário (UTC) | Evento / Ação Tomada | Responsável |
| :--- | :--- | :--- |
| **`<HH:MM>`** | Primeiro sinal de instabilidade / Alerta no Mattermost. | System / Bot |
| **`<HH:MM>`** | Início da investigação técnica. | `<DEVOPS_LEAD>` |
| **`<HH:MM>`** | Identificação da causa raiz do problema. | `<DB_ADMIN>` |
| **`<HH:MM>`** | Aplicação da medida de mitigação / reinicialização de serviços. | `<DEVOPS_LEAD>` |
| **`<HH:MM>`** | Confirmação de restabelecimento do sistema. | `<QA_TEAM>` |
| **`<HH:MM>`** | Envio de comunicado de normalização no canal do Mattermost. | `<DEVOPS_LEAD>` |

---

## 6. Detecção e Alertas
*   **Como o incidente foi descoberto?** `[ ] Monitoramento Automatizado (Mattermost) | [ ] Monitoramento Externo | [ ] Alerta de Usuário.`
*   **Tempo decorrido até a detecção:** `X minutos.`
*   **Efetividade dos Alertas:** `Os alertas disparados foram úteis e acionáveis? Houve atraso nos alertas do webhook devido a problemas de rede ou timeout?`

---

## 7. Ações de Resposta e Mitigação
*Quais ações foram tomadas para conter a falha? Descreva as decisões, escalonamentos de equipe e como a coordenação interna no canal do Mattermost ajudou na velocidade de resposta.*

---

## 8. Causa Raiz (Metodologia dos 5 Porquês)

1.  **Por que o chat ficou fora do ar?** Porque o contêiner do Mattermost não conseguia se conectar ao banco.
2.  **Por que não conectava ao banco?** Porque o contêiner do PostgreSQL estava paralisado.
3.  **Por que o PostgreSQL paralisou?** Porque a VPS esgotou o espaço em disco físico.
4.  **Por que o espaço em disco esgotou?** Porque os logs de auditoria do Mattermost não estavam rotacionando e acumularam 45GB.
5.  **Por que os logs não rotacionavam?** Porque a configuração de log rotate do Docker no host não estava definida para os contêineres do chat.

---

## 9. Fatores Contribuintes
*   **Arquitetura:** `Falta de disco isolado para volumes de logs.`
*   **Monitoramento:** `Ausência de alerta de disco acima de 80% no host.`
*   **Configuração:** `Ausência de limite de tamanho de logs no arquivo docker-compose.yml.`

---

## 10. O que funcionou bem
*   *O script de backup gerou o dump do banco 3 horas antes, garantindo integridade caso precisasse restaurar.*
*   *A colaboração no canal de incidentes do Mattermost permitiu alinhar rapidamente a liberação de espaço em disco.*

## 11. O que NÃO funcionou bem
*   *Ficamos sem acesso ao próprio chat (Mattermost) para coordenar a crise de infraestrutura, forçando o uso de canais alternativos devido ao chat estar hospedado no mesmo servidor.*
*   *O webhook de alerta de backup falhou silenciosamente antes do incidente devido a espaço em disco.*

---

## 12. Avaliação da Comunicação
*   *Houve mensagens objetivas enviadas no canal alternativo?*
*   *Houve vazamento acidental de tokens de webhook ou credenciais nos logs compartilhados durante a crise?*

---

## 13. Ações Corretivas e Preventivas (Plano de Ação)

| Ação Recomendada | Tipo | Prioridade | Responsável | Prazo | Status |
| :--- | :--- | :---: | :--- | :---: | :--- |
| Configurar limite de logs no docker-compose.yml | Prevenção | Alta | DevOps | AAAA-MM-DD | Pendente |
| Criar alerta de monitoramento externo de disco (Zabbix/Prometheus) | Detecção | Alta | Infra | AAAA-MM-DD | Pendente |
| Documentar canal de comunicação alternativo para indisponibilidade do chat | Processo | Média | Tech Lead | AAAA-MM-DD | Pendente |

---

## 14. Lições Aprendidas
*Escreva as conclusões e aprendizados obtidos pela equipe que servirão de referência para o planejamento das próximas sprints de infraestrutura.*

---

## 15. Evidências Anexadas (Sanitizadas)
*Cole aqui logs de erros, métricas de hardware da VPS, capturas de tela e links de commits/PRs relacionados ao incidente. Certifique-se de remover senhas ou tokens de webhook antes de salvar.*

---

Última revisão: 2026-07-13  
Responsável pela revisão: Antigravity  
Motivo da revisão: Inicialização do modelo de postmortem para o ecossistema do CDC Chat  
