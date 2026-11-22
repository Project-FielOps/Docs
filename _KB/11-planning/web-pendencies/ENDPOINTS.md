# Catálogo de endpoints da interface web

## Convenções

- Base: `${environment.apiUrl}`, apontando para `/api/v1`.
- Endpoints abaixo são relativos a `/api/v1`.
- Autenticação: `Authorization: Bearer <accessToken>`, exceto login e recuperação.
- Paginação Spring: `page` base zero, `size` e `sort=campo,direção`.
- Erro canônico: `ApiError`, incluindo `status`, `code`, `message`, `path`, `timestamp`, `requestId` e `fieldErrors` quando aplicável.
- `AVAILABLE`: controller encontrado na API atual.
- `FRONTEND_GAP`: endpoint existe, mas o consumo/fluxo web está ausente ou incompleto.
- `API_GAP`: comportamento requerido pela spec web não possui endpoint atual; exige decisão/implementação backend ou ajuste explícito de UX.

Este catálogo descreve consumo, não substitui OpenAPI nem os DTOs do backend.

## Autenticação

| Método | Endpoint | Uso web | Status |
| --- | --- | --- | --- |
| POST | `/auth/login` | autenticar com `email`, `password`; recebe tokens e `mustChangePassword` | `AVAILABLE`; `FRONTEND_GAP` no campo de troca obrigatória |
| GET | `/auth/me` | carregar usuário autenticado após login/refresh | `AVAILABLE` |
| POST | `/auth/refresh` | rotacionar sessão com `refreshToken` | `AVAILABLE` |
| POST | `/auth/logout` | revogar sessão pelo `refreshToken` | `FRONTEND_GAP` |
| POST | `/auth/forgot-password` | solicitar recuperação com resposta anti-enumeração | `FRONTEND_GAP` |
| POST | `/auth/reset-password` | redefinir usando `token` e nova senha | `FRONTEND_GAP` |
| POST | `/auth/change-password` | trocar senha autenticada/temporária | `FRONTEND_GAP` |

## Dashboard

| Método | Endpoint | Uso web | Status |
| --- | --- | --- | --- |
| GET | `/dashboard/summary` | contagens por status, atrasadas, aguardando revisão e NC por severidade | `AVAILABLE`; validar escopo |

## Usuários

| Método | Endpoint | Parâmetros/corpo relevante | Uso | Status |
| --- | --- | --- | --- | --- |
| GET | `/users` | `role`, `status`, `page`, `size`, `sort` | lista administrativa | `AVAILABLE` |
| POST | `/users` | `CreateUserRequest` | criar usuário | `AVAILABLE` |
| GET | `/users/{id}` | UUID | detalhe/edição | `AVAILABLE` |
| PUT | `/users/{id}` | `UpdateUserRequest` | editar | `AVAILABLE` |
| PATCH | `/users/{id}/status` | `{status}` | ativar/inativar/bloquear conforme enum | `AVAILABLE` |
| GET | `/users/assignable` | `role=TECHNICIAN\|SUPERVISOR`, `status`, `page`, `size`, `sort` | usuários ativos atribuíveis no escopo global único do MVP; somente ADMIN/SUPERVISOR | `AVAILABLE` |
| GET | `/users?search=...` | busca textual | nome/e-mail na listagem | `API_GAP` |

## Clientes

| Método | Endpoint | Parâmetros/corpo relevante | Status |
| --- | --- | --- | --- |
| GET | `/clients` | `status`, `name`, `page`, `size`, `sort` | `AVAILABLE` |
| POST | `/clients` | `ClientUpsertRequest` | `AVAILABLE` |
| GET | `/clients/{id}` | UUID | `AVAILABLE` |
| PUT | `/clients/{id}` | `ClientUpsertRequest` | `AVAILABLE` |
| PATCH | `/clients/{id}/status` | `{status}` | `AVAILABLE` |

## Locais

| Método | Endpoint | Parâmetros/corpo relevante | Status |
| --- | --- | --- | --- |
| GET | `/clients/{clientId}/sites` | `status`, `page`, `size`, `sort` | `AVAILABLE` |
| POST | `/clients/{clientId}/sites` | `SiteUpsertRequest` | `AVAILABLE` |
| GET | `/sites/{id}` | UUID | `AVAILABLE` |
| PUT | `/sites/{id}` | `SiteUpsertRequest` | `AVAILABLE` |
| PATCH | `/sites/{id}/status` | `{status}` | `AVAILABLE` |
| GET | `/sites` | lista global/filtro por cliente | `API_GAP`; rota web global deve selecionar cliente ou aguardar contrato |
| POST | `/sites` | criação sem cliente no path | não usar; `API_GAP` por desenho incompatível |

