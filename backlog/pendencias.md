# Pendências consolidadas

Data do levantamento: 2026-10-19. Fonte: código atual de `API/`, `interface-administrativa/` e `mobile/`, além da KB em `Docs/_KB/`.

## Bloqueadores P0

| ID | Pendência | Impacto | Task |
| --- | --- | --- | --- |
| P-01 | Atribuições e decisões remotas não chegam completamente pelo feed de sync | técnico pode não receber mudança/cancelamento/revisão | [FO-008](FO-008.md) |
| P-02 | NC offline não é operação idempotente do sync | perda ou duplicidade de NC | [FO-008](FO-008.md) |
| P-03 | Upload de evidência não possui chave de idempotência | retry pode duplicar arquivo | [FO-009](FO-009.md) |
| P-04 | Mobile não usa SQLite transacional e ainda mantém fila/cache simplificados | reinício/offline não tem aceite operacional | [FO-007](FO-007.md) |
| P-05 | Planejamento precisa de consulta autorizada de técnicos/supervisores | supervisor pode receber 403 ou listas incompletas | [FO-002](FO-002.md) |
| P-06 | QR/GPS não possuem trilha completa no contrato de inspeção | requisito nativo não é auditável | [FO-009](FO-009.md) |
| P-07 | Web precisa validar detalhe/revisão/evidência com API real | supervisor pode decidir sem visualizar todos os dados | [FO-003](FO-003.md) |
| P-08 | Autorização por objeto e proteção de evidência precisam de testes adversariais | risco de acesso indevido ou alteração após aprovação | [FO-010](FO-010.md) |
| P-09 | Não existe evidência da jornada web → API → mobile → sync → revisão | MVP não pode ser aceito somente por inspeção estática | [FO-011](FO-011.md) |

## Pendências P1

| ID | Pendência | Task |
| --- | --- | --- |
| P-10 | Filtros globais de locais, equipamentos e NC não têm endpoints completos | [FO-004](FO-004.md) |
| P-11 | Busca textual de usuários não é suportada pelo backend | [FO-004](FO-004.md) |
| P-12 | Recuperação/reset de senha existem na API, mas não estão completos no web | [FO-005](FO-005.md) |
| P-13 | Não há suíte Playwright, regressão visual e auditoria WCAG suficiente | [FO-006](FO-006.md) |
| P-14 | Publicação HTTPS, CloudFront, smoke publicado e rollback ainda não têm evidência | [FO-012](FO-012.md) |

## Dependências críticas

`FO-002` depende do contrato de usuários atribuíveis.

`FO-007` depende de sessão e isolamento de dados por conta.

`FO-008` depende das decisões de sync, idempotência e mudanças remotas.

`FO-009` depende do contrato de QR/GPS e upload.

`FO-011` só pode iniciar o aceite final depois de `FO-002`, `FO-003`, `FO-007`, `FO-008`, `FO-009` e `FO-010`.

Nenhuma task deve contornar essas dependências com `localStorage`, dados mockados ou endpoint inventado.
