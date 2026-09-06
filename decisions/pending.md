# Pendências consolidadas do FieldOps

Atualizado em 2026-09-06. Este é o arquivo único para registrar pendências ainda abertas. Decisões aprovadas permanecem em `architecture/decisions.md`; este arquivo registra o que ainda precisa de detalhamento, implementação ou evidência de aceite.

## GAP-013 — evidências e upload — concluído

Validado em 2026-09-06 pelo fluxo Backend Agent → Validation & QA Agent. O backend agora valida extensão, MIME declarado e assinatura binária para JPEG/JPG, PNG e PDF; rejeita conteúdo falsificado; aplica 10 MB por arquivo e 10 evidências por inspeção; grava o MIME detectado; diferencia foto de documento; e expõe `GET /api/v1/inspections/{inspectionId}/evidence/{evidenceId}/content` com autorização por vínculo.

O storage continua selecionável por configuração: disco local no desenvolvimento e S3 privado em produção. A gravação ocorre antes do metadado, com remoção compensatória em falha de persistência/auditoria e callback de transação para falha de commit. O SDK AWS mantém retry padrão para operações S3; exclusões são idempotentes e best-effort.

## GAP-015 — recuperação de senha — concluído

Validado em 2026-09-06 pelo fluxo Backend Agent → Validation & QA Agent. Foram implementados os endpoints `POST /api/v1/auth/forgot-password` e `POST /api/v1/auth/reset-password`, token opaco de 32 bytes com somente hash SHA-256 persistido, validade de 30 minutos, uso único e invalidação de tokens anteriores. O envio usa Amazon SES via `EmailService`, configuração externa e cadeia padrão de credenciais AWS. A resposta é genérica para evitar enumeração, há limitação por IP/e-mail, redefinição transacional com revogação de todas as sessões e logs sem dados sensíveis. Senhas temporárias usam `mustChangePassword`, são informadas no login e bloqueiam recursos até `change-password`.

### Evidência operacional AWS Academy — SES

Durante a tentativa de configuração no AWS Academy, a role `voclabs` negou `ses:GetAccount`, `ses:TagResource` e `ses:CreateEmailIdentity`. Por isso, não foi possível criar ou verificar uma nova identidade remetente do SES nesse laboratório. A limitação é específica do ambiente AWS Academy e não altera a decisão arquitetural de utilizar Amazon SES em homologação e produção.

No desenvolvimento, deve ser utilizado o provider alternativo de `EmailService` (`noop`) ou uma identidade SES previamente provisionada por uma conta com permissões adequadas. Em ambiente AWS real, devem ser configurados uma identidade/domínio verificado, remetente externo, IAM Role com `ses:SendEmail`, região compatível e saída da sandbox quando necessária para produção.

## GAP-019 e DEC-013 — sincronização mobile — backend concluído

O backend implementa `POST /api/v1/mobile/sync/push` e `GET /api/v1/mobile/sync/pull`, com contrato, limites, idempotência persistida, dependências, conflitos, cursor e autenticação documentados em [`specs/synchronization.md`](../specs/synchronization.md). Testes de replay, autorização, cursor e falha de operação passaram.

Continuam pendentes apenas a implementação mobile da SQLite/outbox, testes no dispositivo e a resolução administrativa de conflitos no cliente.

## GAP-020 — cronograma e governança

O compromisso vigente é entrega acadêmica até o fim de novembro de 2026. Ainda falta evidência de um calendário operacional único, com:

- marcos de desenvolvimento, homologação e demonstração;
- responsáveis por aceite técnico e de produto;
- critério para considerar TAP, EAP e cronograma encerrados;
- orçamento, conta AWS e ambientes efetivamente disponíveis.

## GAP-021 e GAP-022 — documentação e catálogos

Permanecem pendências de governança documental:

- revisar referências históricas e navegação sem tratá-las como requisitos atuais;
- decidir a política de versionamento para `specs/` e demais documentos;
- mapear os 82 PBIs para cards derivados, caso seja necessária equivalência;
- registrar claramente quais catálogos são históricos, derivados ou vigentes.

## Infraestrutura AWS e operação

AWS é requisito do projeto, mas a arquitetura ainda é proposta. Faltam decisões operacionais e evidências de execução:

- serviços escolhidos entre ECR/ECS e alternativa equivalente;
- região, conta, IaC e orçamento;
- ambientes local, homologação e produção;
- bucket S3 privado, lifecycle, criptografia e política de acesso;
- Secrets Manager/IAM Role;
- HTTPS, logs, métricas, health check, backup e restauração testada;
- pipeline de teste, build, migração controlada e deploy.

## Qualidade e aceite

Ainda faltam evidências para concluir o aceite do produto:

- testes frontend e auditoria WCAG 2.1 AA;
- teste mobile de SQLite/outbox e permissões negadas;
- teste de carga para p95 de API ≤ 500 ms, excluindo transferências;
- inspeção de logs estruturados e correlation ID em ambiente executando (correlation ID implementado no backend; evidência de ambiente ainda pendente);
- demonstração ponta a ponta com evidência de aceite;
- matriz completa requisito → aceite → teste → evidência.

## Itens já definidos e não pendentes de decisão

Não são novas perguntas de produto: autorização por responsável, correção pós-reprovação, GPS/QR, formatos e limites de evidência, política de conflitos, prazo de revisão, dashboard mínimo, AWS como requisito e entrega até novembro já estão decididos. O que permanece aberto nesses temas é implementação, detalhamento técnico ou evidência de aceite, conforme as seções acima.