## Equipamentos

| Método | Endpoint | Parâmetros/corpo relevante | Status |
| --- | --- | --- | --- |
| GET | `/sites/{siteId}/equipment` | `status`, `page`, `size`, `sort` | `AVAILABLE` |
| POST | `/sites/{siteId}/equipment` | `EquipmentUpsertRequest` | `AVAILABLE` |
| GET | `/equipment/{id}` | UUID | `AVAILABLE` |
| GET | `/equipment/by-qr/{qrCode}` | QR URL-encoded | `AVAILABLE` |
| PUT | `/equipment/{id}` | `EquipmentUpsertRequest` | `AVAILABLE` |
| PATCH | `/equipment/{id}/status` | `{status}` | `AVAILABLE` |
| GET | `/equipment` | lista global/filtros | `API_GAP`; rota global deve selecionar local ou aguardar contrato |
| POST | `/equipment` | criação sem local no path | não usar; `API_GAP` por desenho incompatível |

## Modelos de inspeção

| Método | Endpoint | Parâmetros/corpo relevante | Status |
| --- | --- | --- | --- |
| GET | `/inspection-templates` | `status`, `category`, `page`, `size`, `sort` | `AVAILABLE` |
| POST | `/inspection-templates` | `CreateTemplateRequest` | `AVAILABLE` |
| GET | `/inspection-templates/{id}` | UUID | `AVAILABLE` |
| PUT | `/inspection-templates/{id}` | `UpdateTemplateRequest` | `AVAILABLE` |
| PATCH | `/inspection-templates/{id}/status` | `{status}` | `AVAILABLE` |
| GET | `/inspection-templates/{id}/draft` | versão editável com seções/itens | `AVAILABLE` |
| POST | `/inspection-templates/{id}/sections` | `SectionUpsertRequest` | `AVAILABLE` |
| PUT | `/inspection-templates/{id}/sections/{sectionId}` | `SectionUpsertRequest` | `AVAILABLE` |
| DELETE | `/inspection-templates/{id}/sections/{sectionId}` | — | `AVAILABLE` |
| POST | `/inspection-templates/{id}/sections/{sectionId}/items` | `ItemUpsertRequest` | `AVAILABLE` |
| PUT | `/inspection-templates/{id}/sections/{sectionId}/items/{itemId}` | `ItemUpsertRequest` | `AVAILABLE` |
| DELETE | `/inspection-templates/{id}/sections/{sectionId}/items/{itemId}` | — | `AVAILABLE` |
| POST | `/inspection-templates/{id}/publish` | sem corpo significativo | `AVAILABLE` |
| GET | `/inspection-templates/{id}/versions` | lista não paginada | `AVAILABLE` |
| GET | `/inspection-templates/{id}/versions/{versionNumber}` | número inteiro, não UUID | `AVAILABLE` |

## Inspeções

| Método | Endpoint | Parâmetros/corpo relevante | Status |
| --- | --- | --- | --- |
| GET | `/inspections` | `technicianId`, `supervisorId`, `clientId`, `siteId`, `status`, `priority`, `scheduledFrom`, `scheduledTo`, paginação | `AVAILABLE` |
| POST | `/inspections` | `CreateInspectionRequest` | `AVAILABLE` |
| GET | `/inspections/{id}` | detalhe e snapshot | `AVAILABLE` |
| POST | `/inspections/{id}/assign` | `{technicianId, supervisorId}` | `AVAILABLE` |
| POST | `/inspections/{id}/cancel` | `{reason}` | `AVAILABLE` |
| POST | `/inspections/{id}/start` | execução do técnico; não expor como ação administrativa | `AVAILABLE`, fora da ação web normal |
| POST | `/inspections/{id}/submit` | execução do técnico; não expor como ação administrativa | `AVAILABLE`, fora da ação web normal |
| POST | `/inspections/{id}/reopen-correction` | técnico atribuído; não expor como ação administrativa | `AVAILABLE`, fora da ação web normal |
| GET | `/inspections/{id}/history` | histórico/auditoria autorizada | `FRONTEND_GAP` |
| GET | `/inspections?equipmentId=...` | filtro previsto | `API_GAP` |
| GET | `/inspections?overdue=...` | filtro previsto | `API_GAP` |
| GET | `/inspections?awaitingReview=...` | atalho/filtro previsto | `API_GAP`; pode mapear para status se semanticamente equivalente |

