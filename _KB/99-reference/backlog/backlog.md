# Backlog e rastreabilidade

Fonte histórica: `workflow_cards_github/data/generated_backlog.json`, gerado de uma análise anterior (hash `16cc05dfe25120fc652064495f1f6911cf082637633a37c8b88534073d8ef998`). A cópia está em `../references/generated-backlog-original.json` e contém 80 itens; isso não equivale aos 82 PBI do produto. Não há status de execução confiável nesse catálogo.

| Sprint | Foco | Itens |
|---|---|---:|
| 1 | Arquitetura, TypeScript, Expo Router e qualidade | 25 |
| 2 | Consumo de API, autenticação e sessão | 4 |
| 3 | Dados remotos, cache, listas e filtros | 13 |
| 4 | Formulários, validação e UI dinâmica | 14 |
| 5 | Câmera, imagens, QR Code e localização | 6 |
| 6 | SQLite, outbox e sincronização | 10 |
| 7 | Testes, performance e acessibilidade | 2 |
| 8 | EAS, ambientes e distribuição | 6 |

## Mapa de documentos para itens

| Documento | Itens de origem |
|---|---|
| `architecture/overview.md` e `integration.md` | ARC-001 a ARC-003, IVH-001 |
| `architecture/domain.md` | ARC-004 a ARC-008, ARC-027 |
| `architecture/backend.md` | ARC-009 a ARC-012, ARC-023 a ARC-025, ARC-028 |
| `architecture/frontend.md` | ARC-014, UX-003, UX-007 a UX-010, IVH-006 |
| `architecture/mobile.md` | ARC-013, ARC-029, UX-005, UX-006, UX-011 |
| `architecture/integration.md` | ARC-015 a ARC-019, IVH-005, IVH-007, IVH-012 |
| `architecture/aws.md` | ARC-020 a ARC-022 |
| `engineering/delivery.md` | GES-002 a GES-010, IVH-003, IVH-008 a IVH-016 |

Use o ID, por exemplo `ARC-018`, em issues, pull requests e decisões para manter a rastreabilidade. O JSON permanece a fonte detalhada de responsável, dependências, critérios de aceite e texto original do item.
