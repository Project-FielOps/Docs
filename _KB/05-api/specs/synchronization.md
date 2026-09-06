# Synchronization — Draft Contract

## Overview

Rascunho inicial do contrato de sincronização entre mobile e API, baseado em DEC-013 e na decisão que mobile faz parte do MVP.

## Endpoints (implementação atual e recomendação)

Este documento junta o contrato proposto com os endpoints atualmente implementados no backend e recomenda a escolha prática para o MVP.

Opções:

- Contrato dedicado (proposto): `POST /api/v1/mobile/sync/push` e `GET /api/v1/mobile/sync/pull` — útil para clientes que queiram um único contrato unificado de sync.
- Contrato pragmático (implementação atual): utilizar os endpoints por recurso já disponíveis no backend. Recomenda-se documentar e padronizar este conjunto como contrato canônico se optarmos por não implementar endpoints dedicados.

Endpoints pragmáticos recomendados (usar como contrato canônico se não houver endpoint `/mobile/sync`):

- `PUT /api/v1/inspections/{inspectionId}/responses` — upsert de uma resposta individual (já implementado).
- `PUT /api/v1/inspections/{inspectionId}/responses` (batch) — upsert de múltiplas respostas em lote (usar `BatchAnswerRequest`).
- `POST /api/v1/inspections/{inspectionId}/evidence` — upload de evidência (retorna `evidenceId`).
- `GET /api/v1/inspections` e `GET /api/v1/inspections/{id}` — listar e obter inspeções atribuídas.

Observação: estes endpoints são idempotentes por design na maioria dos casos (batch upsert) e já obedecem regras de autorização e vinculação por inspeção/usuário.

## Semântica (prática)

- Idempotência: quando possível use `operationId`/identificadores únicos em payloads batch para permitir deduplicação no servidor.
- Evidências: enviar via `POST /api/v1/inspections/{inspectionId}/evidence` e referenciar `evidenceId` nas respostas/upserts subsequentes.
- Resposta por operação: em endpoints batch retornar array de resultados por item com `operationId`, `status` e `reason` quando aplicável.
- Conflitos: utilizar `409` quando existir conflito de versão; a resposta deverá conter contexto suficiente para que o cliente decida retry ou intervenção manual.
- Cursor (se implementado): avançar apenas após confirmação local do cliente em cenários que precisem de pull push controlado.

## Respostas

- success per operation: `{ operationId, status: "OK", result?: {...} }`
- partial failure: HTTP 207 or 200 with array of results per operation
- error: standard `ApiError` format

## Security

- autenticação Bearer JWT;
- autorização por objeto aplicada a operações que alterem recursos;
- uploads de evidência autorizados separadamente e com checagem de vinculação ao usuário e inspeção.

## Next steps

- Publicar este documento como contrato canônico se optarmos por usar os endpoints existentes (pragmático).
- Formalizar os schemas JSON dos endpoints listados acima e incluir exemplos de request/response.
- Implementar testes de contrato (mock server) para mobile usando os endpoints pragmáticos ou criar endpoints `/mobile/sync` se preferirem o contrato unificado.

