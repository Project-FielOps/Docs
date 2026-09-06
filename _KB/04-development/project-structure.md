# Estrutura do projeto

Visão rápida dos diretórios relevantes no workspace:

```
/API
  └─ src/main/java/com/fieldops
/interface-administrativa
  └─ src/app
/Docs
  └─ _KB
  └─ references/base_fieldops
```

## Mapeamento de responsabilidades

- Lógica de negócio e regras: `API/src/main/java/com/fieldops/*`.
- Migrations: `API/src/main/resources/db/migration`.
- Frontend: `interface-administrativa/src/app`.
- Documentação e KB: `Docs/_KB` e `Docs/references`.

## Convenções

- Use `feature/` para branches de funcionalidade e `hotfix/` para correção urgente.
- Commits: `feat:`, `fix:`, `docs:`, `test:`, `chore:`.
- PRs: incluir ID de requisito/RN e testes executados.
