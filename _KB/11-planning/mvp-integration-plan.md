# Plano integrado de pendências do MVP — API, painel e mobile

**Levantamento estático inicial:** 2026-09-12. **Rechecagem de rotas e testes:** 2026-10-20. **Estado:** planejamento, sem aceite de integração. Este plano cruza `API/`, `interface-administrativa/` e `mobile/`; não substitui as [decisões aprovadas](../12-decisions/architecture-decisions.md), o [backlog web](web-pendencies/BACKLOG.md), o [backlog backend](backend-pendencies/BACKLOG.md) nem o [mapa mobile](mobile-pendencies.md). P0 é necessário ao fluxo MVP; P1 é acabamento ou ampliação que não deve bloquear o percurso principal, salvo decisão explícita. `EXISTE` indica código observado, `PARCIAL` não indica aceite, `API_GAP` exige contrato/implementação. A rechecagem de 2026-10-20 foi estática e não executou testes nem altera critérios de aceite.

## Percurso que deve funcionar

ADMIN cadastra usuários, clientes, locais e equipamentos; ADMIN/SUPERVISOR autorizado publica modelo; SUPERVISOR agenda inspeção com técnico e supervisor válidos no painel; técnico atribuído recebe a inspeção no mobile, executa checklist dinâmico e registra NC/foto com ou sem rede; o app sincroniza dados e arquivos sem perda/duplicação; supervisor atribuído consulta respostas/evidências e aprova ou reprova; a decisão chega ao mobile; reprovação permite reabertura explícita e nova submissão. Dashboard, histórico e auditoria mostram o estado real. A API valida papel e vínculo em todas as etapas.

## Pendências confirmadas pelo código

| ID | Área | Evidência observada | Impacto |
|---|---|---|---|
| G01 | API → mobile | Na rechecagem estática de 2026-10-20 não foram encontrados controllers/rotas `/api/v1/mobile/sync/push` ou `/pull`, nem `SyncService` no snapshot da API revisado. | O contrato aprovado em DEC-013 não está demonstrado como implementado. Push, pull, cursor, idempotência, atribuições/cancelamentos/decisões remotas e recuperação de falha continuam P0 `API_GAP`; não presumir suporte por `GET /inspections`. |
| G02 | API/painel | `InspectionService.create/assign` aceita usuário `ADMIN` como `supervisorId`; DEC-002 exige supervisor atribuído para revisar | Uma inspeção pode ficar atribuída a alguém que não pode revisá-la. Corrigir validação e teste. P0. |
| G03 | Painel/API | `inspection-new.component.ts` e `inspections-list.component.ts` chamam `UsersService.list()` (`GET /users`, ADMIN-only). `/users/assignable` devolve apenas técnicos ativos | SUPERVISOR não carrega técnicos na lista/formulário e não tem fonte autorizada de supervisores selecionáveis. P0 contrato + frontend. |
| G04 | API/mobile | Inspeção não expõe `requiresLocation`, confirmação QR nem registro de GPS ausente; há somente equipamento no detalhe, busca `/equipment/by-qr` e coordenadas opcionais de evidência | Requisito nativo não tem trilha completa. P0 para o MVP definido, com desenho de contrato pendente. |
| G05 | API/mobile | Não foi encontrado controller para `/api/v1/mobile/sync/push`. O app drena operações por chamadas REST individuais; NC usa `POST /api/v1/inspections/{id}/non-conformities`, fora do contrato idempotente de sync. | NC criada offline não tem envio idempotente/replay definido; foto vinculada depende do ID da NC/resposta. Implementar e testar o fluxo aprovado antes de declarar sync funcional. P0. |
| G06 | API/mobile | Upload multipart não recebe chave de idempotência; resposta de evidência tem checksum, mas não URL; `/content` é autenticado | Timeout após persistência pode criar duplicata no retry e atingir limite de 10. Contrato de upload/reconciliação e teste são P0. |
| G07 | API | `EvidenceService.delete` permite ao técnico excluir evidência em inspeção não aprovada, inclusive se já sincronizada | Contraria DEC-010, que proíbe remoção de evidência sincronizada pelo técnico. Corrigir sem remover arquivo pendente local automaticamente. P0. |
| G08 | Mobile | Store e repository usam mocks/memória; checklist e observação não persistem; foto e sync são simulados; `syncNow` promove qualquer inspeção pendente a `SUBMITTED` | Fluxo offline e estados oficiais não são confiáveis. P0. |
| G09 | Mobile visual | `Button` secundário mantém texto branco sobre verde claro; filtro de inspeções é só texto; estados de erro/vazio/sync são incompletos; checklist só mostra conformidade | Baixa legibilidade, ações enganosas e impossibilidade de executar modelos com outros `responseType`. P0 no fluxo; motion/polimento P1. |
| G10 | Painel | Login/refresh existem; logout só limpa tokens locais; `mustChangePassword` não está no tipo de login; planejamento chama rota ADMIN-only; detalhe não busca evidências; `EvidenceService` não baixa `/content` | Sessão e revisão visual não cobrem todos os dados/ações reais. P0. |
| G11 | Painel | `inspection-detail.canCancel()` proíbe `REJECTED` e permite `SUBMITTED`/`UNDER_REVIEW`, divergindo do fluxo `ASSIGNED|IN_PROGRESS|REJECTED → CANCELED`; filtros avançados estão na UI sem contrato equivalente | Ações exibidas incorretamente e risco de 409. Ajustar UI ao estado autorizado, sem abrir exceção no backend. P0 para cancelamento; filtros extras P1. |
| G12 | Documentação | DEC-013 aprova push/pull, mas as rotas correspondentes não foram encontradas na rechecagem estática da API em 2026-10-20; documentação histórica também precisa ser comparada ao contrato aprovado. | Diferenciar decisão aprovada de implementação ausente e alinhar docs/cliente à especificação vigente. Não declarar sincronização implementada até localizar endpoints e testes executáveis. P0. |

