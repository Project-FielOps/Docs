# Validação independente das correções do FieldOps

Data: 2026-09-06
Escopo: API Spring Boot e documentação de contrato afetada pelos GAPs implementáveis.

## Handoff do Backend Agent

`READY_FOR_VALIDATION`: foram corrigidos os desvios de autorização por papel/vínculo, reabertura após reprovação, histórico de respostas, validação tipada, NC/evidência, proteção de entidades aprovadas, vínculo item-seção, seleção de versão, filtros administrativos, status de usuário em JWT e formato de erros.

## Revisão Validation & QA Agent

Cobertura: controllers, serviços, repositories, entidades relacionadas, transições, DTOs, auditoria, filtros, caminho cruzado e autenticação. Nesta rodada, foram incluídos validação e download de evidências, limites, conteúdo binário, compensação storage/banco, endpoint de usuários atribuíveis, seleção de storage local/S3 e sessões de autenticação.

Casos verificados: ADMIN em operações de execução/revisão, técnico/supervisor não atribuídos, acesso por URL cruzada, resposta incompatível, `REJECTED` sem reabertura, NC crítica sem descrição/evidência, edição após aprovação, seção/item divergente, versão publicada não ativa, filtros de prioridade/período, usuário inativado após emissão de token, supervisor consultando técnicos ativos, técnico bloqueado da consulta de atribuíveis, composição exclusiva do storage local, rotação/reuso de refresh token, logout e alteração de senha.

Comandos executados:

- `JAVA_HOME=/usr/lib/jvm/java-21-temurin-jdk mvn -Dmaven.repo.local=/tmp/fieldops-m2 test` — `BUILD SUCCESS`, 10 testes, 0 falhas, 0 erros.
- `git diff --check` — sem erros de whitespace.
- `mvn -Dmaven.repo.local=/tmp/fieldops-m2 -Dtest=AuthenticationTest test` — `BUILD SUCCESS`, 9 testes, 0 falhas, 0 erros.
- `mvn -Dmaven.repo.local=/tmp/fieldops-m2 package -DskipTests` — `BUILD SUCCESS` com o SDK S3.
- `mvn -Dmaven.repo.local=/tmp/fieldops-m2 test` — `BUILD SUCCESS`, 14 testes, 0 falhas, 0 erros.
- `mvn -Dmaven.repo.local=/tmp/fieldops-m2 test` — `BUILD SUCCESS`, 16 testes, 0 falhas, 0 erros.
- `mvn -Dmaven.repo.local=/tmp/fieldops-m2 -Dtest=EvidenceServiceTest,EvidenceStorageTest test` — `BUILD SUCCESS`, 7 testes, 0 falhas, 0 erros.
- `mvn -Dmaven.repo.local=/tmp/fieldops-m2 test` — `BUILD SUCCESS`, 21 testes, 0 falhas, 0 erros.
- `mvn -Dmaven.repo.local=/tmp/fieldops-m2 package -DskipTests` — `BUILD SUCCESS`.

## Veredito

`APPROVED_WITH_WARNINGS`

Os GAPs GAP-001..004, GAP-011..014, GAP-016..018 e GAP-023..025 foram considerados corrigidos. GAP-013 foi aprovado após validação dos limites, MIME/assinatura, download autorizado e compensação transacional. GAP-015 foi aprovado após validação independente do fluxo completo de recuperação, SES configurável, token seguro, reset transacional e senha temporária. GAP-019 permanece parcial apenas quanto ao mobile. Não foram aprovados como implementação os GAPs documentais, cronograma, catálogos e lacunas de frontend.

Risco residual: não há reconciliação automática para o caso extremo de falha do processo exatamente antes do callback de transação; a compensação cobre falhas de persistência, auditoria e commit, e deve ser complementada por monitoramento operacional antes do release.

## Revalidação do item 4 — qualidade e aceite técnico

### Matriz requisito → símbolo/endpoint → teste → resultado

| Requisito | Símbolo/endpoint | Teste executado | Resultado |
|---|---|---|---|
| Health check público | `GET /actuator/health` | `AuthenticationTest.healthEndpointIsPublicAndReturnsCorrelationId` | Passou; HTTP 200 e `status=UP` |
| Correlation ID informado pelo cliente | `RequestCorrelationFilter`, `ApiError` | `AuthenticationTest.loginWithWrongPasswordReturns401WithStandardErrorBody` | Passou; o mesmo ID aparece no header e em `requestId` |
| Correlation ID seguro quando inválido | `RequestCorrelationFilter` | `AuthenticationTest.invalidRequestIdIsReplacedWithSafeCorrelationId` | Passou; valor inválido é substituído por UUID |
| Propagação para logs | MDC `requestId` e `logging.pattern.level` | Inspeção estática da configuração + execução da aplicação de teste | Passou; o padrão inclui `[requestId:%X{requestId}]` |
| Regressão backend | Todos os módulos | `mvn clean test` | Passou; 23 testes, 0 falhas, 0 erros |
| Empacotamento | Artefato Spring Boot | `mvn clean package` | Passou; JAR executável gerado |

