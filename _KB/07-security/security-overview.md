# Segurança

## Objeto da segurança

A segurança do projeto deve garantir:

- autenticação de usuários ativos;
- autorização por perfil e por objeto;
- rastreabilidade de ações críticas;
- proteção de credenciais e tokens;
- utilização segura de arquivos e evidências;
- segregação clara entre usuários, escopos e inspeções.

## Regras obrigatórias

- credenciais e tokens não devem ser gravados em logs;
- troca de senha ou bloqueio do usuário deve invalidar sessões conforme política definida pela API;
- somente usuários ativos podem acessar a solução;
- a autorização real deve ser validada na API;
- desvios de implementação devem ser apontados como pendências de correção.

## Status atual

A base documental exige essa política mesmo que a implementação atual ainda não a tenha alcançado plenamente. A segurança deve ser tratada como requisito obrigatório e não como detalhe opcional.

## Documentação relacionada

- [../05-api/api-overview.md](../05-api/api-overview.md)
- [../12-decisions/ADR-002-authorization.md](../12-decisions/ADR-002-authorization.md)
- [../12-decisions/ADR-007-session-logout.md](../12-decisions/ADR-007-session-logout.md)
