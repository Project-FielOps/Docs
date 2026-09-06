# Components

Este documento mapeia os componentes do sistema para acelerar o entendimento de mudanças.

## Backend

- `auth`: autenticação, tokens e gestão de sessão.
- `user`: CRUD de usuários e papéis.
- `client`: cadastros de cliente e permissões.
- `site`: locais e vinculação com cliente.
- `equipment`: equipamentos e QR.
- `template`: modelos, versões, seções e itens.
- `inspection`: agendamento, snapshot, respostas e fluxo de inspeção.
- `evidence`: upload, metadados e vinculação com inspeção/response.
- `review`: revisão e decisão.
- `audit`: eventos de auditoria e trilha histórica.
- `dashboard`: agregações e relatórios (parcial).

## Frontend

- Painel Angular com módulos correspondentes aos domínios.

## Mobile (proposto)

- armazenamento local (SQLite) e outbox para operações pendentes.
- sincronização push/pull com a API.

## Observability

- logs estruturados;
- métricas e tracing (TO-BE).

## Referências

- Código em `API/src/main/java/com/fieldops`.
- Frontend em `interface-administrativa/src/app`.
