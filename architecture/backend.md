# Arquitetura do backend

## Estado atual

A API está em `API/`, usa Java 21, Spring Boot 3.3, Spring Security com JWT, Spring Data JPA, PostgreSQL e Flyway. O contrato público inicia em `/api/v1`; Swagger está em `/swagger-ui.html` quando a aplicação está em execução.

## Organização

Os pacotes seguem o domínio: `auth`, `user`, `client`, `site`, `equipment`, `template`, `inspection`, `evidence`, `review`, `audit`, `dashboard` e `shared`. Em cada contexto, separar controller, application, domain, repository, dto e mapper. Controllers adaptam HTTP; serviços de aplicação aplicam caso de uso e regras; domínio não depende de detalhes HTTP.

## Contrato HTTP

Use autenticação Bearer, validação no servidor e autorização por endpoint. Listagens usam `page`, `size` e `sort`. Erros seguem `ApiError` com `timestamp`, `status`, `error`, `code`, `message`, `path` e `details`. Alterações críticas geram `AuditEvent`; logs não incluem senha, token, nem conteúdo de evidências.

## Persistência e testes

Migrações ficam em `src/main/resources/db/migration`. O schema é validado pela aplicação, nunca recriado automaticamente fora de ambiente descartável. Testes de integração usam JUnit 5, Spring Boot Test, Spring Security Test e PostgreSQL via Testcontainers. Ao mudar uma transição, validar cenários autorizados, recusados e auditados.

O contrato aprovado de sincronização está em `../specs/synchronization.md` e `decisions.md` como DEC-013. Os endpoints ainda não estão implementados; não afirmar suporte mobile até haver push, pull, upload separado e testes de replay.
