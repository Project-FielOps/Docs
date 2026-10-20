# Especificação funcional do aplicativo FieldOps

**Tipo:** especificação de produto, catálogo de telas e critérios de aceite  
**Responsabilidade:** Produto e Projeto (P.O./P.M.)  
**Data de consolidação:** 2026-09-07  
**Status:** ativo para detalhamento do MVP; itens explicitamente marcados como P1 ou FUTURE não fazem parte do aceite mínimo

## 1. Objetivo deste documento

Este documento transforma a documentação de produto do FieldOps em uma visão operacional completa das telas, capacidades, permissões, estados, fluxos e critérios de aceite necessários para as três superfícies do produto:

1. API central, autoridade de autenticação, autorização, workflow, dados e auditoria;
2. painel administrativo web, usado para cadastro, modelagem, planejamento, acompanhamento e revisão;
3. aplicativo mobile Android, usado pelo técnico para executar inspeções em campo, inclusive sem conectividade.

Ele também registra a validação da superfície atualmente encontrada no repositório. A existência de uma rota, componente ou endpoint é evidência de implementação, mas não equivale a aceite funcional. O aceite exige fluxo completo, autorização por perfil e objeto, persistência, tratamento de erro e teste correspondente.

## 2. Fontes e precedência

Esta especificação deve ser lida em conjunto com:

- [Escopo do MVP](product-mvp.md);
- [Requisitos funcionais](requirements-functional.md);
- [Regras de negócio](product-business-rules.md);
- [Papéis e permissões](user-roles.md);
- [Fluxos principais](user-flows.md);
- [Rastreabilidade](requirements-traceability.md);
- [Arquitetura do mobile](../03-architecture/mobile.md);
- [Design do mobile](../03-architecture/mobile-design.md);
- [Especificações da API](../05-api/specs/README.md);
- [Diagnóstico de divergências](../99-reference/diagnostics/discovery.md).

Quando houver conflito, aplica-se a hierarquia da Knowledge Base: decisões aprovadas em `12-decisions`, documentação consolidada da KB, código como evidência AS-IS e referências históricas. Uma tela presente no código não autoriza um perfil a executar uma operação proibida pela regra de negócio.

## 3. Visão do produto e limites

### 3.1 Problema resolvido

O FieldOps substitui a fragmentação de formulários, fotografias, mensagens e decisões por um fluxo rastreável:

`cadastro → modelo publicado → planejamento → execução → evidências → sincronização → revisão → aprovação/reprovação → auditoria`.

### 3.2 Escopo do MVP

O MVP deve permitir que uma organização prestadora opere inspeções para vários clientes, com um técnico responsável e um supervisor responsável por inspeção. O fluxo deve funcionar de ponta a ponta desde o painel web, continuar no Android offline e retornar ao painel para revisão.

Obrigatório:

- autenticação, refresh, logout, recuperação/troca de senha e sessões seguras;
- gestão de usuários e situação do usuário;
- gestão de clientes, locais e equipamentos;
- criação, edição, prévia, publicação e versionamento de modelos;
- agendamento com cliente, local, modelo publicado, equipamento quando aplicável, técnico, supervisor, prioridade, prazo e instruções;
- execução do técnico atribuído, com checklist dinâmico, observações, fotografia, não conformidade, QR e localização quando exigidos;
- persistência local, download prévio, retomada, outbox, push idempotente, pull por cursor, retry e conflito explícito;
- revisão exclusiva do supervisor atribuído, com aprovação ou reprovação justificada;
- correção após reprovação com reabertura explícita, preservando histórico;
- dashboard mínimo, auditoria, erros padronizados, migrações, OpenAPI e testes críticos.

Fora do MVP: portal do cliente, multiempresa, ordens de serviço, planos de ação completos, manutenção preventiva, roteirização, geofencing, áudio/vídeo, OCR, IA, IoT, ERP, webhooks e relatórios regulatórios específicos.

### 3.3 Estados oficiais e locais

O estado do workflow da inspeção é independente do estado local de sincronização.

| Dimensão | Estados/valores | Significado |
|---|---|---|
| Workflow | `ASSIGNED`, `IN_PROGRESS`, `SUBMITTED`, `UNDER_REVIEW`, `APPROVED`, `REJECTED`, `CANCELED` | Estado reconhecido pelo backend |
| Sincronização | `synced`, `pending`, `syncing`, `error`, `conflict` | Estado do dispositivo/outbox |
| Usuário | ativo/inativo | Usuário inativo não inicia nova sessão |
| Modelo | rascunho/publicado/inativo | Publicação cria versão imutável |
| Evidência | pendente/sincronizada/erro/conflito | Resultado do envio do arquivo |