O painel já contém rotas, serviços e componentes para cadastros, modelos, planejamento, revisão, dashboard e auditoria. Isso é implementação parcial. O [backlog web](web-pendencies/BACKLOG.md) continua fonte dos detalhes por tela. O mapa mobile descreve endpoints e DTOs esperados por fluxo; sua presença documental não comprova implementação na API. As lacunas globais de listas/filtros da web são P1 se o MVP usar navegação contextual cliente → local → equipamento e filtros suportados; não são condição para iniciar a jornada principal.

## Decisões técnicas a registrar antes das tasks dependentes

As linhas abaixo foram aprovadas no [ADR-009](../12-decisions/ADR-009-mvp-cross-app-integration.md) em 2026-09-12. As recomendações operacionais continuam em [decisões de entrega propostas](../../decisions/delivery-decisions-proposal.md). DEC-002/003/009/010/013 e ADR-008 já estavam fechadas; não reabrir autorização, reabertura, GPS opcional, remoção de evidência sincronizada nem push/pull como escolhas livres.

| ID | Pergunta em aberto | Recomendação para o MVP | Alternativa/efeito | Desbloqueia |
|---|---|---|---|---|
| D01 | Como o técnico recebe atribuições, cancelamentos e decisões web? | Ampliar feed `pull` para alterações autorizadas de inspeção/snapshot e revogação de escopo, com cursor e tombstone; reconciliar lista inicial por `GET /inspections` | Polling paginado periódico por lista/detalhe pode ser ponte, mas precisa regra de remoção e não cumpre sozinho o feed aprovado | G01, API-01, MOB-05 |
| D02 | Como o painel obtém técnicos e supervisores para atribuição? | Expandir consulta de atribuíveis com papel e escopo autorizados; supervisor vê técnicos ativos e supervisores permitidos, sem abrir `GET /users` administrativo | Fixar supervisor atual em toda inspeção reduziria escolha; confirmar com produto antes de adotar | G03, API-03, WEB-01 |
| D03 | Qual é o registro persistido de QR e GPS? | Definir campos/eventos de execução ligados à inspeção: equipamento esperado, código lido ou entrada manual, resultado de correspondência, coordenada pontual ou motivo de ausência; `requiresLocation` no detalhe/snapshot | Guardar só em metadados de foto deixa inspeções sem foto sem prova | G04, API-04, MOB-06 |
| D04 | Como a NC offline entra no sync? | Adicionar operação idempotente de criação/atualização de NC com ID de cliente ou correlação; resposta/evidência dependem do ID confirmado | Enviar `POST` avulso após reconexão exige idempotência própria e fila separada | G05, API-05, MOB-04 |
| D05 | Como reconciliar upload após timeout? | Aceitar chave de upload idempotente por usuário/inspeção, retornar sempre o mesmo `evidenceId` e permitir consulta por chave; manter hash/tamanho como verificação | Busca por checksum na listagem é heurística insuficiente para arquivos iguais legitimamente anexados duas vezes | G06, API-06, MOB-04 |
| D06 | Como preservar dados offline ao sair/trocar de conta? | Tokens em Secure Store; cache e outbox isolados por usuário; logout revoga token e bloqueia acesso ao cache da conta, sem apagar pendências; limpeza de dados só em ação explícita ou política após sincronização | Apagar cache no logout perderia trabalho pendente; manter cache visível a outro usuário vazaria dados | MOB-01/02 |
| D07 | Como representar dados do card que a API não retorna (`code`, endereço, contagens e progresso)? | Derivar progresso/contagens do snapshot e respostas, buscar local pelo `siteId` se endereço for essencial e ocultar código até contrato formal | Acrescentar campos ao resumo se houver necessidade validada de desempenho/UX | MOB-03, API opcional |
| D08 | Como tratar rotas globais de locais/equipamentos/NC e filtros sem API? | No MVP, exigir contexto cliente/local/inspeção e remover controles que não têm resultado real; manter ampliação de endpoints para P1 | Criar endpoints globais paginados aumenta escopo e autorização | WEB-03, API-GAP-001..005 |

