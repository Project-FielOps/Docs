# ADR-007 — Logout e invalidação de sessão

## Status
Accepted

## Context

A base documental exige que a troca de senha ou o bloqueio do usuário invalide sessões conforme a política definida pela API. A documentação também exige que credenciais e tokens não sejam gravados em logs. No código atual, a revogação de sessão e o logout formal ainda podem estar incompletos.

## Decision

A política de logout e invalidação de sessão deve ser definida formalmente antes de ser documentada como regra ativa. Enquanto isso, a regra documental deve ser mantida como requisito obrigatório, e qualquer implementação incompleta deve ser tratada como pendência de correção.

## Alternatives Considered

- assumir que o logout atual é suficiente;
- documentar regra de sessão sem evidência de implementação;
- ignorar a exigência da base documental.

## Consequences

- a política de autenticação precisa ser explicitada e testada;
- a segurança da sessão e a auditoria ficam mais robustas;
- as lacunas de implementação ficam registradas em vez de mascaradas.

## Related Components

- auth;
- security;
- JWT;
- user state management.

## Related Requirements

- autenticação;
- sessão e invalidação;
- segurança do sistema.

## References

- [../01-product/product-business-rules.md](../01-product/product-business-rules.md)
- [../99-reference/diagnostics/discovery.md](../99-reference/diagnostics/discovery.md)
