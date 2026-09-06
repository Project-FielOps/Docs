# Especificação de sincronização

**Status:** direção aprovada; payload detalhado ainda precisa de refinamento.

## Contrato escolhido

- `POST /api/v1/mobile/sync/push`: recebe `deviceId`, operações ordenadas por dependência e `operationId` único; responde resultado individual `APPLIED`, `ALREADY_APPLIED`, `REJECTED`, `CONFLICT` ou `DEPENDENCY_FAILED`.
- `GET /api/v1/mobile/sync/pull?cursor={cursor}`: retorna alterações autorizadas e `nextCursor`; o cliente só persiste o lote e depois avança o cursor.
- Upload de evidência ocorre separadamente, com metadados idempotentes, estado de arquivo e confirmação antes de considerar evidência obrigatória entregue.

## Regras

SQLite grava antes da rede. Outbox sobrevive a reinício, mantém falha parcial e não duplica replay. Push respeita dependências; arquivos podem seguir dados estruturados. Conflito preserva valor local e sinaliza intervenção. Inspeção aprovada rejeita atualização tardia comum. Aplicativo não descarta alteração local após cancelamento remoto sem informar o técnico.

## Submissão

O servidor só confirma conclusão/submissão quando respostas obrigatórias, observações exigidas e evidências obrigatórias foram aceitas. Enquanto offline, o cliente exibe “Aguardando sincronização”; isso não altera o estado oficial até confirmação.

## Pendências do contrato

Definir esquema JSON final, paginação/lote, retenção da chave idempotente, backoff e limite de tentativas, códigos de conflito, autenticação de dispositivo e estratégia de upload privado. DEC-007 já exige conflito consultável, dados locais preservados e decisão administrativa auditada entre manter o servidor ou aceitar o envio.
