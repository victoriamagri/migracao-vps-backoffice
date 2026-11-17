# Migração das rotinas, skills e agentes da Victoria para a VPS do time (Backoffice)

Situação em 17/11/2026. Padrão seguido (definido pela gerência do Backoffice): cada rotina numa pasta
`~/rotinas/<nome>/` com `rodar.sh` e `LEIAME.md`, agendada no crontab com comentário, credenciais só no `~/.env`
(`chmod 600`), skills em `~/.claude/skills/<nome>/`. Rotina que não serve mais sai do agendamento.

## Resumo

- **Rotinas agendadas:** 12 no notebook. **3 já rodam na VPS**, 2 estão montadas e esperando a conta do Claude,
  5 travadas por pendências da VPS, 1 removida e 1 fica no notebook (pessoal).
- **Skills:** as **13 operacionais** já estão na VPS. As pessoais e de treinamento ficaram no notebook, por decisão.
- **Agentes (workflows salvos):** os **2** já estão na VPS.
- **Maior bloqueio:** a conta do Claude da VPS sem limite (ver pendência 1).

## Rotinas agendadas

| Rotina | Horário | Situação | Trava em |
| --- | --- | --- | --- |
| Elogios Franquias | todo dia 10h30 (grava na 1ª e 3ª quarta) | ✅ Na VPS; tarefa do Windows desligada | — |
| Monitor Premium Avaliações | todo dia 10h | ✅ Na VPS; publicou em 17/11 (estava parado desde 12/11) | — |
| Monitor Envio em Massa | todo dia 14h | ✅ Na VPS; publicou em 17/11 (estava parado desde 12/11) | — |
| Navi (pendências e panorama semanal) | todo dia 15h, sexta 16h | ⏳ Montada e revisada; leitura do Slack testada | 1, 2 e 3 |
| Respostas Airbnb | seg a sex 9h | ⏳ Montada; leitura da Stays testada (lote de 50). Publicação fica no notebook | 1 e 2 |
| Panorama Nova Ocorrência | seg a sex 16h | ⏸️ Entrega definida: DM da Navi | 1, 2 e 3 |
| Triagem de Comentários | seg a sex 14h | ⏸️ Leitura vai para a VPS; envio fica no notebook | 1, 2, 3 e 4 |
| Contestar Airbnb | 9h e 21h30 | ⏸️ Seleção vai para a VPS; envio fica no notebook | 1, 2 e 4 |
| Reclame Aqui | de hora em hora, 8h às 20h | ⏸️ Parte de Slack e Sapron vai para a VPS | 1, 2, 4 e 5 |
| Portal Avaliações Airbnb | todo dia 9h | ⏸️ Repositório no GitHub atrás do notebook; aviso é balão do Windows | Decisão da Victoria |
| Relatório Áreas Ocorrências | sexta 9h | 🗑️ Removida do agendamento (só gerava HTML local que ninguém recebia) | — |
| Meu Dia | seg a sex 8h30 | Fica no notebook (pessoal, lê arquivos do notebook) | — |

## Skills

### Passadas para a VPS (13 operacionais)

| Skill | Roda na VPS? | Observação |
| --- | --- | --- |
| analise-sapron-implantacao | Sim | Usa Sapron (conectado) |
| caca-ocorrencias | Parcial | Lê o app de ocorrências pelo Lovable (pendência 3) |
| cobrar-sapron | Sim | Usa Sapron (conectado) |
| respostas-sapron | Sim | Usa Sapron (conectado) |
| notas-avaliacao | Sim | Nekt e kpi-system (conectados) |
| kpi-qualidade | Parcial | Parte dos KPIs vem do Lovable (pendência 3) |
| kpi-comentario | Parcial | Parte dos KPIs vem do Lovable (pendência 3) |
| panorama-nova-ocorrencia | Não ainda | Lê tudo pelo Lovable (pendência 3) |
| triagem-comentarios-negativos | Parcial | Leitura sim; envio pelo Chrome fica no notebook (pendência 4) |
| responder-airbnb | Parcial | Lote e textos na VPS; publicação pelo Chrome no notebook |
| contestar-airbnb | Parcial | Seleção na VPS; envio pelo Chrome no notebook |
| reclame-aqui | Parcial | Leitura do site exige Chrome (4) e o Slack exige Composio (5) |
| navi | Sim | Posta com o token próprio do app Navi |

