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

As decisões de produto foram definidas, mas permanecem pendências de detalhamento, implementação e evidência de aceite. A lista vigente está consolidada em [decisions/pending.md](../../../decisions/pending.md). O GAP-013 é explicado ali: existe upload em disco/S3, mas ainda faltam validações finais, limite por inspeção, download autorizado e consistência arquivo-banco. O próximo passo deve ser priorizado a partir desse arquivo, sem tratar decisões já aprovadas como perguntas novamente.
