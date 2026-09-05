# Guia de desenvolvimento

## Antes de alterar código

Leia `Docs/knowledge-base/README.md`, `product/mvp.md`, a especificação do domínio e `architecture/decisions.md`. Identifique RN/FR/AC afetados. Se a alteração for nova decisão, atualize ADR antes do contrato.

## Repositórios

API usa Java 21, Spring Boot 3.3, camadas por contexto, DTO separado de JPA, Flyway e PostgreSQL. Painel usa Angular standalone, TypeScript estrito, serviços tipados, guards e interceptors. Mobile futuro usa Expo/RN, SQLite operacional, Secure Store e outbox. Não coloque regra apenas na UI.

## Comandos locais

API: em `API/`, `docker compose up -d`, `mvn test`, `mvn package`. Web: em `interface-administrativa/`, `npm ci`, `npm test -- --watch=false`, `npm run build`, `npx prettier --check src`. O Docker é necessário aos testes Testcontainers. Não há comando válido para executar mobile ainda.

## Git e revisão

Use `feat:`, `fix:`, `test:`, `docs:` ou `chore:` em commits focados. PR deve indicar comportamento, IDs afetados, migração/contrato, validação executada, limitações e captura de tela quando houver UI.