Uma conclusão offline deve ser comunicada como “concluída no dispositivo — envio pendente”; não pode ser exibida como aprovada ou como enviada ao servidor antes da confirmação da API.

## 4. Usuários, responsabilidades e autorização

### 4.1 ADMIN — administrador

Pode autenticar, administrar usuários, clientes, locais, equipamentos, modelos e consultar auditoria conforme escopo definido. Não executa inspeção, não responde checklist, não revisa, não aprova e não reprova por padrão.

### 4.2 SUPERVISOR — supervisor

Pode autenticar, administrar modelos, planejar inspeções dentro do escopo permitido, acompanhar inspeções e revisar somente inspeções sob sua responsabilidade. Não executa inspeções como técnico e não pode decidir uma inspeção sem vínculo de supervisor.

### 4.3 TECHNICIAN — técnico

Pode autenticar, consultar somente inspeções atribuídas a si, baixar dados autorizados, iniciar, responder, registrar observações/evidências, corrigir uma reprovação após reabertura e enviar para revisão. Não pode alterar cadastro, modelo ou decisão do supervisor.

### 4.4 CLIENT_VIEWER — visualizador de cliente

É um papel reconhecido no domínio, mas permanece FUTURE no MVP. Não deve aparecer como fluxo concluído nem receber permissões implícitas.

### 4.5 Regras de segurança aplicadas a todas as telas

- toda ação protegida deve ser autorizada no backend por perfil e por objeto;
- filtros enviados pelo cliente não substituem escopo calculado pela identidade autenticada;
- recurso inexistente e recurso fora do escopo devem ter resposta segura, sem vazamento;
- inspeção aprovada não pode ser alterada por fluxo comum;
- cancelamento exige justificativa e não se aplica normalmente a inspeção aprovada;
- histórico não pode ser apagado quando o registro é inativado;
- tokens, senhas, conteúdo sensível de evidências e credenciais não vão para logs;
- toda mutação crítica deve produzir auditoria com ator, data, recurso, ação e motivo quando aplicável.

## 5. Mapa de navegação do painel web

```text
/login
└── /app
    ├── /dashboard
    ├── /users
    ├── /clients
    │   └── /:clientId/sites
    ├── /sites
    │   └── /:siteId/equipment
    ├── /equipment
    ├── /inspection-templates
    │   ├── /new
    │   ├── /:templateId/edit
    │   ├── /:templateId/preview
    │   └── /:templateId/versions
    ├── /inspections
    │   ├── /new
    │   ├── /:inspectionId
    │   └── /:inspectionId/review
    ├── /non-conformities
    └── /audit
```

O shell deve possuir navegação, usuário autenticado, papel visível, logout, estados de carregamento, vazio e erro. A visibilidade do menu é conveniência; a API continua sendo a autoridade.

## 6. Catálogo de telas do painel web

### WEB-01 — Login

**Atores:** todos os usuários ativos.  
**Rota:** `/login`.  
**Objetivo:** iniciar sessão no sistema.

Elementos: e-mail, senha, Entrar, loading, mensagens acionáveis, recuperação de senha quando exposta. Critérios: ativo entra; inativo e credencial inválida falham sem vazamento; refresh e logout protegem a sessão; token não vai para log.

**Estado observado:** a rota existe no Angular. O aceite de produção depende de execução integrada com a API e dos testes de sessão registrados na KB.

### WEB-02 — Shell e sessão

**Rota base:** `/app`. **Atores:** autenticados.

Deve conter menu contextual, nome/e-mail/papel, logout, navegação permitida, tratamento de 401/403 e preservação de `requestId`. Não deve permitir acesso direto a rotas por digitação nem confiar apenas em esconder botões.

**Estado observado:** shell, guard de autenticação, guard de papel e interceptor existem.

### WEB-03 — Dashboard operacional

**Rota:** `/app/dashboard`. **Atores:** ADMIN/SUPERVISOR.

Mostrar inspeções por estado, atrasadas, aguardando revisão, NC por severidade, Nova inspeção e Aguardando revisão, além de loading/vazio/erro. Números vêm da API, respeitam escopo e levam a filtros equivalentes.

