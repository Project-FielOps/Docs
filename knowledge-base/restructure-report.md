# Relatório da reestruturação

## Criados

`knowledge-base/discovery.md`, `knowledge-base/sources.md`, `knowledge-base/README.md`, `references/` com cópias e manifesto; `product/overview.md`, `product/mvp.md`, `product/business-rules.md`, `product/glossary.md`; `requirements/functional.md`, `requirements/non-functional.md`, `requirements/traceability.md`; `specs/README.md`, `specs/authentication.md`, `specs/inspections.md`, `specs/synchronization.md`; `architecture/data-model.md`; `decisions/pending.md`; guias de engenharia, demonstração e planejamento.

## Alterados

`README.md` passou a ser o mapa da base e deixou de apontar para `base.md`/TAPs Markdown inexistentes. `architecture/decisions.md` recebeu decisões confirmadas; `discovery.md` recebeu respostas de 05/09.

## Preservados

Os capítulos originais, diagrama, ficha, TAP, EAP, cronograma e catálogos derivados não foram apagados. Uma cópia verificável está em `references/`; os arquivos de gestão permanecem nos caminhos originais.

## Principais melhorias

- fonte de verdade explícita e separação entre requisito, implementação, proposta e pendência;
- RN/FR/AC/OBJ rastreáveis sem duplicar as 90 regras;
- estados, autorização, correção, GPS/QR e sync fechados conforme respostas do responsável;
- modelo conceitual separado do diagrama divergente;
- plano atualizado para novembro e AWS sem declarar infraestrutura inexistente;
- diagnóstico com 25 gaps de código/documentação e riscos acionáveis.

## Pendências restantes

DEC-004/005/006/007/010/011/012 continuam abertas. Faltam mobile, sync implementado, pipeline/deploy AWS, testes frontend e evidência de demonstração. O próximo passo recomendado é transformar DEC-002/003/013 em issues técnicas e corrigir autorização por objeto antes de construir o mobile.
