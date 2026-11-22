# Estratégia de testes

---
source: base_fieldops/Projeto FieldOps/19 - Critérios de Avaliação de62594f77cf82f88570814cc1106f5d.md
origin: ../references/base_fieldops/Projeto FieldOps/19 - Critérios de Avaliação de62594f77cf82f88570814cc1106f5d.md
status: active
last_reviewed: 2026-09-05
migration_note: ../migration-inventory.csv
---

## Objetivo

Os testes devem validar que a implementação atual está alinhada com a base documental e com as decisões formais do projeto.

## Cobertura recomendada

- autenticação e autorização;
- criação e publicação de templates;
- inspeção atribuída e fluxo de execução;
- validação de itens obrigatórios e evidências;
- revisão e reprovação;
- correção pós-reprovação;
- auditoria e rastreabilidade;
- proteção de inspeções aprovadas;
- comportamento de sincronização em mobile.

## Observação

O código atual contém testes de autenticação, versionamento e inspeção, mas isso não significa que o sistema inteiro esteja validado. O estado dos testes deve ser lido como evidência de AS-IS e não como conclusão de conformidade funcional.

## Critérios de qualidade

- cada regra do produto deve ter correspondência em teste;
- divergências entre código e base documental devem ser tratadas como pendências de correção;
- qualquer alteração de comportamento deve ser acompanhada de atualização de documentação e testes.

## Rubricas e critérios de avaliação (Importado)

(Trechos do documento original `19 - Critérios de Avaliação`)

### Princípios da avaliação

A avaliação deverá considerar tanto o produto entregue quanto a aprendizagem individual. A existência de uma funcionalidade pronta não será suficiente quando o estudante não conseguir explicar as decisões, o fluxo de dados e sua contribuição.

Princípios:

- avaliação incremental, não concentrada apenas na última semana;
- equilíbrio entre funcionamento, qualidade e compreensão;
- evidências por commits, pull requests, testes e demonstrações;
- integração real entre as aplicações;
- distinção entre nota da equipe e desempenho individual;
- valorização da confiabilidade, não apenas da quantidade de telas.

### Rubrica integrada do produto — 100 pontos (resumo)

| Dimensão | Pontos | Evidências esperadas |
| --- | --- | --- |
| Visão, requisitos e aderência ao escopo | 5 | Backlog coerente, histórias e regras atendidas. |
| Arquitetura e organização | 10 | Separação de responsabilidades, estrutura e decisões documentadas. |
| Aplicativo mobile | 20 | Navegação, checklist, estados, persistência e experiência de campo. |
| Recursos nativos | 10 | Câmera, QR Code, localização e permissões. |
| Offline e sincronização | 15 | SQLite, outbox, idempotência, falhas e estado visível. |
| Interface administrativa | 10 | Cadastros, modelos, planejamento, acompanhamento e revisão. |
| API, regras e modelo de dados | 15 | Segurança, validações, endpoints, persistência e documentação. |
| Testes, segurança e qualidade | 7 | Testes críticos, lint, tratamento de erros e ausência de segredos. |
| Documentação e processo | 4 | README, OpenAPI, backlog, PRs e evidências. |
| Demonstração e defesa técnica | 4 | Fluxo ponta a ponta e explicação individual. |
| **Total** | **100** |  |

### Componentes de avaliação por disciplina

Detalhes por disciplina e marcos estão preservados no original e podem ser consultados em `Docs/_KB/imports/original-19-criterios-avaliacao.md`.

## Fontes relacionadas

- [../../../API/README.md](../../../API/README.md)
- [../99-reference/diagnostics/discovery.md](../99-reference/diagnostics/discovery.md)
- [../01-product/product-business-rules.md](../01-product/product-business-rules.md)