**Estado observado:** resumo, atrasos, revisão, NC e atalhos existem; escopo e dados reais requerem integração.

### WEB-04 — Usuários

**Rota:** `/app/users`. **Atores:** somente ADMIN.

Buscar/filtrar por nome, e-mail, papel e situação; criar/editar; alterar situação; tratar duplicidade. Formulário: nome, e-mail, telefone, papel, senha inicial e erros de campo. Inativar preserva histórico; usuário inativo não recebe inspeção; somente ADMIN gerencia.

**Estado observado:** tabela, filtros, diálogo, papéis e status existem; `CLIENT_VIEWER` é opção de cadastro, não portal implementado.

### WEB-05 — Clientes

**Rota:** `/app/clients`. **Atores:** ADMIN/SUPERVISOR conforme escopo.

Listar, buscar, criar, editar, inativar e abrir locais. Cliente inativo não aparece em novo planejamento; local novo sempre pertence a cliente; histórico é preservado.

**Estado observado:** rota/listagem e serviço existem; aceite do formulário requer integração.

### WEB-06 — Locais

**Rotas:** `/app/sites` e `/app/clients/:clientId/sites`.

Selecionar cliente, listar, buscar, criar, editar, inativar e abrir equipamentos. Todo local pertence a cliente; local inativo não entra em nova inspeção; equipamento/inspeção devem ser compatíveis.

**Estado observado:** rotas aninhadas e listagem existem.

### WEB-07 — Equipamentos

**Rotas:** `/app/equipment` e `/app/sites/:siteId/equipment`.

Campos: local, nome, patrimônio, série, fabricante, modelo, descrição, QR, instalação e situação. Buscar por texto, status e QR; QR é único; inativo não é atribuído; QR do mobile deve validar local.

**Estado observado:** listagem, filtros, busca QR, diálogo e status existem.

### WEB-08 — Lista de modelos

**Rota:** `/app/inspection-templates`.

Listar título, categoria, situação, versão e data; criar, editar rascunho, prévia, versões, publicar e inativar. Publicação exige título, categoria, seção/item válido, tipo de resposta e ordem explícita; versão publicada é imutável.

**Estado observado:** lista, criação, prévia, versões e status têm rotas/componentes.

### WEB-09 — Editor de modelo

**Rota:** `/app/inspection-templates/:templateId/edit`.

Editar metadados, seções e itens de rascunho; definir tipo, obrigatório, observação/evidência em falha e ordem; salvar, validar e publicar. Operações devem validar que seção/item pertencem ao modelo. Publicado gera nova versão.

**Estado observado:** builder e formulário de seção existem; validações cruzadas devem ser testadas.

### WEB-10 — Prévia do modelo

**Rota:** `/app/inspection-templates/:templateId/preview`.

Exibir checklist como o técnico receberá, com seções, ordem, instrução, tipo, obrigatoriedade e regras. É somente leitura.

### WEB-11 — Histórico de versões

**Rota:** `/app/inspection-templates/:templateId/versions`.

Exibir versões, autor, data e situação. Inspeção existente mantém versão/snapshot mesmo com nova publicação ou inativação.

**Estado observado:** histórico e vazio existem.

### WEB-12 — Lista de inspeções

**Rota:** `/app/inspections`.

Filtros: estado, técnico, supervisor, cliente, local, equipamento, prioridade, intervalo, atrasadas e aguardando revisão. Exibir código, modelo, vínculos, data, prioridade, estado, atraso, progresso e atualização.

ADMIN administra sem executar/decidir; SUPERVISOR vê seu escopo; técnico não deve escapar da regra usando web.

**Estado observado:** rota/lista e filtros de prioridade/período existem; cobertura completa requer teste.

### WEB-13 — Nova inspeção / planejamento

**Rota:** `/app/inspections/new`. **Atores:** SUPERVISOR.

Fluxo: modelo/versão publicada ativa → cliente → local → equipamento quando exigido → técnico ativo → supervisor → data/prazo → prioridade → instruções → revisão. Validar compatibilidade, ativo, escopo, modelo ativo, obrigatoriedades e atribuição pós-sync. Cancelamento futuro exige justificativa.

**Estado observado:** rota e componente existem; integração de atribuição pendente.

### WEB-14 — Detalhe da inspeção

**Rota:** `/app/inspections/:inspectionId`.

Exibir snapshot, vínculos, datas, prioridade, status, progresso, respostas, evidências, NC, revisões, histórico e ações compatíveis. Aprovação congela inspeção e dependentes.

