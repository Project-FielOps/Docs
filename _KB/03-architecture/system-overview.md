# Arquitetura do FieldOps

## Visão do sistema

O FieldOps digitaliza inspeções técnicas em campo. A solução prevista possui três aplicações independentes, integradas por uma API REST versionada:

```text
App mobile (Expo/React Native + SQLite) ─┐
                                         ├─ HTTPS/JSON/Multipart → API (Spring Boot)
Painel administrativo (Angular) ─────────┘                           │
                                              PostgreSQL + evidências em objeto
```

A organização contém a API (`API`), o painel (`interface-administrativa`) e o app do técnico (`aplicativo-mobile`). O app grava tudo primeiro no SQLite (outbox) e sincroniza pelo contrato [`/mobile/sync`](../05-api/specs/synchronization-contract.md).

## Limites e fontes de verdade

O PostgreSQL é a fonte oficial após a sincronização. O SQLite do app mobile é uma persistência operacional: alterações são gravadas localmente antes do envio e não devem ser descartadas silenciosamente. Arquivos de evidência ficam em armazenamento de objetos; o banco guarda apenas metadados e a chave do objeto.

A API concentra autorização, regras de negócio, auditoria e transições de estado. Interfaces não devem duplicar nem contornar essas validações. O painel consome `/api/v1`; o contrato navegável da API é publicado pelo Swagger.

## Domínios principais

- Identidade e acesso: usuários, papéis e tokens.
- Cadastros: clientes, locais e equipamentos.
- Modelos: checklist versionado, publicação imutável e snapshot.
- Inspeções: planejamento, execução, revisão e decisão.
- Evidências, não conformidades, auditoria e sincronização.

## Invariantes

Versões publicadas e snapshots não podem ser alterados. Uma inspeção aprovada não aceita edição comum. Cada operação de sincronização precisa de identificador idempotente. A autorização é aplicada pela API em todos os endpoints protegidos.

## Guias por frente

- [Domínio, dados e ciclo de inspeção](domain.md)
- [Backend Spring Boot](backend.md)
- [Interface administrativa Angular](frontend.md)
- [Aplicativo Expo/React Native](mobile.md)
- [Integração e sincronização](integration.md)
- [Arquitetura AWS proposta](aws.md)
