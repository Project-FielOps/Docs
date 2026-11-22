# ADR-009 — Integração do MVP entre API, painel e mobile

**Data da proposta:** 2026-09-12  
**Status:** Accepted — aprovado pelo responsável em 2026-09-12.  
**Origem:** decisões D01–D08 fornecidas e aprovadas na conversa. A aprovação define a direção técnica; não declara contrato implementado, testes concluídos nem aceite de produto.

## Contexto

O [plano integrado](../11-planning/mvp-integration-plan.md) identifica lacunas no recebimento de mudanças remotas pelo mobile, na seleção de responsáveis no painel, em QR/GPS, NC offline, upload idempotente, armazenamento local e dados de UI. As decisões abaixo detalham como fechar essas lacunas sem reabrir DEC-002/003/009/010/013 nem ADR-008.

## D01 — Mudanças remotas para o mobile

Ampliar `GET /api/v1/mobile/sync/pull` como feed incremental autorizado após carga inicial em `GET /api/v1/inspections`. Publicar nova atribuição, cancelamento, aprovação, reprovação, reatribuição, mudanças relevantes de estado e perda de escopo com tombstone ou evento equivalente. Exigir cursor paginado, ordem determinística, autorização por usuário e avanço local do cursor apenas depois de persistência bem-sucedida. Atualização remota não apaga edição pendente; conflito é preservado e visível. Listagem periódica pode reconciliar, mas não será o mecanismo principal.

## D02 — Candidatos de atribuição

Criar ou ampliar uma consulta operacional como `GET /api/v1/users/assignable?role=TECHNICIAN|SUPERVISOR`, com escopo autorizado, usuários ativos, paginação, busca textual quando necessária e visibilidade conforme papel. `GET /users` permanece administrativo. A resposta operacional deve expor apenas dados necessários à atribuição. O contrato final de escopo e filtros precisa ser especificado e testado antes do consumo web.

### Clarificação operacional do MVP — 2026-10-19

Para o MVP, o workspace opera com uma única organização e sem vínculo persistido de usuário a cliente, local ou organização. Portanto, o escopo autorizado de `/users/assignable` é global dentro dessa organização única: ADMIN e SUPERVISOR podem consultar somente usuários `ACTIVE` com papel `TECHNICIAN` ou `SUPERVISOR`, conforme o parâmetro `role`. `ADMIN`, usuários inativos e papéis não atribuíveis não são retornados nem aceitos no agendamento. A consulta é paginada por `page`, `size` e `sort`; filtros de status não podem ampliar o conjunto e status diferente de `ACTIVE` retorna vazio. `GET /users` continua exclusivo da gestão administrativa. Um escopo por cliente/local exige decisão e modelo de dados próprios em evolução posterior.

## D03 — QR, GPS e política de localização

Persistir contexto de execução próprio da inspeção, sem depender de metadados de evidência. Inspeção/template expõem `requiresLocation: boolean`; registrar equipamento esperado, método `QR_CODE|MANUAL`, QR lido quando houver, resultado `MATCH|MISMATCH|NOT_CHECKED`, coordenadas e instante da captura ou motivo de ausência. GPS é opcional por padrão; negar permissão deve ser registrado. QR divergente não confirma vínculo e existe alternativa manual. O schema, endpoint/operação de sync e semântica de `requiresLocation=true` ainda devem ser detalhados sem contrariar a continuidade da inspeção em caso de GPS indisponível definida em DEC-009.

## D04 — NC offline

Adicionar `NON_CONFORMITY_UPSERT` à outbox e ao contrato de sync, com `operationId`, `clientEntityId` estável, `inspectionId`, vínculo com item/resposta, severidade, descrição, versão base quando aplicável e dependências explícitas. Ordem lógica: resposta → NC → evidência → submissão. Replay não duplica NC; o servidor devolve o ID persistido; falha parcial preserva pendências. Detalhar payload, mapa `clientEntityId` → ID servidor e resolução de versão antes da implementação.

## D05 — Upload idempotente

Adicionar `Idempotency-Key: <UUID>` ou campo multipart equivalente ao upload, com identidade lógica `userId + inspectionId + idempotencyKey`. Retry da mesma tentativa após timeout retorna o mesmo `evidenceId`, sem novo arquivo/metadado. Checksum valida integridade, mas não substitui a chave, pois anexos distintos podem conter bytes iguais. Definir retenção da chave, comparação de payload e resposta a reutilização com conteúdo divergente no contrato detalhado.

## D06 — Isolamento local por usuário

Guardar tokens e sessão em Secure Store; particionar cache, respostas, outbox e cursor SQLite por `userId`; segregar arquivos locais por `userId`. No logout: solicitar revogação remota, remover tokens ativos, bloquear acesso ao cache da conta e preservar operações pendentes. Outra conta não pode enxergar dados anteriores. Limpeza automática só alcança dados sincronizados conforme retenção; nunca remove pendências, erros, conflitos ou arquivos não confirmados. Definir recuperação local ao entrar novamente na mesma conta e comportamento de logout offline no desenho detalhado.

## D07 — Dados dos cartões mobile

Derivar no app progresso, quantidade de itens, respostas, pendências e NCs a partir do snapshot/cache disponível, sem ampliar DTOs só para copiar mocks. Endereço pode ser buscado e cacheado por `siteId` quando necessário. Ocultar código da inspeção enquanto não existir contrato formal. Projeções de resumo na API ficam para necessidade comprovada de desempenho, latência ou UX.

## D08 — Rotas e filtros globais do painel

No MVP, usar navegação cliente → local → equipamento e inspeção → NC, com apenas filtros suportados pela API. Não criar agora listagens globais de locais, equipamentos ou NC nem filtros avançados sem necessidade comprovada; tratá-los como P1. Controle sem backend real não deve simular sucesso.

## Consequências e aceite

Após aprovação formal, especificar schemas, erros, autorização, migrações e testes de contrato para D01–D05; implementar isolamento e UI para D06–D08. As dependências detalhadas são `D01 → API-01/MOB-05`, `D02 → API-03/WEB-01`, `D03 → API-04/MOB-06`, `D04 → API-05/MOB-04`, `D05 → API-06/MOB-04`, `D06 → MOB-01/02`, `D07 → MOB-03`, `D08 → WEB-03`. A [matriz de aceite](../11-planning/mvp-integration-plan.md) exige jornada reproduzível entre as três aplicações.

## Decisões anteriores preservadas

Somente técnico atribuído executa; somente supervisor atribuído revisa; reprovação exige reabertura explícita; GPS é opcional por padrão e sua ausência é registrada; técnico não remove evidência sincronizada; `push/pull` são os endpoints oficiais; estado oficial e estado local de sync são distintos; conflito não é resolvido automaticamente no MVP.