**Estado observado:** rota existe.

### WEB-15 — Revisão

**Rota:** `/app/inspections/:inspectionId/review`. **Atores:** supervisor atribuído.

Mostrar respostas por item, pendências, observações, fotos/documentos autorizados, NC, localização, histórico e ações Aprovar/Reprovar. Reprovar exige motivo; correção exige reabertura explícita; decisão é auditada; fora do escopo falha.

**Estado observado:** rota/componente existem; transições e autorização precisam de teste integrado.

### WEB-16 — Não conformidades

**Rota:** `/app/non-conformities`.

Listar por inspeção, severidade, estado, data, responsável e evidência; mostrar origem no item e histórico. NC crítica exige descrição/evidência quando a regra se aplicar; aprovada/cancelada fica imutável; planos de ação completos são FUTURE.

**Estado observado:** rota/lista existem; filtros e bloqueios por estado pendentes de comprovação.

### WEB-17 — Auditoria

**Rota:** `/app/audit`.

Filtrar por data, ator, ação, recurso, inspeção e motivo; cobrir autenticação, cadastros, publicação, planejamento, respostas, evidências, correção, decisões e conflito. Não expor tokens/senhas.

**Estado observado:** rota e controller existem.

## 7. Mapa e catálogo de telas do mobile Android

```text
/ → /(auth)/login ou /(tabs)/home
/(tabs): home | inspections | history | profile
/notifications
/inspections/:id: detalhe | checklist | evidências | revisão
```

O mobile não é painel administrativo: planejamento, modelagem, gestão e decisão ficam no web.

### MOB-01 — Entrada e login

Inicialização decide por sessão local; login tem e-mail, senha, loading e erro. Deve usar API real, Secure Store e tratar sessão expirada.

**Estado observado:** existe redirect/login, mas a tela indica ambiente de demonstração e dados mockados. Parcial/mock.

### MOB-02 — Home

Próxima inspeção, resumo do dia, pendências, conectividade, sincronização, Sincronizar agora e notificações. Estados vazio/offline/erro são obrigatórios.

### MOB-03 — Lista de inspeções

Busca por título/local/código; filtros interativos por status; cards com prioridade, progresso, pendência de sync e conflito. Loading/vazio/erro/offline devem ser explícitos.

**Estado observado:** busca/cards existem; filtros de status aparecem, mas não são interativos no código observado; dados vêm de store/mock.

### MOB-04 — Histórico

Inspeções anteriores do técnico, inclusive submissões/revisões/rejeições/cancelamentos e correções. Não apagar pendentes, erros ou conflitos.

**Estado observado:** tela/filtro existem; SQLite e retenção não comprovados.

### MOB-05 — Perfil e notificações

Perfil mostra identidade, papel, código, última sync real, política offline e logout. Notificações mostram atribuição, sync, lembrete e prioridade, com lida/deep link.

**Estado observado:** telas visuais existem; sync estática e notificações mockadas.

### MOB-06 — Detalhe da inspeção

Código, título, status, data, local, descrição, progresso, evidências, conflito e ações por estado: iniciar, continuar, reabrir correção ou leitura.

**Estado observado:** detalhe e transições locais existem; backend/escopo não comprovados.

### MOB-07 — Checklist

Snapshot dinâmico, seções, instrução, tipo, obrigatório, progresso e regras de falha. Salvar local antes da rede, recuperar após reinício, outbox e idempotência.

**Estado observado:** itens/opções/progresso em memória; persistência/outbox não comprovadas.

### MOB-08 — Evidências/observações

Captura real de foto, permissões, thumbnails, vínculo com item/NC, observação e status de envio. Aceitar JPEG/JPG/PNG/PDF, 10 MB/arquivo, 10/inspeção, com validação e preservação de pendências.

**Estado observado:** lista/observação e “Simular captura de foto” existem; câmera/upload real não comprovados.

### MOB-09 — QR e localização

Durante detalhe/checklist, solicitar câmera/localização no momento de uso; validar QR/equipamento/local, registrar coordenada, precisão, data, permissão negada e pendência. QR e localização são obrigatórios quando configurados.

**Estado observado:** não foram encontrados fluxos nativos no inventário mobile; `requiresLocation` aparece apenas no domínio/mock.

### MOB-10 — Revisão e conclusão

Resumo, obrigatórios, evidências, conexão e conflito; bloquear incompleto; offline marca conclusão local pendente; online exige confirmação da API; falha conserva dados.

