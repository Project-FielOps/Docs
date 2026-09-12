# Estratégia do FieldOps Web QA Agent

## Sequência de validação

1. Resolva requisito, regras, design system e contrato esperado.
2. Inspecione status, diff e handoff sem assumir que estejam completos.
3. Classifique o risco e expanda para consumidores, rotas e integrações.
4. Monte a matriz de estados, papéis, viewports e endpoints.
5. Revise código e testes do autor.
6. Execute testes direcionados e gates completos.
7. Inspecione runtime, responsividade, teclado e design.
8. Execute integração real com a API quando disponível.
9. Consolide evidências, findings e um único veredito.

## Matriz mínima

```text
Requirement/source: ...
Route/component: ...
Role/domain state: ...
UI state: ...
Viewport: ...
API endpoint/error: ...
Test or inspection: ...
Evidence/result: ...
```

Amplie a matriz proporcionalmente ao risco. Mudanças de token, tema, componente compartilhado, autenticação, interceptor, formulário base, tabela base ou contrato HTTP exigem regressão em múltiplos consumidores.

## Evidência aceitável

- teste automatizado executado com resultado registrado;
- inspeção de runtime identificando rota, viewport, papel e estado;
- chamada real à API com entrada e resultado não sensíveis;
- inspeção estática claramente limitada ao que ela prova.

Build não prova comportamento, screenshot não prova interação e mock não prova integração.

## Regra de veredito

- `APPROVED`: todos os gates aplicáveis passaram, cenários críticos têm evidência e não há finding aberto relevante.
- `APPROVED_WITH_WARNINGS`: apenas riscos baixos, limitações externas ou melhorias não obrigatórias, explicitamente registrados.
- `CHANGES_REQUIRED`: há finding crítico/alto/médio que viola requisito, design system, integração ou qualidade obrigatória; gate da mudança falhou; ou falta teste direto relevante sem justificativa.
- `BLOCKED_BY_DOCUMENTATION`: lacuna ou conflito normativo permite resultados materialmente diferentes e impede avaliação correta.
