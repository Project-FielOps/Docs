# Documentation Rules

## Metadata

Use frontmatter apenas quando trouxer valor. Exemplo:

---
title: Authentication Architecture
status: active
last_reviewed: 2026-09-05
owners:
  - backend
related:
  - ../05-api/authentication.md
---

## Statuses

- `active`: documento canônico e atualizado
- `proposed`: rascunho aguardando validação
- `deprecated`: substituído por documento mais recente
- `legacy`: material histórico

## One concept, one source

- evitar duplicação; referenciar o documento canônico em vez de copiar conteúdo.
- regras de negócio vivem em `01-product/product-business-rules.md`.

## ADRs

- decisões arquiteturais devem ser registradas em `12-decisions` com formato ADR.
- cada ADR inclui status, contexto, decisão, alternativas, consequencias e referências.

## Process

- Ao alterar comportamento: criar PR, atualizar KB, criar/atualizar ADR se necessário.
- Marcar divergências entre código e documentação como `deviation` em issues.
- Revisões: manter `last_reviewed` atualizado.