**Estado observado:** revisão e bloqueio existem, mas conclusão altera store local e não comprova envio real.

### MOB-11 — Sincronização/conflito

Acessível por Home/Perfil/cards. Mostrar outbox, operação, tentativa, erro, retry, dependências, cursor e conflito. Push idempotente; pull após persistência; conflito explícito; sync automático ao voltar rede.

**Estado observado:** tipos/textos de status existem, mas central, SQLite e outbox não foram identificados.

## 8. Funcionalidades transversais e definição de pronto

Todas as telas devem possuir loading, vazio, erro, sucesso e autorização; mutações preservam formulário em falha. O mobile deve manter áreas de toque de 44 pontos, labels, contraste e feedback que não dependa apenas de cor. Dados seguem `cliente → local → equipamento → modelo/versão/snapshot → inspeção → respostas/evidências/NC → revisão`.

Uma tela só é pronta quando possui rota, fluxo completo, contrato vigente, autorização no backend, persistência adequada, auditoria, testes permitidos/proibidos e, no mobile offline, recuperação após reinício e sem rede. Não usar mock, “simular” ou valor estático no caminho de produção.

### 8.1 Verificação operacional de usabilidade

Esta verificação complementa os critérios de aceite das telas; não substitui pesquisa com usuários nem declara que ela foi realizada. QA deve executar os cenários com os papéis e dados de teste previstos e guardar resultado reproduzível.

| Cenário | Critério observável de aprovação |
|---|---|
| Planejar uma inspeção | ADMIN/SUPERVISOR identifica os campos obrigatórios antes de enviar; erro informa o campo e como corrigir; após falha recuperável, os valores já preenchidos permanecem. |
| Executar checklist no mobile | Técnico distingue claramente item respondido, obrigatório pendente e ação disponível; salvar/erro offline informa se o dado ficou local ou foi sincronizado. |
| Revisar uma inspeção | Supervisor identifica responsável, estado e evidências antes de decidir; reprovação informa que o motivo é obrigatório; decisão inválida não apaga o contexto. |
| Encontrar dados | Busca sem correspondência informa “sem resultados” e permite ajustar filtros; estado vazio inicial sugere somente uma ação disponível e autorizada. |
| Recuperar erro | Mensagem nomeia o que falhou e a próxima ação segura; retry não cria duplicação; ação em andamento não pode ser enviada repetidamente. |

Para cada cenário, registrar papel, ambiente, dados de teste, resultado observado e evidência. `PASS` exige todos os critérios da linha; comportamento não observado ou apenas inferido do código fica `NOT RUN`, não `PASS`. A revisão de acessibilidade segue o checklist próprio em [WEB-ACCESSIBILITY-AUDIT](../11-planning/web-pendencies/WEB-ACCESSIBILITY-AUDIT.md).

## 9. Aceite ponta a ponta do MVP

1. ADMIN cria usuários ativos e cadastros.
2. SUPERVISOR cria/publica modelo e planeja inspeção completa.
3. TECHNICIAN sincroniza e vê somente sua atribuição.
4. Técnico baixa, perde rede, inicia, responde, registra falha/observação/foto, QR/localização quando exigidos.
5. App reinicia e recupera dados.
6. Técnico é bloqueado por obrigatório ausente e conclui localmente quando válido.
7. Retorno da rede executa push idempotente/pull por cursor e envia para revisão.
8. Supervisor atribuído revisa e reprova com motivo.
9. Técnico reabre explicitamente, corrige e reenvia preservando histórico.
10. Supervisor aprova e recursos ficam protegidos.
11. Auditoria registra ações sem tokens/senhas.
12. Repetições e acessos fora de escopo falham com segurança.

## 10. Priorização

**P0:** autenticação/autorização, cadastros, templates/versionamento/snapshot, planejamento, checklist persistente, evidências, NC, QR, localização, SQLite/outbox/sync/conflito, revisão/correção, dashboard mínimo, auditoria, testes e build Android.

**P1:** notificações, assinatura desenhada, PDF, histórico detalhado, comentários por item, biometria, tema escuro, CSV, reatribuição, comparação e assistência de conflito.

**FUTURE:** portal do cliente, multiempresa, planos de ação completos, ordens de serviço, manutenção, assinatura avançada, geofencing, rotas, áudio/vídeo, OCR, IA, IoT, ERP, webhooks e relatórios regulatórios.
