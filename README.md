# Docs — Projeto FieldOps

Esta é a base de conhecimento versionada do FieldOps: produto, regras, requisitos, especificações implementáveis, arquitetura, engenharia, decisões e planejamento acadêmico. O código vive em `API/` e `interface-administrativa/`; o app mobile ainda é uma frente planejada.

## Roteiro de leitura

1. [Visão do produto](product/overview.md) e [MVP](product/mvp.md) — o que o sistema deve fazer.
2. [Arquitetura geral](architecture/overview.md) — limites entre API, painel, mobile e dados.
3. [Arquitetura AWS](architecture/aws.md) — topologia de produção e responsabilidades de infraestrutura.
4. [Plano de implantação](engineering/deployment-plan.md) — sequência para backend, banco, S3 e frontend.
5. [Plano de CI/CD](engineering/cicd.md) — validação local e promoção via GitHub Actions.
6. [Especificações](specs/README.md) — contratos implementáveis por domínio.

Para agentes de IA, leia também [regras de desenvolvimento](_KB/13-ai/DEVELOPMENT-RULES.md), [mapa do código](_KB/13-ai/CODEBASE-MAP.md) e [fonte de verdade](knowledge-base/README.md). Ao implementar, diferencie sempre requisito, decisão, estado observado e evidência de validação.

## Conteúdo

As fontes originais foram preservadas em [`references/`](references/README.md). A precedência, os estados do conhecimento e o método de manutenção estão em [`knowledge-base/README.md`](knowledge-base/README.md). O diagnóstico completo está em [`knowledge-base/discovery.md`](knowledge-base/discovery.md).

| Área | Documento | Uso |
|---|---|---|
| Diagnóstico e fontes | [`knowledge-base/discovery.md`](knowledge-base/discovery.md), [`knowledge-base/sources.md`](knowledge-base/sources.md) | Evidências, lacunas, conflitos e inventário |
| Produto | [`product/overview.md`](product/overview.md), [`product/mvp.md`](product/mvp.md), [`product/business-rules.md`](product/business-rules.md) | Problema, escopo, regras e vocabulário |
| Requisitos | [`requirements/functional.md`](requirements/functional.md), [`requirements/non-functional.md`](requirements/non-functional.md), [`requirements/traceability.md`](requirements/traceability.md) | IDs, aceites e rastreabilidade |
| Especificações | [`specs/README.md`](specs/README.md) | Casos de uso por domínio e contratos |
| Arquitetura | [`architecture/overview.md`](architecture/overview.md), [`architecture/data-model.md`](architecture/data-model.md), [`architecture/decisions.md`](architecture/decisions.md) | Limites, dados e decisões |
| Infraestrutura e entrega | [`architecture/aws.md`](architecture/aws.md), [`engineering/deployment-plan.md`](engineering/deployment-plan.md), [`engineering/cicd.md`](engineering/cicd.md) | AWS, implantação e automação |
| Engenharia | [`engineering/development-guide.md`](engineering/development-guide.md), [`engineering/testing-strategy.md`](engineering/testing-strategy.md), [`engineering/demo.md`](engineering/demo.md) | Execução, testes e demonstração |
| Planejamento | [`planning/academic.md`](planning/academic.md), [`planning/mvp.md`](planning/mvp.md) | Calendário confirmado e roadmap |

| Documento | Atividade de origem | Descrição |
|---|---|---|
| [`02-eap/eap-fieldops.md`](02-eap/eap-fieldops.md) | Aula 3 | Estrutura Analítica do Projeto (EAP), com diagrama e status de cada pacote de trabalho (concluído / parcial / pendente) |
| [`03-cronograma/cronograma-fieldops.md`](03-cronograma/cronograma-fieldops.md) | Aula 4 | Cronograma com 65 itens, dependências, estimativas de duração, responsáveis e marcos do projeto |
| [`01-tap/TAP_FieldOps_Modelo_4.1.docx`](01-tap/TAP_FieldOps_Modelo_4.1.docx) | Aula 2 | Termo de Abertura original em Word; aprovação nele permanece em branco |

## Nota sobre o "PII deste semestre"

O TAP da Aula 2 pede dois documentos: um referente ao "PII deste semestre" e outro referente ao Projeto FieldOps. Neste caso os dois **são o mesmo projeto** — o FieldOps é o próprio Projeto Integrador II do semestre — por isso os dois TAPs tratam do FieldOps sob duas óticas complementares (acadêmica vs. produto), em vez de dois projetos distintos.

## Regra de leitura

Use requisitos e regras textuais para implementar o MVP. Código divergente é um item de correção documentado no diagnóstico. O diagrama de classes, a ficha e o cronograma anterior são referências históricas até serem reconciliados.
