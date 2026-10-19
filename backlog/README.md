# Backlog executável do FieldOps

Este diretório transforma as pendências encontradas na integração API, painel web e aplicativo mobile em tasks executáveis por agents.

## Regras de execução

1. Ler a task, a documentação normativa em `Docs/_KB/` e o contrato OpenAPI antes de alterar código.
2. Não simular endpoints ausentes, respostas de API ou estados de sincronização no caminho de produção.
3. Toda alteração de frontend deve incluir testes unitários/componente e uma evidência E2E.
4. O painel web deve usar Playwright para jornadas de navegador, incluindo estados de sucesso, erro, loading, vazio, 401, 403 e 409 quando aplicável.
5. O mobile deve usar testes de unidade/componente e um runner E2E compatível com o ambiente Android disponível, como Maestro ou Detox. Playwright pode validar fluxos web/API, mas não substitui o teste no dispositivo.
6. Testes E2E devem executar contra API real ou um servidor de contrato explicitamente versionado; mocks só podem existir em testes unitários.
7. Cada task deve registrar comandos executados, ambiente, dados de teste e resultado.
8. Não marcar task como `DONE` apenas porque o código existe. Exigir critérios de aceite e evidências.
9. Mudanças de contrato, autorização, banco, sync ou segurança devem atualizar a documentação correspondente em `Docs/_KB/`.
10. Commits devem ser pequenos e seguir `feat:`, `fix:`, `test:`, `docs:` ou `chore:`.

## Fluxo de estados

`BACKLOG → READY → IN_PROGRESS → VALIDATION → DONE`

Use `BLOCKED` somente quando a dependência estiver claramente identificada e não houver trabalho seguro que possa avançar sem ela.

## Gatilhos mínimos por tipo

| Área | Validação obrigatória |
| --- | --- |
| Angular web | `npm test -- --watch=false`, `npm run build`, `npx prettier --check src`, Playwright desktop/mobile e teclado |
| React Native mobile | typecheck/build, testes de componentes/serviços, teste E2E em Android ou runner aprovado, reinício offline quando aplicável |
| API | `mvn test`, testes de autorização/contrato e integração com PostgreSQL/Testcontainers quando aplicável |
| Jornada integrada | API real + web Playwright + mobile E2E, com registro de usuários, IDs, estados e resultado |

## Índice

| ID | Task | Prioridade | Estado inicial | Arquivo |
| --- | --- | --- | --- | --- |
| FO-001 | Contratos de sessão e tratamento de erros no web | P0 | READY | [FO-001.md](FO-001.md) |
| FO-002 | Planejamento e usuários atribuíveis | P0 | BLOCKED | [FO-002.md](FO-002.md) |
| FO-003 | Detalhe, revisão e evidências no web | P0 | IN_PROGRESS | [FO-003.md](FO-003.md) |
| FO-004 | Dashboard, listas e filtros reais | P0/P1 | IN_PROGRESS | [FO-004.md](FO-004.md) |
| FO-005 | Recuperação e troca de senha | P0 | READY | [FO-005.md](FO-005.md) |
| FO-006 | Qualidade frontend, acessibilidade e regressão visual | P0 | READY | [FO-006.md](FO-006.md) |
| FO-007 | Persistência offline do mobile | P0 | BLOCKED | [FO-007.md](FO-007.md) |
| FO-008 | Sync incremental, NC e conflitos | P0 | BLOCKED | [FO-008.md](FO-008.md) |
| FO-009 | QR, GPS e evidências idempotentes | P0 | BLOCKED | [FO-009.md](FO-009.md) |
| FO-010 | Autorização e proteção de dados/evidências | P0 | READY | [FO-010.md](FO-010.md) |
| FO-011 | Integração E2E da jornada vertical | P0 | BLOCKED | [FO-011.md](FO-011.md) |
| FO-012 | Publicação e operação do frontend | P1 | BLOCKED | [FO-012.md](FO-012.md) |

O inventário resumido e a matriz de dependências estão em [pendencias.md](pendencias.md).
