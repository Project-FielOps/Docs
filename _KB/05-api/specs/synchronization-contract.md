# Especificação de sincronização

**Status:** contrato MVP aprovado e implementado no backend em 2026-09-06.

## Contrato escolhido

- `POST /api/v1/mobile/sync/push`: recebe `deviceId`, operações ordenadas por dependência e `operationId` único; responde resultado individual `APPLIED`, `ALREADY_APPLIED`, `REJECTED`, `CONFLICT` ou `DEPENDENCY_FAILED`.
- `GET /api/v1/mobile/sync/pull?cursor={cursor}`: retorna alterações autorizadas e `nextCursor`; o cliente só persiste o lote e depois avança o cursor.
- Upload de evidência ocorre separadamente, com metadados idempotentes, estado de arquivo e confirmação antes de considerar evidência obrigatória entregue.

## Payload e limites do MVP

`push` recebe no máximo 50 operações e 1 MB por requisição; cada operação pode ter até 256 KB e no máximo 10 dependências. O `attempt` é opcional, começa em 1 e aceita no máximo 5. As operações suportadas no backend são `ANSWER_UPSERT`, `INSPECTION_START`, `INSPECTION_SUBMIT` e `INSPECTION_REOPEN_CORRECTION`.

```json
{
  "deviceId": "uuid",
  "operations": [{
    "operationId": "uuid",
    "type": "ANSWER_UPSERT",
    "entityId": "inspection-uuid",
    "baseVersion": 3,
    "dependsOn": [],
    "payload": {},
    "attempt": 1
  }]
}
```

`pull` usa cursor numérico opaco ao cliente, retorna no máximo 100 alterações e só avança após o cliente persistir o lote. O cursor inicial é `0`.

As chaves de idempotência são retidas por 90 dias. O cliente usa retry com no máximo 5 tentativas e backoff de 1s, 2s, 4s, 8s e 16s, com jitter de até 25%; erros de autorização e validação não devem ser repetidos automaticamente.

O JWT e o `deviceId` UUID são obrigatórios. O dispositivo é associado ao usuário autenticado pela operação; não há cadastro separado de dispositivo no MVP.

Os códigos de conflito são `VERSION_CONFLICT`, `INVALID_STATE`, `OBJECT_NOT_FOUND`, `FORBIDDEN_SCOPE`, `REQUIRED_FIELD_MISSING`, `REQUIRED_EVIDENCE_MISSING`, `DEPENDENCY_FAILED` e `APPROVED_RESOURCE_IMMUTABLE`.

## Regras

SQLite grava antes da rede. Outbox sobrevive a reinício, mantém falha parcial e não duplica replay. Push respeita dependências; arquivos podem seguir dados estruturados. Conflito preserva valor local e sinaliza intervenção. Inspeção aprovada rejeita atualização tardia comum. Aplicativo não descarta alteração local após cancelamento remoto sem informar o técnico.

## Submissão

O servidor só confirma conclusão/submissão quando respostas obrigatórias, observações exigidas e evidências obrigatórias foram aceitas. Enquanto offline, o cliente exibe “Aguardando sincronização”; isso não altera o estado oficial até confirmação.

## Limitações conhecidas

O app mobile e sua SQLite/outbox ainda não fazem parte deste repositório. A implementação backend mantém conflitos consultáveis por resultado da operação e preserva os dados locais por não descartá-los; a decisão administrativa entre manter servidor ou aceitar envio continua sendo responsabilidade da evolução de resolução de conflitos. Upload privado permanece separado do push.
