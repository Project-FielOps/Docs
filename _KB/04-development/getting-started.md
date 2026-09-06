# Getting Started — Desenvolvimento local

## Pré-requisitos

- Java 21
- Maven 3.9+
- Node.js / npm (para frontend)
- Docker (para Postgres em desenvolvimento e testes com Testcontainers)

## Backend

No diretório `API/`:

```bash
# Subir banco de desenvolvimento
docker compose up -d

# Rodar testes
mvn test

# Rodar aplicação
mvn -DskipTests package
java -jar target/fieldops-api-0.1.0.jar
```

Notas:
- As migrações Flyway estão em `src/main/resources/db/migration`.
- Perfil `dev` cria seed de usuários e dados básicos se o banco estiver vazio.

## Frontend

No diretório `interface-administrativa/`:

```bash
npm ci
npm start
```

## Mobile

O mobile é parte do MVP mas não está implementado neste repositório. Documentação de integração e contrato de sync se encontra em `Docs/_KB/12-decisions/ADR-006-mobile-and-sync.md`.

## Verificação rápida

- API: acionar `http://localhost:8080/swagger-ui.html` após subir a aplicação.
- Banco: verificar `localhost:5433` conforme `API/docker-compose.yml`.
