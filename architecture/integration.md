# Integração e sincronização

## Contratos entre aplicações

Web e mobile consomem a API por HTTPS. A API é a única responsável por autenticação, autorização, validação, transição de estado e auditoria. O painel usa JSON; o mobile usa JSON e multipart para evidências. Contratos publicados devem ser representados no OpenAPI e não duplicados em documentação informal.

## Fluxo vertical prioritário

```text
Supervisor publica modelo → agenda inspeção → técnico baixa e executa →
sincroniza → supervisor revisa → aprova ou reprova → auditoria registra
```

Esse fluxo é a referência para IVH-001, IVH-005, IVH-006 e IVH-007. Dados de demonstração devem incluir usuário, cliente, local, equipamento, modelo publicado e inspeção atribuída.

## Sincronização

DEC-013 aprovou operações separadas: `POST /api/v1/mobile/sync/push` e `GET /api/v1/mobile/sync/pull?cursor=...`, com resultados por operação, `operationId` idempotente, cursor somente após persistência e upload de evidência separado. O payload e parâmetros operacionais continuam em [specification de sync](../specs/synchronization.md). Esses endpoints ainda não existem no código observado.

Em qualquer alternativa, a API deve reconhecer a chave idempotente, devolver resultado por operação e manter a inspeção aprovada protegida. Falhas parciais preservam a outbox; reenvio não pode duplicar resposta, evidência ou transição.

## Verificação

Validar web/API, mobile/API e offline/sync separadamente. Cobrir token expirado, falta de permissão, perda de conexão durante foto, repetição de operação, cancelamento remoto e conflito com decisão de revisão.
