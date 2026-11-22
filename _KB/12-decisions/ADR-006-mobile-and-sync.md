# ADR-006 — Mobile no MVP e sincronização offline

## Status
Accepted

## Context

A documentação original coloca o mobile como parte do MVP, mas define a sincronização offline como um requisito funcional do projeto com regras específicas de persistência local e sincronização com a API. O código atual não demonstra um contrato de sincronização finalizado.

## Decision

O mobile faz parte do MVP, mas sem a sincronização offline no mesmo grau de maturidade da documentação original. A sincronização offline continua sendo um requisito funcional do projeto, porém deve ser tratada com contrato, planejamento e implementação formalizados, sem ser assumida como concluída.

## Alternatives Considered

- tratar mobile como fora do MVP;
- tratar sync offline como concluída apenas por presença de SQLite ou outbox no código;
- ignorar o requisito em documentação oficial.

## Consequences

- a implementação do app precisa acompanhar a regra de execução em campo e persistência local;
- a interface de sincronização precisa ser especificada antes de encerrar a funcionalidade;
- qualquer desvio do código deve ser classificado como pendência de implementação.

## Implementation Notes

- Para o MVP recomenda-se adotar a opção pragmática: documentar os endpoints existentes do backend (batch `PUT /api/v1/inspections/{id}/responses` e `POST /api/v1/inspections/{id}/evidence`) como o contrato canônico de sincronização móvel, até que haja decisão de implementar endpoints unificados `/api/v1/mobile/sync`.
- Se optarmos por um contrato unificado no futuro, formalizar payloads, idempotência, e testes de contrato antes de alterar o backend.

## Next steps

- Atualizar `Docs/_KB/05-api/specs/synchronization.md` para refletir a escolha pragmática (feita) e incluir exemplos de payloads e respostas.
- Identificar gaps no código que exigiriam alteração para suportar um contrato `/mobile/sync` e estimar esforço.

## Related Components

- mobile;
- sync flow;
- evidence handling;
- API integration.

## Related Requirements

- execução em campo;
- offline-first;
- sincronização idempotente;
- rastreabilidade de evidências.

## References

- [../../references/base_fieldops/Projeto FieldOps](../../references/base_fieldops/Projeto%20FieldOps)
- [../01-product/requirements.md](../01-product/requirements.md)