### Veredito independente

`APPROVED_WITH_WARNINGS`

O núcleo implementável do item 4 no backend foi concluído: health check, correlação entre requisição/resposta/erro/log e testes de regressão. Permanecem como evidências operacionais, fora do escopo de um teste de integração local, a medição de p95 ≤ 500 ms sob carga representativa, a inspeção em ambiente AWS e a demonstração ponta a ponta com mobile.

## Revalidação do GAP-019 — sincronização backend

### Matriz requisito → símbolo/endpoint → teste → resultado

| Requisito | Símbolo/endpoint | Teste executado | Resultado |
|---|---|---|---|
| Push autenticado para técnico | `POST /api/v1/mobile/sync/push` | `SyncIntegrationTest.pushPersistsRejectedOperationAndReplayIsIdempotent` | Passou |
| Idempotência server-side | `SyncOperationRepository` | Mesmo teste, segundo envio do mesmo `operationId` | Passou; `ALREADY_APPLIED` |
| Pull com cursor inicial | `GET /api/v1/mobile/sync/pull` | `SyncIntegrationTest.pullStartsAtZeroAndReturnsNextCursor` | Passou; cursor `0` |
| Papel incorreto | `SyncController` | `SyncIntegrationTest.nonTechnicianCannotUseSync` | Passou; HTTP 403 |
| Cursor inválido | `SyncService.parseCursor` | `SyncIntegrationTest.invalidCursorIsRejected` | Passou; `INVALID_SYNC_CURSOR` |
| Migration e integração JPA | `V4__mobile_sync.sql` | `mvn clean test` e `mvn clean package` | Passou; 27 testes e JAR gerado |

### Veredito independente

`APPROVED_WITH_WARNINGS`

O contrato aprovado foi implementado para as operações online suportadas. A cobertura mobile da SQLite/outbox, retry real em dispositivo e resolução administrativa de conflitos permanecem fora deste repositório.

## Revalidação do GAP-015 — recuperação de senha

### Matriz requisito → símbolo/endpoint → teste → resultado

| Requisito | Símbolo/endpoint | Teste/evidência | Resultado |
|---|---|---|---|
| Anti-enumeração e resposta 202 | `POST /api/v1/auth/forgot-password` | `AuthenticationTest.forgotPasswordHasSameGenericResponseForKnownAndUnknownEmail` | Passou |
| Token seguro, hash e uso único | `PasswordResetToken`, `PasswordResetTokenRepository` | fluxo de reset e consulta por token puro | Passou |
| Expiração e invalidação anterior | `expiresAt`, `invalidateActiveByUserId` | inspeção de migration/serviço + lock pessimista | Passou |
| SES sem acoplamento de domínio | `EmailService`, `AwsSesEmailService`, `MailProperties` | inspeção de configuração e SDK; provider `noop` nos testes | Passou |
| Reset transacional e revogação | `AuthService.resetPassword` | `AuthenticationTest.resetPasswordConsumesTokenAndRevokesRefreshSession` | Passou |
| Senha temporária e primeiro acesso | `mustChangePassword`, `JwtAuthenticationFilter` | `AuthenticationTest.temporaryPasswordIsReportedAndBlocksNormalResourcesUntilChanged` | Passou |
| Regressão completa | API + Flyway/Testcontainers | `mvn clean test` | 30 testes, 0 falhas, 0 erros |

### Veredito independente

`APPROVED`

O GAP-015 foi validado como implementado corretamente. Não houve push. Permanecem apenas evidências operacionais de SES (sandbox/produção, domínio/remetente verificado e IAM da conta AWS), que são configuração de ambiente e não lacunas do backend.

### Evidência operacional SES — AWS Academy

Na tentativa de configuração operacional no AWS Academy, a role `voclabs` apresentou negações de IAM para `ses:GetAccount`, `ses:TagResource` e `ses:CreateEmailIdentity`. Como resultado, o laboratório não permite criar/verificar uma nova identidade remetente. Essa restrição é específica do laboratório e não caracteriza falha na integração desacoplada do backend.

Para desenvolvimento, permanece disponível o provider alternativo de `EmailService` (`noop`) ou uma identidade SES previamente provisionada. Para AWS real, o checklist operacional é: identidade/domínio verificado, remetente configurado externamente, IAM Role com `ses:SendEmail`, região compatível e saída da sandbox quando aplicável.
