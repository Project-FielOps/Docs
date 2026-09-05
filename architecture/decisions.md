# Decisões arquiteturais

## DEC-001 Fonte de verdade do MVP

**Status:** aprovada em 2026-09-05. As regras textuais e decisões consolidadas da base `base_fieldops` definem produto e MVP. O diagrama e a ficha são históricos; o código divergente é pendência. Perfis oficiais são `ADMIN`, `SUPERVISOR`, `TECHNICIAN` e `CLIENT_VIEWER` futuro. Estados persistidos usam os valores ingleses do contrato observado, com `ASSIGNED` incluído.

## DEC-002 Autorização por responsável

**Status:** aprovada em 2026-09-05. No MVP, somente o técnico atribuído inicia, responde, adiciona evidências e conclui sua inspeção; somente o supervisor atribuído inicia revisão e decide. ADMIN mantém acessos/cadastros e não recebe exceção operacional. A API é a autoridade; guards e menus são apenas ergonomia. O código atual que admite ADMIN ou supervisor não atribuído deve ser corrigido e coberto por testes.

## DEC-003 Correção após reprovação

**Status:** aprovada em 2026-09-05. `REJECTED` pode ser reaberta por ação explícita para `IN_PROGRESS`; o técnico atribuído corrige o checklist inteiro e reenvia. Respostas anteriores permanecem auditáveis; a regra de liberar somente itens indicados fica para P1. A transição exige o supervisor atribuído e não permite alteração silenciosa de respostas pelo supervisor.

```mermaid
stateDiagram-v2
  [*] --> ASSIGNED
  ASSIGNED --> IN_PROGRESS: técnico inicia
  IN_PROGRESS --> SUBMITTED: técnico envia
  SUBMITTED --> UNDER_REVIEW: supervisor inicia
  UNDER_REVIEW --> APPROVED: supervisor aprova
  UNDER_REVIEW --> REJECTED: supervisor reprova com motivo
  REJECTED --> IN_PROGRESS: técnico reabre correção
  ASSIGNED --> CANCELED: supervisor cancela com motivo
  IN_PROGRESS --> CANCELED: supervisor cancela com motivo
  REJECTED --> CANCELED: supervisor cancela com motivo
```

## DEC-008 Prazo e cloud

**Status:** aprovada em 2026-09-05. A entrega acadêmica deve ocorrer até o fim de novembro de 2026. AWS é requisito do projeto. A arquitetura AWS em `aws.md` continua proposta até a equipe escolher serviços, região, conta, IaC e orçamento; nenhum recurso é declarado provisionado sem evidência.

## DEC-009 GPS e QR divergente

**Status:** aprovada em 2026-09-05. GPS é pontual e opcional no MVP: se negado/indisponível, a inspeção continua e registra a ausência. QR que aponta para equipamento diferente não pode confirmar vínculo; o técnico deve identificar manualmente o equipamento previsto. A leitura nunca amplia autorização.

## DEC-013 Contrato de sincronização

**Status:** aprovada em 2026-09-05. Usar `POST /api/v1/mobile/sync/push` e `GET /api/v1/mobile/sync/pull?cursor=...`. Cada operação tem `operationId` idempotente e resultado individual; o cursor só avança depois de persistir alterações. Upload de evidência é separado e a submissão só é confirmada após evidências obrigatórias. Payloads, códigos de conflito e limites permanecem a especificar em `specs/synchronization.md`.
