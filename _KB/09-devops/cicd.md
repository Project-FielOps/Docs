# CI/CD

## Recomendações

- pipeline com lint, build, testes unitários e integração;
- deploy automatizado para dev/staging/prod;
- migrações Flyway automáticas com validação pré-deploy;
- jobs para backup e verificação de integridade de evidências.

## Observações

- configurar variáveis secretas em providers e não no repositório;
- rodar testes de integração com Testcontainers em pipeline para garantir comportamento do banco.

## Referências

- `API/pom.xml`;
- pipeline a ser definido e documentado em `Docs/_KB/09-devops/deployment.md`.