### Ficaram no notebook (por decisão: pessoais ou de treinamento)

iavd, nivelamento-2, preparar-nivelamento-2, heartbeat, meu-dia, score-uso-ia, mapa-atividades, setup-check,
auditoria-ambiente-agentes, doctor-semanal, deepdive, eval, hillclimbing, metodo-batelada, quadro-batelada.

## Agentes (workflows salvos)

| Agente | Situação |
| --- | --- |
| analise-sapron-implantacao | ✅ Na VPS (chamado pelo Panorama Nova Ocorrência) |
| analise-sapron-manutencao | ✅ Na VPS (chamado pelo Panorama Nova Ocorrência) |

## Pendências da VPS

| # | Pendência | Rotinas afetadas | Quem resolve | Ação sugerida |
| --- | --- | --- | --- | --- |
| 1 | Conta do Claude da VPS sem limite (já sem limite às 10h50 de 17/11, antes de qualquer rotina da Victoria usar) | Navi, Respostas Airbnb, Panorama, Triagem, Contestar, Reclame Aqui | Gerência | Conta só para a VPS do time, ou plano com limite maior |
| 2 | Sem troca automática de conta (o switcher do notebook não existe na VPS) | As mesmas 6 | Gerência + admin da VPS | Switcher na VPS ou conta fixa com limite suficiente |
| 3 | Conector Lovable sem login na conta do Claude da VPS | Panorama, Triagem, caca-ocorrencias, KPIs e o alerta de Nova Ocorrência da Navi | Gerência (autorizar) + Victoria (login) | Autorizar e fazer o login uma vez em `/mcp` |
| 4 | Sem navegador: nada de Chrome, Chrome DevTools, Claude in Chrome, Playwright ou tela virtual | Reclame Aqui, Contestar, publicação das Respostas Airbnb, envio da Triagem | Admin da VPS (precisa de sudo) | Manter a publicação no notebook (decidido em 17/11) ou instalar Chrome + tela virtual e testar |
| 5 | Composio não configurado no usuário da VPS | Reclame Aqui | Victoria | Adicionar o Composio e fazer o login dela na VPS |
| 6 | Rotinas da VPS não entram na tela de Uso de IA (o coletor só roda no notebook) | Todas as que usam o Claude | Gerência / dono do coletor | Confirmar se precisa contar e, se sim, instalar o coletor na VPS |
| 7 | Sem aviso central de falha do cron e sem rotação dos logs das rotinas | Todas | Victoria | Aviso por DM da Navi também nas 3 rotinas que já rodam |
| 8 | Sem sudo no usuário da Victoria (esperado) | Instalação de pacote do sistema | Admin da VPS | Pedir quando decidir a pendência 4 |

## O que já está pronto na VPS

- Acesso por ssh (Tailscale), login do Claude e do GitHub.
- `~/.env` com 12 credenciais (`chmod 600`; valores conferidos por hash, nunca exibidos).
- Servidores MCP conectados: Sapron, Suporte Unificado, kpi-system e Taskdex. Nekt pela conta do Claude.
- 13 skills operacionais em `~/.claude/skills` e 2 workflows salvos em `~/.claude/workflows`.
- Correções feitas no caminho: Monitor Premium e Monitor Envio em Massa falhavam desde 13/11 porque liam a chave
  do Sapron de um arquivo que passou a guardar só a referência; na VPS elas leem do `~/.env`.

## Próximos passos

1. Levar as pendências 1, 2 e 3 à gerência de uma vez.
2. Com a resposta, ligar Navi e Respostas Airbnb no cron e desligar as tarefas no Windows.
3. Depois, Panorama Nova Ocorrência e Triagem; por último, Contestar e Reclame Aqui (parte sem navegador).
4. Decidir o destino do Portal Avaliações Airbnb.
