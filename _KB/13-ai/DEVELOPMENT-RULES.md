# Development Rules

## Coding standards

- Java: seguir convenções do projeto Spring Boot, evitar uso de `lombok` em APIs públicas sem justificativa; preferir DTOs explícitos para contratos.
- TypeScript/Angular: manter `strict` e usar serviços para integração com a API.
- Tests: cada comportamento crítico (segurança, publicação, snapshot, sync) deve ter cobertura de unidade e integração.

## Branching and commits

- Branches: `feat/`, `fix/`, `chore/`, `test/`, `hotfix/`.
- Commits: usar convenções `feat:`, `fix:`, `docs:` e referenciar IDs de requisito (ex: `feat: add inspection snapshot (FR-TEMPLATE-002)`).
- PR: descrever alteração, requisitos afetados, ADRs relacionadas e testes executados.

## PR checklist

- [ ] Revisar ADRs afetadas
- [ ] Atualizar documentação na KB quando comportamento mudar
- [ ] Incluir/atualizar testes automatizados
- [ ] Validar migrações Flyway em ambiente de staging
- [ ] Verificar não expor credenciais ou tokens

## Testing requirements

- Unit tests para lógica de validação do template e regras de resposta
- Integration tests com Testcontainers para fluxos de inspeção
- Contracts: manter OpenAPI atualizado e testado

## Deployment notes

- Não alterar configurações sensíveis no repositório
- Usar variáveis de ambiente e secrets manager em CI/CD

## Pairing with agents

- Agents podem propor mudanças e abrir PRs, mas não podem alterar ADRs sem aprovação humana.
- Agentes devem criar descrições claras, incluir testes e apontar divergências entre código e `base_fieldops`.
