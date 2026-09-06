# Arquitetura Desejada (TO-BE)

## Visão geral

A arquitetura TO-BE descreve a proposta normativa do projeto conforme `base_fieldops` e as decisões aprovadas (ADRs). Ela representa o alvo para o qual a implementação deve convergir.

## Componentes principais desejados

- Backend: API robusta em Spring Boot com validação, autorização por objeto e auditoria extensiva.
- Mobile: app Android (Expo/React Native ou nativo) com experiência de execução e download de dados.
- Storage de evidências: AWS S3 (ou equivalente) com metadados em PostgreSQL.
- Sync: push/pull com `operationId` idempotente e cursor de pull.
- Infra: AWS provisionada com IaC, ambientes dev/staging/prod.
- Observability: logs estruturados, tracing e métricas.

## Requisitos não funcionais

- disponibilidade e durabilidade para evidências;
- segurança e conformidade de tokens e credenciais;
- escalabilidade para múltiplas organizações;
- compatibilidade offline parcial (download e execução local) — sync formal.

## Diagramas sugeridos

- System Context;
- Container Diagram (backend, frontend, mobile, storage);
- Deployment Diagram para AWS.

## Referências

- [../../references/base_fieldops/Projeto FieldOps](../../references/base_fieldops/Projeto%20FieldOps)
- [../12-decisions/ADR-005-aws-storage.md](../12-decisions/ADR-005-aws-storage.md)
- [../12-decisions/ADR-006-mobile-and-sync.md](../12-decisions/ADR-006-mobile-and-sync.md)
