# Planejamento e handoff do Backend Agent

## Plano antes da implementação

Use este conteúdo, ajustando a forma ao tamanho da mudança:

```text
REQUIREMENT: RN/FR/AC e objetivo
RELEVANT_DOCS: fontes normativas e decisões
EXPECTED_BEHAVIOR: comportamento observável
CURRENT_BEHAVIOR: evidência encontrada no código/testes
AFFECTED_MODULES: contextos e integrações
LIKELY_FILES: arquivos prováveis, sem tratar previsão como compromisso
DEPENDENCIES_AND_CONSUMERS: produtores, consumidores e efeitos indiretos
API_IMPACT: endpoint, DTO, status, erro, paginação e OpenAPI
DATABASE_IMPACT: entidade, query, transação, índice e migration
SECURITY_IMPACT: autenticação, papel, vínculo, IDOR e dados sensíveis
RISKS: regressão, concorrência, integridade e operação
TEST_PLAN: unitário, integração, contrato e regressão
OPEN_GAPS: DOCUMENTATION_GAP / DOCUMENTATION_CONFLICT
IMPLEMENTATION_STEPS: passos pequenos e verificáveis
```

## Handoff

```text
READY_FOR_VALIDATION

Requirements: ...
Behavior delivered: ...
Files changed: ...
Contract/data/security impacts: ...
Tests added or updated: ...
Commands and results: ...
Known limitations: ...
Documentation markers: ...
Residual risks: ...
```

Se documentação precisar mudar, use:

```text
DOCS_CHANGE_REQUIRED
Document: ...
Current documented behavior: ...
Expected/new behavior: ...
Reason: ...
Impact: ...
Decision owner needed: ...
```
