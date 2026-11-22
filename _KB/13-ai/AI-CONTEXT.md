# AI Context

## Objetivo do projeto

O FieldOps é uma plataforma para planejar, executar e revisar inspeções técnicas em campo, substituindo formulários físicos e processos fragmentados por um fluxo digital com rastreabilidade, evidência e revisão.

## Fonte de verdade

A documentação original em `base_fieldops/Projeto FieldOps` é a fonte normativa do produto e do domínio. O código nos repositórios representa o estado atual do sistema e deve ser tratado como AS-IS, não como padrão definitivo quando divergir da base documental.

## Arquitetura

- backend: Java 21 + Spring Boot;
- frontend administrativo: Angular;
- banco: PostgreSQL;
- migrações: Flyway;
- API: REST /api/v1;
- segurança: Spring Security + JWT;
- mobile: parte do MVP, mas não totalmente implementado;
- AWS e storage de objetos: requisitos obrigatórios do projeto.

## Domínio

Entidades centrais:

- usuário;
- cliente;
- local;
- equipamento;
- template e versão;
- inspeção;
- snapshot;
- resposta;
- evidência;
- não conformidade;
- revisão;
- auditoria.

## Regras fundamentais

- somente usuários ativos podem iniciar sessão;
- autorização por perfil e por objeto é obrigatória;
- técnico atribuído executa e responde somente sua inspeção;
- supervisor atribuído revisa somente o seu escopo;
- template publicado é imutável;
- inspeção preserva snapshot;
- reprovação pode reabrir a inspeção para correção;
- evidência precisa estar vinculada à inspeção;
- qualquer divergência entre código e documentação deve ser registrada como desvio a corrigir.

## Componentes críticos

- auth;
- template versioning;
- inspection flow;
- review workflow;
- evidence handling;
- audit trail.

## Restrições

- não transformar hipótese em fato;
- não tratar proposta como requisito concluído;
- não misturar AS-IS com TO-BE;
- não ignorar documentação histórica, mas manter seu status de legado.

## Documentação relacionada

- [../README.md](../README.md)
- [../12-decisions/README.md](../12-decisions/README.md)
- [../99-reference/open-questions.md](../99-reference/open-questions.md)
- [../99-reference/diagnostics/discovery.md](../99-reference/diagnostics/discovery.md)