Datas `scheduledFrom` e `scheduledTo` são `Instant` em ISO-8601 com timezone. O frontend não deve enviar data local ambígua.

## Respostas

| Método | Endpoint | Uso web | Status |
| --- | --- | --- | --- |
| GET | `/inspections/{inspectionId}/responses` | detalhe e revisão | `FRONTEND_GAP` |
| PUT | `/inspections/{inspectionId}/responses/{inspectionItemId}` | execução do técnico | disponível, não usar para edição administrativa |
| PUT | `/inspections/{inspectionId}/responses` | batch do técnico/sync | disponível, não usar para edição administrativa |

## Evidências

| Método | Endpoint | Uso web | Status |
| --- | --- | --- | --- |
| GET | `/inspections/{inspectionId}/evidence` | galeria/metadados | `AVAILABLE` |
| GET | `/inspections/{inspectionId}/evidence/{evidenceId}` | metadado individual | `FRONTEND_GAP` |
| GET | `/inspections/{inspectionId}/evidence/{evidenceId}/content` | visualizar/baixar conteúdo autorizado | `FRONTEND_GAP` |
| POST | `/inspections/{inspectionId}/evidence` | captura/upload; web não envia evidência no MVP | disponível, fora do escopo web MVP |
| DELETE | `/inspections/{inspectionId}/evidence/{evidenceId}` | remoção administrativa auditada conforme regra | `FRONTEND_GAP`; confirmar UX/permissão antes de expor |

## Revisão

| Método | Endpoint | Corpo | Status |
| --- | --- | --- | --- |
| POST | `/inspections/{inspectionId}/begin-review` | sem corpo significativo | `AVAILABLE` |
| POST | `/inspections/{inspectionId}/approve` | `{comments?}` | `AVAILABLE` |
| POST | `/inspections/{inspectionId}/reject` | `{reason, comments?}` | `AVAILABLE` |
| GET | `/inspections/{inspectionId}/reviews` | — | `AVAILABLE` |

Somente o supervisor atribuído decide. Ocultar botão no frontend não substitui a resposta 403 do backend.

## Não conformidades

| Método | Endpoint | Uso web | Status |
| --- | --- | --- | --- |
| GET | `/inspections/{inspectionId}/non-conformities` | detalhe/revisão | `AVAILABLE` |
| GET | `/inspections/{inspectionId}/non-conformities/{id}` | detalhe | `AVAILABLE` |
| PATCH | `/inspections/{inspectionId}/non-conformities/{id}/status` | alteração autorizada de status | `AVAILABLE` |
| POST | `/inspections/{inspectionId}/non-conformities` | execução do técnico | disponível, fora da criação administrativa normal |
| PUT | `/inspections/{inspectionId}/non-conformities/{id}` | execução/correção autorizada | disponível; não presumir permissão administrativa |
| GET | `/non-conformities` | lista global com filtros | `API_GAP`; bloqueia a rota global prevista |

## Auditoria

| Método | Endpoint | Parâmetros | Status |
| --- | --- | --- | --- |
| GET | `/audit-events` | `entityType`, `entityId`, `actorId`, `page`, `size`, `sort` | `AVAILABLE` |
| GET | `/audit-events?from&to&action&inspectionId&reason` | filtros previstos | `API_GAP` |

## Matriz tela → endpoints principais

| Spec | Endpoints principais |
| --- | --- |
| WEB-01 | `/auth/*` |
| WEB-02 | `/auth/me`, `/auth/refresh`, `/auth/logout` |
| WEB-03 | `/dashboard/summary` |
| WEB-04 | `/users`, `/users/{id}`, `/users/assignable` |
| WEB-05 | `/clients`, `/clients/{id}`, `/clients/{clientId}/sites` |
| WEB-06 | `/clients/{clientId}/sites`, `/sites/{id}` |
| WEB-07 | `/sites/{siteId}/equipment`, `/equipment/{id}`, `/equipment/by-qr/{qrCode}` |
| WEB-08..11 | `/inspection-templates/**` |
| WEB-12..14 | `/inspections/**`, respostas, evidências, NC e histórico |
| WEB-15 | `/inspections/{id}/begin-review|approve|reject|reviews` e dados relacionados |
| WEB-16 | `/inspections/{id}/non-conformities/**`; endpoint global pendente |
| WEB-17 | `/audit-events`; filtros adicionais pendentes |
