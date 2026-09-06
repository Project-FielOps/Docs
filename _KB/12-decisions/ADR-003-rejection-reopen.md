# ADR-003 — Reabertura de inspeção após reprovação

## Status
Accepted

## Context

A documentação original e as decisões consolidadas explicitam que a inspeção reprovada pode ser reaberta para correção. Essa regra precisa ser preservada como decisão formal para evitar comportamento arbitrário.

## Decision

Quando a inspeção é reprovada, a correção segue um fluxo explícito de reabertura para `IN_PROGRESS`, sob responsabilidade do técnico atribuído. A inspeção mantém rastreabilidade histórica da decisão anterior.

## Alternatives Considered

- permitir reabertura implícita sem documento;
- reclassificar reprovação como bloqueio permanente;
- transformar reprovação em texto sem efeito no fluxo.

## Consequences

- o ciclo de correção e revisão fica rastreável;
- a equipe precisa manter o histórico de decisão;
- a implementação deve preservar o estado e a trilha de auditoria.

## Related Components

- inspection;
- review;
- audit;
- API flow.

## Related Requirements

- revisão e reprovação;
- correção após rejeição;
- rastreabilidade operacional.

## References

- [../01-product/business-rules.md](../01-product/business-rules.md)
- [../../architecture/decisions.md](../../architecture/decisions.md)
