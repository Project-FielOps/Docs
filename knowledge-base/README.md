# Fontes e manutenção da base

## Autoridade de cada informação

1. **Produto e regras:** capítulos textuais e decisões consolidadas de `base_fieldops`, por confirmação do responsável em 2026-09-05. A documentação consolidada em `product/`, `requirements/` e `specs/` deriva dessas fontes e preserva seus identificadores.
2. **Estado implementado:** código local da API e do painel. Um comportamento contrário à regra é um desvio a corrigir, não uma aprovação implícita de nova regra.
3. **Decisões novas:** somente decisões explicitamente confirmadas recebem esse estado em `decisions/`. Propostas e perguntas continuam identificadas como tal.
4. **Gestão acadêmica:** TAP, EAP e cronograma são artefatos datados. Aprovação, calendário e responsáveis atuais exigem confirmação quando houver conflito.
5. **Diagrama, ficha e análise anterior:** referências históricas em `references/`. Não introduzem funcionalidades no MVP por si só.

## Vocabulário de status

- **Requisito da base:** comportamento esperado explicitamente documentado.
- **Implementação observada:** encontrado na leitura estática do código em 2026-09-05.
- **Teste existente:** código de teste encontrado; não significa executado nesta revisão.
- **Proposta:** alternativa técnica ou de organização ainda sem decisão de produto.
- **Pendente:** informação ou validação que falta; ausência de resposta não constitui aprovação.

## Manutenção

Cada regra tem uma única definição em [regras de negócio](../product/business-rules.md). Cada requisito funcional e critério de aceitação é definido na especificação do seu domínio; índices e matriz apontam para ela. Preserve RN, UC, PBI e os aliases BR/FR/AC. Não reutilize IDs removidos.

Ao alterar um comportamento: identificar requisito e regra, registrar decisão se houver mudança de escopo, atualizar especificação, contrato e teste, e registrar evidência de validação. Alterações de implementação não devem apagar o comportamento pretendido nem o desvio documentado.

[Diagnóstico](discovery.md) registra as evidências da análise inicial. [Decisões pendentes](../decisions/pending.md) concentra as perguntas abertas. [Relatório](restructure-report.md) descreve esta reorganização.
