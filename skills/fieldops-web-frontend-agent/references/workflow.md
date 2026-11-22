# Planejamento e handoff do Web Frontend Agent

## Plano para mudança relevante

```text
REQUIREMENT: RN/FR/AC e objetivo
RELEVANT_DOCS: fontes normativas e decisões
DESIGN_SYSTEM_RULES: tokens, componentes, estados e padrões aplicáveis
EXPECTED_BEHAVIOR: comportamento observável por papel e estado
CURRENT_BEHAVIOR: evidência no frontend e no backend
AFFECTED_SURFACE: rotas, páginas, componentes e padrões compartilhados
API_CONTRACT: endpoint, DTO, paginação, erros e autorização
STATE_MATRIX: loading/empty/no-results/error/forbidden/processing/success
RESPONSIVE_AND_A11Y: breakpoints, teclado, foco, contraste e leitura
RISKS: regressão visual, funcional, integração e autorização
TEST_PLAN: componente, serviço HTTP, regressão, visual e integração
OPEN_GAPS: documentação, design system ou contrato
IMPLEMENTATION_STEPS: incrementos pequenos e verificáveis
```

Mudança é relevante quando afeta fluxo de usuário, contrato HTTP, autenticação/autorização, componente ou token compartilhado, navegação, formulário complexo, tabela, responsividade ou mais de uma feature.

## Handoff

```text
READY_FOR_WEB_QA

Requirements and sources: ...
Behavior delivered: ...
Files/routes changed: ...
Design system conformance: ...
API endpoints and contract: ...
State matrix covered: ...
Responsive/accessibility evidence: ...
Tests added or updated: ...
Commands and results: ...
Known limitations: ...
Documentation/design markers: ...
Residual risks: ...
```

## Encerramento após QA

Com `APPROVED` ou `APPROVED_WITH_WARNINGS`, revise o diff aprovado, crie o commit local apenas com os arquivos da entrega e informe hash e mensagem. Use o prefixo convencional adequado. Não execute `git push`.