Detalhar schemas, erros, migrações e testes nos contratos das tasks antes de implementar cada endpoint. A aprovação não significa que um endpoint novo já existe.

## Backlog integrado do MVP

Estados após aprovação: `READY` = direção aprovada, pode iniciar especificação/implementação; `PARTIAL` = há código, sem aceite; `BLOCKED` = depende de outra task. Nenhum item está `DONE` sem teste e evidência da jornada.

| Ordem | Task | Prioridade/estado | Dependências | Critério de aceite objetivo |
|---|---|---|---|---|
| 1 | API-00 · Corrigir `supervisorId` para `SUPERVISOR` ativo e proibir exclusão de evidência sincronizada pelo técnico | P0 READY | DEC-002/010 | Testes rejeitam ADMIN como revisor e delete após confirmação; mantêm leitura/auditoria. |
| 2 | DOC-00 · Unificar contrato de sync, exemplos, erros, estados e fotografias documentais datadas | P0 READY | DEC-013 | Rascunho antigo deixa de indicar endpoints por recurso como canônicos; API, web e mobile apontam ao mesmo contrato. |
| 3 | API-01 · Feed de mudanças remotas de atribuição/status/revisão e revogação de escopo | P0 READY | D01 aprovada | Técnico recebe nova atribuição e decisão em pull; cursor paginado e autorização por usuário; cancelamento/reassignment não apagam outbox. |
| 4 | API-03 · Consulta autorizada de atribuíveis por papel para planejamento | P0 READY | D02 aprovada | SUPERVISOR carrega técnicos/supervisores permitidos sem acesso à gestão de usuários; teste de escopo. |
| 5 | API-04 · Contrato e persistência de QR/GPS/ausência e `requiresLocation` | P0 READY | D03 aprovada | Divergência QR não confirma vínculo; GPS negado registra ausência e não bloqueia quando opcional; detalhe permite ao app aplicar a política. |
| 6 | API-05 · NC offline idempotente | P0 READY | D04 aprovada | Replay não duplica NC; NC crítica/evidência são validadas antes da submissão; vínculos/autorização testados. |
| 7 | API-06 · Idempotência e recuperação de upload multipart | P0 READY | D05 aprovada | Mesmo upload após timeout retorna mesmo ID; arquivo/metadado consistentes; limites e autorização preservados. |
| 8 | WEB-01 · Corrigir planejamento e filtros de técnico: usar atribuíveis, supervisor permitido e seleção paginada | P0 BLOCKED | API-03; WEB-T011/024 | Supervisor agenda inspeção completa sem 403; ADMIN segue escopo; não depende de lista limitada aos primeiros 200. |
| 9 | WEB-02 · Completar sessão e revisão: troca obrigatória, logout remoto, `/content`, estados, cancelamento e decisões por responsável | P0 PARTIAL | WEB-T004..008/025..030; API-00 | Jornada web login → agendamento → revisão com foto e motivo passa; 401/403/409 são distinguíveis. |
| 10 | WEB-03 · Fechar cadastros, modelo, dashboard e auditoria do caminho principal | P0 PARTIAL | WEB-T009/010/012/013/015/017..022/032; D08 | ADMIN prepara dados; supervisor publica/agende conforme papel; dashboard usa dados reais; rotas sem endpoint não simulam sucesso. |
| 11 | MOB-01 · Cliente HTTP e sessão real com Secure Store, rota protegida e usuário real | P0 READY | D06 | Login, `/me`, refresh, primeiro acesso, logout e recuperação funcionam; técnico não vê dados de outra conta. |
| 12 | MOB-02 · SQLite versionado, cache, outbox, cursor e arquivo local | P0 BLOCKED | MOB-01; D06 | Edição e anexos sobrevivem a reinício offline; fila/cursor são transacionais e isolados por conta. |
| 13 | MOB-03 · UI de inspeções/checklist dinâmico e visual operacional | P0 BLOCKED | MOB-02; D07 | Lista/detalhe reais; todos os `responseType`; texto legível em botões; filtros funcionam; loading/empty/error/offline/pending/conflict acessíveis. |
| 14 | MOB-04 · NC, foto, observação por item, upload e submissão dependente | P0 BLOCKED | API-05/06, MOB-03 | Foto/NC sobrevivem offline; resposta → NC/evidência → submit; validações locais e do servidor batem; nada é marcado `SUBMITTED` antes do servidor. |
| 15 | MOB-05 · Push/pull, retry e conflitos com estado oficial separado | P0 BLOCKED | API-01, MOB-02/04 | Dois dispositivos, falha parcial, replay, reprovação remota, cancelamento e cursor após commit passam sem perda/duplicidade. |
| 16 | MOB-06 · QR, entrada manual e GPS pontual | P0 BLOCKED | API-04, MOB-03 | QR divergente exige identificação manual; permissão negada é registrada; ação fica utilizável em campo. |
| 17 | QA-01 · Contratos e jornada local das três aplicações | P0 BLOCKED | API-00..06, WEB-01..03, MOB-01..06 | Testes de autorização e contrato; execução offline/reinício/upload/revisão; evidência de dispositivo e navegador; `mvn test`, `npm test`, builds, typecheck e verificação de formatação. |
| 18 | QA-02 · Acessibilidade, segurança e operação publicada | P0 BLOCKED | QA-01; infraestrutura AWS | WCAG 2.1 AA nas telas críticas, smoke HTTPS, S3 privado, SES quando aplicável, logs/correlation ID, backup/rollback e avaliação de latência documentados. |

