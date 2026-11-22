# Qualidade e entrega

Cada alteração deve preservar o contrato REST, as regras de transição de inspeção e a auditabilidade das ações críticas. Migrações de banco são versionadas por Flyway; não altere uma migração já aplicada.

A API usa Java 21, Spring Boot, JUnit 5 e Testcontainers. Execute `mvn test` em `API/`; Docker é necessário para os testes de integração. O painel usa Angular, TypeScript estrito, Vitest e Prettier. Em `interface-administrativa/`, execute `npm test -- --watch=false`, `npm run build` e `npx prettier --check src` conforme a alteração.

Teste autorização, casos inválidos e transições de fluxo ao alterar domínios de inspeção. Para mudanças de interface, valide carregamento, erro, vazio e permissões por papel. Não registre senhas, tokens, evidências nem dados sensíveis nos logs.

Use commits focados com os prefixos existentes: `feat:`, `fix:`, `test:`, `docs:` ou `chore:`. Pull requests devem explicar o comportamento, registrar a validação executada e incluir capturas quando a interface mudar.
