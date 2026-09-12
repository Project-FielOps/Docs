---
title: FieldOps Knowledge Base
status: active
last_reviewed: 2026-09-06
owners:
  - product
  - backend
  - architecture
related:
  - ../references/base_fieldops/Projeto FieldOps
  - 99-reference/diagnostics/discovery.md
  - 99-reference/knowledge-base-maintenance.md
---

# FieldOps Knowledge Base

## Objetivo

Esta base de conhecimento reúne o contexto essencial do projeto FieldOps para humanos e agentes de IA. Ela funciona como ponto central de consulta para produto, arquitetura, domínio, API, desenvolvimento, infraestrutura, segurança e planejamento.

## Fonte de verdade

A fonte normativa do produto e do domínio é a documentação original em `base_fieldops/Projeto FieldOps`.

Hierarquia de autoridade:

1. decisões formalmente aprovadas e registradas em `_KB/12-decisions`;
2. documentação técnica consolidada em `_KB/`;
3. implementação atual do código como evidência de AS-IS;
4. documentação histórica e legado em `Docs/references` e `_KB/99-reference` como contexto e referência;
5. hipóteses, propostas e rascunhos devem ser marcados como pendentes e não como fato.

## Como usar esta KB

- novos desenvolvedores devem começar por `00-overview` e `04-development`;
- requisitos e regras de negócio ficam em `01-product` e `02-domain`;
- arquitetura e integrações ficam em `03-architecture`;
- API, dados e segurança em `05-api`, `06-data` e `07-security`;
- infraestrutura e deploy em `08-infrastructure` e `09-devops`;
- decisões, planejamento e questões em `12-decisions`, `11-planning` e `99-reference`;
- agentes de IA devem começar por `13-ai/AI-CONTEXT.md`.

## Quick Navigation

- Arquitetura: [03-architecture/system-overview.md](03-architecture/system-overview.md) e [AWS](03-architecture/aws.md)
- Design systems: [interface administrativa](03-architecture/WEB_DESIGN_SYSTEM.md) e [aplicativo mobile](03-architecture/DESIGN_SYSTEM.md)
- Domínio: [02-domain/domain-overview.md](02-domain/domain-overview.md)
- Regras de negócio: [01-product/product-business-rules.md](01-product/product-business-rules.md)
- APIs: [05-api/api-overview.md](05-api/api-overview.md)
- Banco e dados: [06-data/data-model.md](06-data/data-model.md)
- Segurança: [07-security/security-overview.md](07-security/security-overview.md)
- Desenvolvimento: [04-development/development-guide.md](04-development/development-guide.md)
- Infraestrutura: [08-infrastructure/infrastructure-overview.md](08-infrastructure/infrastructure-overview.md) e [plano](08-infrastructure/deployment-plan.md)
- CI/CD: [09-devops/cicd-plan.md](09-devops/cicd-plan.md)
- Testes: [10-quality/testing.md](10-quality/testing.md)
- Decisões: [12-decisions/README.md](12-decisions/README.md)
- Planejamento: [11-planning/roadmap.md](11-planning/roadmap.md)

## Fontes originais

O inventário e a localização das fontes originais estão em [99-reference/sources/sources.md](99-reference/sources/sources.md). O conteúdo histórico não deve ser tratado como contrato vigente sem passar pela hierarquia de autoridade desta KB.

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

## Regra de organização

Não crie novos documentos nos diretórios legados fora de `_KB/`. Use `01-product` para produto/requisitos, `03-architecture` para arquitetura, `04-development` para desenvolvimento, `05-api` para contratos, `08-infrastructure` para implantação, `09-devops` para CI/CD, `10-quality` para qualidade, `11-planning` para planejamento e `12-decisions` para decisões. Diagnósticos e evidências ficam em `99-reference`.

## Fontes primárias

- [../references/base_fieldops/Projeto FieldOps](../references/base_fieldops/Projeto%20FieldOps)
- [99-reference/diagnostics/discovery.md](99-reference/diagnostics/discovery.md)
- [99-reference/knowledge-base-maintenance.md](99-reference/knowledge-base-maintenance.md)
- [../../API/README.md](../../API/README.md)
- [../../interface-administrativa/package.json](../../interface-administrativa/package.json)
