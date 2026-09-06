# Docs — Projeto FieldOps

Esta é a documentação versionada do FieldOps. A fonte atual para humanos e agentes de IA é `_KB/`. O código vive em `API/` e `interface-administrativa/`; o app mobile ainda é uma frente planejada.

## Roteiro de leitura

1. [Visão do produto](_KB/01-product/product-overview.md) e [MVP](_KB/01-product/product-mvp.md) — o que o sistema deve fazer.
2. [Arquitetura geral](_KB/03-architecture/system-overview.md) — limites entre API, painel, mobile e dados.
3. [Arquitetura AWS](_KB/03-architecture/aws.md) — topologia de produção e responsabilidades de infraestrutura.
4. [Plano de implantação](_KB/08-infrastructure/deployment-plan.md) — sequência para backend, banco, S3 e frontend.
5. [Plano de CI/CD](_KB/09-devops/cicd-plan.md) — validação local e promoção via GitHub Actions.
6. [Especificações](_KB/05-api/specs/README.md) — contratos implementáveis por domínio.

Para agentes de IA, leia também [regras de desenvolvimento](_KB/13-ai/DEVELOPMENT-RULES.md), [mapa do código](_KB/13-ai/CODEBASE-MAP.md) e [fonte de verdade](_KB/README.md). Ao implementar, diferencie sempre requisito, decisão, estado observado e evidência de validação.

## Organização

`_KB/` contém a documentação atual; `references/` contém fontes históricas; `decisions/` contém pendências e evidências; `01-tap/`, `02-eap/` e `03-cronograma/` são artefatos acadêmicos. Os antigos diretórios de categoria são apenas compatibilidade; veja [LEGACY-PATHS.md](LEGACY-PATHS.md). A precedência e manutenção estão em [`_KB/README.md`](_KB/README.md).

| Área | Documento | Uso |
|---|---|---|
| Diagnóstico e fontes | [`_KB/99-reference/diagnostics/discovery.md`](_KB/99-reference/diagnostics/discovery.md), [`_KB/99-reference/sources/sources.md`](_KB/99-reference/sources/sources.md) | Evidências, lacunas, conflitos e inventário |
| Produto e requisitos | [`_KB/01-product/README.md`](_KB/01-product/README.md) | Problema, escopo, regras, requisitos e rastreabilidade |
| Especificações | [`_KB/05-api/specs/README.md`](_KB/05-api/specs/README.md) | Casos de uso e contratos implementáveis |
| Arquitetura | [`_KB/03-architecture/`](_KB/03-architecture/), [`_KB/12-decisions/`](_KB/12-decisions/) | Limites, dados e decisões |
| Infraestrutura e entrega | [`_KB/03-architecture/aws.md`](_KB/03-architecture/aws.md), [`_KB/08-infrastructure/deployment-plan.md`](_KB/08-infrastructure/deployment-plan.md), [`_KB/09-devops/cicd-plan.md`](_KB/09-devops/cicd-plan.md) | AWS, implantação e automação |
| Engenharia e qualidade | [`_KB/04-development/`](_KB/04-development/), [`_KB/10-quality/`](_KB/10-quality/) | Execução, testes e demonstração |
| Planejamento | [`_KB/11-planning/`](_KB/11-planning/) | Calendário confirmado e roadmap |

| Documento | Atividade de origem | Descrição |
|---|---|---|
| [`02-eap/eap-fieldops.md`](02-eap/eap-fieldops.md) | Aula 3 | Estrutura Analítica do Projeto (EAP), com diagrama e status de cada pacote de trabalho (concluído / parcial / pendente) |
| [`03-cronograma/cronograma-fieldops.md`](03-cronograma/cronograma-fieldops.md) | Aula 4 | Cronograma com 65 itens, dependências, estimativas de duração, responsáveis e marcos do projeto |
| [`01-tap/TAP_FieldOps_Modelo_4.1.docx`](01-tap/TAP_FieldOps_Modelo_4.1.docx) | Aula 2 | Termo de Abertura original em Word; aprovação nele permanece em branco |

## Nota sobre o "PII deste semestre"

O TAP da Aula 2 pede dois documentos: um referente ao "PII deste semestre" e outro referente ao Projeto FieldOps. Neste caso os dois **são o mesmo projeto** — o FieldOps é o próprio Projeto Integrador II do semestre — por isso os dois TAPs tratam do FieldOps sob duas óticas complementares (acadêmica vs. produto), em vez de dois projetos distintos.

## Regra de leitura

Use requisitos e regras textuais para implementar o MVP. Código divergente é um item de correção documentado no diagnóstico. O diagrama de classes, a ficha e o cronograma anterior são referências históricas até serem reconciliados.
