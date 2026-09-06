# ADR-001 — Fonte de verdade do produto e do domínio

## Status
Proposed

## Context

A documentação original em `base_fieldops` contém a visão funcional do produto, as regras de negócio e o modelo de domínio. O código presente no repositório implementa parte da solução, mas apresenta divergências e desvios em autorização, estados e arquitetura de sincronização.

## Decision

A documentação original em `base_fieldops` é a fonte normativa completa do produto e do domínio. O código atua como evidência do estado atual do sistema e deve ser comparado contra a base documental para identificar desvios e pendências.

## Alternatives Considered

- usar o código como fonte principal de verdade;
- misturar documentação histórica, regras do produto e implementação em uma única visão;
- tratar todas as divergências como novas regras sem validação.

## Consequences

- a base documental permanece a referência para requisitos e domínio;
- o código e os testes servem para validar o AS-IS e apontar correções;
- divergências exigem classificação formal de desvio ou pendência.

## Related Components

- documentação do produto;
- regras de negócio;
- arquitetura;
- código do backend e do painel.

## Related Requirements

- MVP funcional e documental dos capítulos originais;
- autorização por perfil e por objeto;
- rastreabilidade de inspeção, evidência e revisão.

## References

- [../../references/base_fieldops/Projeto FieldOps](../../references/base_fieldops/Projeto%20FieldOps)
- [../99-reference/diagnostics/discovery.md](../99-reference/diagnostics/discovery.md)