`WEB-T*` são tarefas já detalhadas no backlog web; `BE-*` de AWS/CI continuam no backlog backend. Este plano não duplica esses cards. A ordem permite implementar contratos e fundação em paralelo onde não há dependência, mas o aceite final exige a jornada integrada. BE-015 foi reclassificada de P2 para P0 no [backlog backend](backend-pendencies/BACKLOG.md), preservando o histórico da classificação anterior.

## Fora do caminho crítico

Listagens globais de local/equipamento/NC, filtros avançados, push notification remoto, analytics avançado e polimento de animação são P1/P2 conforme escopo vigente. Não substituem QR, GPS opcional registrado, NC, evidência, offline, sync, revisão nem AWS, que permanecem no MVP.

## Sequência de execução após aprovação de D01–D08

ADR-009 registrado → API-00 → DOC-00 → API-01/03/04/05/06 → congelar contratos MVP → MOB-01/02/03/04/05/06 → WEB-01/02/03 → QA integrado → homologação AWS. Essa é a ordem de integração e aceite; preparação de tarefas independentes pode ocorrer antes. AWS-01/02, OPS-01 e SES-01 da [proposta operacional](../../decisions/delivery-decisions-proposal.md) precisam de valores e responsáveis antes da implantação dependente.

## Critério final de aceite reproduzível

1. ADMIN prepara os cadastros necessários.
2. SUPERVISOR agenda uma inspeção válida.
3. O técnico atribuído recebe a inspeção no mobile.
4. O técnico inicia a execução.
5. Trabalha sem rede.
6. Responde o checklist dinâmico.
7. Registra não conformidade.
8. Adiciona evidência.
9. Reinicia o aplicativo sem perder dados.
10. Conclui localmente, mantendo o status oficial anterior até confirmação.
11. Recupera a conexão.
12. Sincroniza sem duplicações e com falhas parciais tratadas.
13. O supervisor atribuído visualiza respostas e evidências.
14. Aprova ou reprova com motivo quando exigido.
15. A decisão chega ao mobile pelo feed incremental.
16. Uma reprovação permite reabertura explícita pelo técnico atribuído.
17. Alterações pendentes, falhas e conflitos permanecem preservados e visíveis.

Cada passo requer evidência de execução no ambiente identificado; a aprovação de D01–D08 por si só não equivale ao aceite do MVP.

Use o [registro de validação integrada](MVP-INTEGRATED-TEST-RECORD.md) para anotar ambiente, versões, resultado e evidência de cada passo.
