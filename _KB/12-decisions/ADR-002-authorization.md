# ADR-002 — Autorização por perfil e por objeto

## Status
Accepted

## Context

A base documental original define que o técnico somente executa inspeções atribuídas a ele, o supervisor aborda apenas inspeções do seu escopo e o administrador cuida de cadastros e permissões, sem assumir execução operacional por padrão. O código do repositório pode apresentar permissões mais amplas em alguns pontos.

## Decision

A autorização por perfil e por objeto é regra oficial do MVP. Em caso de divergência entre implementação e regra documental, o comportamento observado deve ser registrado como desvio pendente de correção.

## Alternatives Considered

- permitir que a implementação atual ditasse as regras;
- permitir exceções administrativas sem documentação;
- ignorar diferenças entre regra e código.

## Consequences

- a segurança e a rastreabilidade do fluxo ficam consistentes com o produto;
- o código precisa ser revisado para atender ao modelo de acesso da base documental;
- agências de IA e desenvolvedores precisam validar reduzido escopo de cada operação.

## Related Components

- auth;
- inspection;
- review;
- user;
- API security.

## Related Requirements

- autenticação e autorização do MVP;
- responsabilidade de técnico e supervisor;
- regras de consulta e revisão.

## References

- [../../references/base_fieldops/Projeto FieldOps](../../references/base_fieldops/Projeto%20FieldOps)
- [../01-product/product-business-rules.md](../01-product/product-business-rules.md)
