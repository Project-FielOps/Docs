# Risco, findings e veredito

## Risco da mudança

- `LOW`: alteração interna isolada, sem regra, contrato, dados ou segurança. Testes direcionados e compilação.
- `MEDIUM`: validação localizada, query/paginação, cadastro ou DTO não público. Testes direcionados, integração do domínio e build.
- `HIGH`: API/DTO público, migration, entidade, transação, estado, template/snapshot, evidência, auditoria, integração, componente compartilhado, autenticação ou autorização. Matriz positiva/negativa, integrações relacionadas, regressão e build completo.
- `CRITICAL`: potencial de bypass de vínculo, privilege escalation, edição após aprovação, perda/corrupção de dados, migration destrutiva, quebra de idempotência, segredo/evidência pública ou indisponibilidade sistêmica. Exija validação ampla e nenhum finding crítico/alto aberto.

Eleve o risco quando alcance ou incerteza forem maiores que o diff sugere.

## Severidade

- `CRITICAL`: bypass de segurança, segredo exposto, perda/corrupção ou indisponibilidade sistêmica.
- `HIGH`: requisito obrigatório violado, autorização por objeto incorreta, contrato quebrado, transição inválida ou migration insegura.
- `MEDIUM`: cenário relevante sem tratamento, regressão localizada, validação ou transação incompleta.
- `LOW`: defeito real de impacto/probabilidade baixos.
- `IMPROVEMENT`: melhoria opcional; não bloqueia isoladamente.

Formato:

```text
[SEVERITY] Título
Requirement/source: ...
Location: ...
Evidence: ...
Impact: ...
Reproduction: ...
Expected correction: ...
```

## Veredito

- `APPROVED`: todos os gates aplicáveis passaram; nenhum finding além de `IMPROVEMENT`; evidência suficiente.
- `APPROVED_WITH_WARNINGS`: comportamento aceito e seguro; apenas `LOW`/`IMPROVEMENT` ou limitação externa registrada.
- `CHANGES_REQUIRED`: qualquer `CRITICAL`/`HIGH`; `MEDIUM` que viole aceite; teste obrigatório ou build falhando por causa da mudança; evidência insuficiente para risco alto.
- `BLOCKED_BY_DOCUMENTATION`: lacuna/conflito normativo permite comportamentos materialmente diferentes e impede julgamento correto.

Falha ambiental é registrada separadamente e não vira automaticamente bloqueio documental.

## Definition of Done

- repositório, escopo, RN/FR/AC e fontes identificados;
- gaps/conflitos críticos resolvidos ou bloqueados;
- impactos diretos e indiretos analisados;
- contrato `/api/v1`, autorização por papel/objeto, transições e imutabilidade validados;
- transação, concorrência, migration e privacidade de evidências avaliadas quando aplicáveis;
- auditoria e erros sem dados sensíveis;
- testes do requisito e adversariais executados;
- `mvn test` e `mvn package` passando para backend relevante;
- OpenAPI e documentação coerentes;
- nenhum finding `CRITICAL` ou `HIGH` aberto;
- relatório e veredito independentes produzidos.

Checks inexistentes de lint, cobertura, dependências ou SAST devem aparecer como `DOCUMENTATION_GAP`/não configurado, nunca como passes.
