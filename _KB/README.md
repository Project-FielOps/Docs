---
title: FieldOps Knowledge Base
status: active
last_reviewed: 2026-09-05
owners:
  - product
  - backend
  - architecture
related:
  - ../references/base_fieldops/Projeto FieldOps
  - ../knowledge-base/discovery.md
  - ../knowledge-base/README.md
---

# FieldOps Knowledge Base

## Objetivo

Esta base de conhecimento reúne o contexto essencial do projeto FieldOps para humanos e agentes de IA. Ela funciona como ponto central de consulta para produto, arquitetura, domínio, API, desenvolvimento, infraestrutura, segurança e planejamento.

## Fonte de verdade

A fonte normativa do produto e do domínio é a documentação original em `base_fieldops/Projeto FieldOps`.

Hierarquia de autoridade:

1. documentação original em `base_fieldops`;
2. decisões formalmente aprovadas e registradas em `_KB/12-decisions`;
3. documentação técnica consolidada em `Docs/`;
4. implementação atual do código como evidência de AS-IS;
5. documentação histórica e legado em `Docs/references` e `Docs/planning` como contexto e referência;
6. hipóteses, propostas e rascunhos devem ser marcados como pendentes e não como fato.

## Como usar esta KB

- novos desenvolvedores devem começar por `00-overview` e `04-development`;
- requisitos e regras de negócio ficam em `01-product` e `02-domain`;
- arquitetura e integrações ficam em `03-architecture`;
- API, dados e segurança em `05-api`, `06-data` e `07-security`;
- infraestrutura e deploy em `08-infrastructure` e `09-devops`;
- decisões, planejamento e questões em `12-decisions`, `11-planning` e `99-reference`;
- agentes de IA devem começar por `13-ai/AI-CONTEXT.md`.

## Quick Navigation

- Arquitetura: [03-architecture/current-architecture.md](03-architecture/current-architecture.md)
- Domínio: [02-domain/domain-overview.md](02-domain/domain-overview.md)
- Regras de negócio: [01-product/business-rules.md](01-product/business-rules.md)
- APIs: [05-api/api-overview.md](05-api/api-overview.md)
- Banco e dados: [06-data/data-model.md](06-data/data-model.md)
- Segurança: [07-security/security-overview.md](07-security/security-overview.md)
- Desenvolvimento: [04-development/getting-started.md](04-development/getting-started.md)
- Infraestrutura: [08-infrastructure/infrastructure-overview.md](08-infrastructure/infrastructure-overview.md)
- Testes: [10-quality/testing.md](10-quality/testing.md)
- Decisões: [12-decisions/README.md](12-decisions/README.md)
- Planejamento: [11-planning/roadmap.md](11-planning/roadmap.md)

## Imported Originals (drafts)

- Regras de negócio (import): [Docs/_KB/imports/original-09-regras-de-negocio.md](imports/original-09-regras-de-negocio.md)
- Modelo de dados (import): [Docs/_KB/imports/original-10-modelo-de-dados.md](imports/original-10-modelo-de-dados.md)
- API REST (import): [Docs/_KB/imports/original-12-api-rest.md](imports/original-12-api-rest.md)
- Arquitetura (import): [Docs/_KB/imports/original-11-arquitetura.md](imports/original-11-arquitetura.md)
- Backlog do Produto (import): [Docs/_KB/imports/original-15-backlog.md](imports/original-15-backlog.md)
- Roadmap (import): [Docs/_KB/imports/original-16-roadmap.md](imports/original-16-roadmap.md)
- Critérios de Aceitação (import): [Docs/_KB/imports/original-17-criterios-aceitacao.md](imports/original-17-criterios-aceitacao.md)
- Exemplo de Ficha (import): [Docs/_KB/imports/original-21-exemplo-ficha.md](imports/original-21-exemplo-ficha.md)

## O que está documentado aqui

- problema e objetivo do produto;
- usuários, papéis, escopo e MVP;
- domínio e regras do negócio;
- arquitetura atual e desejada;
- padrões de desenvolvimento e execução do projeto;
- autenticação, autorização, API e banco;
- evidências, gaps e pendências;
- decisões aprovadas e rastreabilidade.

## O que não é esta KB

- não é uma cópia literal de todas as fontes antigas;
- não mistura regra do produto com hipótese de implementação;
- não trata como concluído aquilo que está apenas planejado;
- não ignora divergências entre documentação e código.

## Atualização da documentação

Ao alterar comportamento, regra, contrato, arquitetura ou segurança:

1. confirmar se a mudança é compatível com a base documental original;
2. registrar decisão formal quando houver mudança de escopo;
3. atualizar a documentação relevante na KB;
4. atualizar testes e implementação conforme necessário;
5. registrar pontos pendentes e desvios observados.

## Fontes primárias

- [../references/base_fieldops/Projeto FieldOps](../references/base_fieldops/Projeto%20FieldOps)
- [../knowledge-base/discovery.md](../knowledge-base/discovery.md)
- [../knowledge-base/README.md](../knowledge-base/README.md)
- [../../API/README.md](../../API/README.md)
- [../../interface-administrativa/package.json](../../interface-administrativa/package.json)
