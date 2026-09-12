# Especificações pendentes da interface web

## Convenções transversais

Todas as superfícies abaixo devem:

- seguir integralmente o [Design System Industrial Web](../../03-architecture/WEB_DESIGN_SYSTEM.md);
- usar Angular 22, componentes standalone, TypeScript estrito, Angular Material, SCSS, formulários reativos e serviços tipados;
- tratar `loading`, `loaded`, `empty`, `no-results`, `error`, `forbidden`, `processing` e `success` conforme aplicabilidade;
- preservar formulário e contexto após falha recuperável;
- impedir submissão duplicada;
- exibir `fieldErrors` junto aos campos e permitir informar `requestId` em suporte;
- funcionar nos breakpoints compact (1024–1279 px), standard (1280–1599 px) e wide (≥1600 px);
- buscar WCAG 2.1 AA, incluindo teclado, foco visível, semântica, contraste, zoom de 200% e conteúdo não dependente apenas de cor;
- respeitar autorização por papel, vínculo e objeto aplicada pela API;
- possuir testes de componente/serviço e validação visual proporcional ao risco.

Respostas paginadas seguem `PageResponse<T>` com `content`, `page`, `size`, `totalElements` e `totalPages`. Listagens usam `page`, `size` e `sort` somente quando o endpoint suporta paginação.

## Rastreabilidade das specs

| Spec | Requisitos/aceites principais | Regras relacionadas |
| --- | --- | --- |
| WEB-01/02 | FR-AUTH-001, AC-AUTH, AC-SECURITY | RN-001..014 |
| WEB-03 | dashboard mínimo de DEC-012, NFR-008/013 | escopo por ator e estados de interface |
| WEB-04 | FR-AUTH-002, AC-USERS | RN-008..014 |
| WEB-05..07 | FR-MASTER-001, AC-MASTER | RN-015..023 |
| WEB-08..11 | FR-TEMPLATE-001/002, AC-TEMPLATE | RN-024..031 |
| WEB-12..14 | FR-PLAN-001, AC-SCHEDULING; consulta de FR-EXEC/EVID/NC | RN-032..078 e imutabilidade aplicável |
| WEB-15 | FR-REVIEW-001, AC-REVIEW | RN-079..083 |
| WEB-16 | FR-NC-001, AC-NONCONFORMITY | regras de vínculo, evidência e imutabilidade |
| WEB-17 | FR-AUDIT-001, AC-SECURITY | RN-084..090 |
| WEB-18/19 | NFR-008, NFR-011, NFR-013 e AC-RELEASE | DEC-011 e definição de pronto |

Os intervalos RN servem como rota de leitura; a implementação deve identificar as regras exatas afetadas em cada task e manter a matriz requisito → teste → evidência.

## WEB-01 — Autenticação e recuperação de acesso

**Rotas:** `/login`, `/forgot-password`, `/reset-password`, `/change-password`  
**Atores:** usuário não autenticado nas três primeiras; usuário autenticado com troca obrigatória na última.

### Comportamento

- Login solicita e-mail e senha, bloqueia duplo envio e carrega o usuário em `/auth/me` após receber os tokens.
- Erro de credencial não revela se o e-mail existe; indisponibilidade de rede tem mensagem e retry apropriados.
- Recuperação sempre mostra resposta neutra; reset recebe token por URL e nova senha/confirmação.
- `mustChangePassword` redireciona para troca obrigatória antes das áreas protegidas.
- Refresh rotativo atualiza os tokens uma única vez para requisições concorrentes; falha encerra a sessão.
- Logout chama a API com refresh token, limpa o estado local mesmo se a revogação remota falhar e retorna ao login.
- Token, senha e refresh token não aparecem em URL, UI de erro ou logs.

### Endpoints

`POST /auth/login`, `GET /auth/me`, `POST /auth/refresh`, `POST /auth/logout`, `POST /auth/forgot-password`, `POST /auth/reset-password`, `POST /auth/change-password`.

### Aceite mínimo

- login válido e inválido; usuário inativo; troca obrigatória; refresh; expiração; logout; recuperação e reset;
- retorno seguro à rota pretendida somente quando autorizado;
- teste do serviço/interceptor e jornada integrada com a API.

**Estado observado:** login, armazenamento e refresh existem parcialmente; o modelo frontend ainda não representa `mustChangePassword`, logout atual é apenas local e as rotas de recuperação/troca não foram encontradas. **Pendente.**

## WEB-02 — Shell, navegação e autorização de interface

**Rota:** `/app/**`  
**Atores:** ADMIN e SUPERVISOR autenticados.

### Comportamento

- Sidenav e topbar seguem tokens, dimensões e breakpoints do design system.
- Exibir usuário, papel, ambiente e logout; recolher sidenav sem perder nomes acessíveis.
- Itens e ações aparecem conforme papel, mas 403 da API continua tratado como fonte de verdade.
- Acesso direto a rota protegida exige sessão; rota proibida mostra estado seguro sem revelar dados.
- Breadcrumb aparece apenas quando a hierarquia exigir; foco é movido de forma coerente após navegação.

### Endpoints

`GET /auth/me`, `POST /auth/refresh`, `POST /auth/logout`.

### Aceite mínimo

- navegação ADMIN/SUPERVISOR, refresh de página, sessão expirada, 401/403, drawer em 1024 px e teclado completo.

**Estado observado:** shell, guards e interceptors existem, mas não têm cobertura suficiente nem aceite visual. **Pendente.**

## WEB-03 — Dashboard operacional

**Rota:** `/app/dashboard`  
**Atores:** ADMIN e SUPERVISOR, respeitando o escopo retornado pela API.

### Comportamento

- Mostrar contagem por status, atrasadas, aguardando revisão e NC por severidade.
- Atalhos `Criar inspeção` e `Revisar pendências` aparecem somente para ações permitidas.
- Cada indicador acionável abre a lista com filtro equivalente; não exibir métricas avançadas no MVP.
- Cards têm contexto, valor e estado de carregamento; falha parcial não deve fabricar zero.

### Endpoint

`GET /dashboard/summary`.

### Aceite mínimo

- dados reais, escopo por usuário, loading/empty/error, navegação dos cards e responsividade.

**Estado observado:** componente e endpoint existem; integração e escopo precisam de evidência. **Pendente.**

## WEB-04 — Gestão de usuários

**Rota:** `/app/users`  
**Ator:** somente ADMIN.

### Comportamento

- Listar e paginar por papel e status; busca por nome/e-mail só será adicionada quando suportada pelo contrato.
- Criar com nome, e-mail, telefone, papel e senha temporária conforme DTO vigente.
- Editar campos permitidos e alterar status com confirmação contextual.
- Não oferecer `CLIENT_VIEWER` como capacidade funcional do MVP além do que o domínio autorizar explicitamente.
- Para planejamento, buscar somente técnicos ativos e atribuíveis.

### Endpoints

`GET/POST /users`, `GET/PUT /users/{id}`, `PATCH /users/{id}/status`, `GET /users/assignable`.

### Aceite mínimo

- ADMIN permitido; SUPERVISOR e acesso direto proibidos; duplicidade de e-mail; paginação; estados; confirmação e erros de campo.

**Estado observado:** listagem/formulário estão presentes; busca textual não é suportada pela API e testes são insuficientes. **Pendente.**

## WEB-05 — Gestão de clientes

**Rota:** `/app/clients`  
**Atores:** ADMIN/SUPERVISOR conforme autorização vigente.

### Comportamento

- Listar/paginar com filtros de nome e status; criar, consultar, editar e alterar status.
- Abrir os locais vinculados sem perder o contexto do cliente.
- Cliente inativo não aparece em novo planejamento; histórico permanece consultável.

### Endpoints

`GET/POST /clients`, `GET/PUT /clients/{id}`, `PATCH /clients/{id}/status`, `GET /clients/{clientId}/sites`.

### Aceite mínimo

- validações do DTO, paginação/filtros, duplicidade quando aplicável, inativação e drill-down.

**Estado observado:** rota e serviço existem; falta aceite automatizado, visual e integrado. **Pendente.**

## WEB-06 — Gestão de locais

**Rotas:** `/app/sites`, `/app/clients/:clientId/sites`  
**Atores:** ADMIN/SUPERVISOR conforme autorização vigente.

### Comportamento

- No contexto de cliente, listar/paginar por status, criar local, consultar, editar e alterar status.
- Exibir endereço, contato, coordenadas opcionais e vínculo com cliente.
- Abrir equipamentos do local; local inativo não participa de novo planejamento.
- A rota global deve exigir seleção de cliente antes da consulta ou depender de novo endpoint global documentado.

### Endpoints

`GET/POST /clients/{clientId}/sites`, `GET/PUT /sites/{id}`, `PATCH /sites/{id}/status`.

### Aceite mínimo

- vínculo obrigatório, troca de contexto segura, status, drill-down, paginação e comportamento explícito da rota global.

**Estado observado:** o backend não possui `GET /sites` nem `POST /sites`; qualquer chamada global pelo `CrudService` é inválida. **Pendente com `API_GAP` ou ajuste de UX para seleção de cliente.**

## WEB-07 — Gestão de equipamentos

**Rotas:** `/app/equipment`, `/app/sites/:siteId/equipment`  
**Atores:** ADMIN/SUPERVISOR conforme autorização vigente.

### Comportamento

- No contexto de local, listar/paginar por status, criar, consultar, editar e alterar status.
- Exibir nome, patrimônio, série, fabricante, modelo, descrição, QR, instalação e situação conforme DTO.
- Consulta por QR deve codificar o valor com segurança e tratar inexistência.
- Equipamento inativo não participa de novo planejamento.
- A rota global deve exigir seleção de cliente/local ou depender de endpoint global documentado.

### Endpoints

`GET/POST /sites/{siteId}/equipment`, `GET/PUT /equipment/{id}`, `GET /equipment/by-qr/{qrCode}`, `PATCH /equipment/{id}/status`.

### Aceite mínimo

- vínculo com local, QR único, filtros, status, paginação e comportamento explícito da rota global.

**Estado observado:** não existem `GET /equipment` nem `POST /equipment` top-level. **Pendente com `API_GAP` ou ajuste de UX.**

## WEB-08 — Lista e ciclo de vida de modelos

**Rota:** `/app/inspection-templates`  
**Atores:** ADMIN/SUPERVISOR conforme autorização vigente.

### Comportamento

- Listar/paginar por status e categoria; criar rascunho; abrir edição, prévia e versões.
- Alteração de status e publicação exigem feedback e confirmação proporcionais.
- Versão publicada é imutável; nova edição ocorre sobre rascunho conforme regra vigente.

### Endpoints

`GET/POST /inspection-templates`, `GET/PUT /inspection-templates/{id}`, `PATCH /inspection-templates/{id}/status`, `POST /inspection-templates/{id}/publish`.

### Aceite mínimo

- filtros, paginação, estados vazio/sem resultado, criação, status, publicação válida/inválida e permissões.

**Estado observado:** rotas/componentes existem sem cobertura suficiente. **Pendente.**

## WEB-09 — Construtor de modelo

**Rota:** `/app/inspection-templates/:templateId/edit`

### Comportamento

- Editar título, descrição e categoria do rascunho.
- Criar, editar, excluir e ordenar seções e itens.
- Configurar tipo de resposta, obrigatoriedade e regras de observação/evidência.
- Separar estrutura, propriedades e prévia; drag-and-drop nunca é o único mecanismo.
- Validar pertencimento entre modelo, seção e item e impedir edição indevida de versão publicada.

### Endpoints

`GET /inspection-templates/{id}/draft`, `PUT /inspection-templates/{id}`, `POST/PUT/DELETE /inspection-templates/{id}/sections[...]`, `POST/PUT/DELETE /inspection-templates/{id}/sections/{sectionId}/items[...]`, `POST /inspection-templates/{id}/publish`.

### Aceite mínimo

- CRUD completo, ordenação acessível, validação de cada tipo, estado sujo, erro preservando edição, publicação e regressão de vínculos cruzados.

**Estado observado:** builder existe parcialmente; aceite funcional, acessível e integrado é pendente.

## WEB-10 — Prévia de modelo

**Rota:** `/app/inspection-templates/:templateId/preview`

### Comportamento

- Apresentar somente leitura com seções, ordem, itens, instruções, tipos, obrigatoriedade e regras condicionais.
- Diferenciar rascunho da versão publicada e informar qual fonte está sendo visualizada.

### Endpoints

`GET /inspection-templates/{id}/draft`, `GET /inspection-templates/{id}/versions/{versionNumber}`.

### Aceite mínimo

- fidelidade estrutural, tipos de resposta, vazio/erro e teclado/leitura sem permitir mutação.

**Estado observado:** componente existe; falta comprovação. **Pendente.**

## WEB-11 — Histórico de versões

**Rota:** `/app/inspection-templates/:templateId/versions`

### Comportamento

- Listar versões em ordem clara, com número, autor/data quando fornecidos e estrutura somente leitura.
- Abrir uma versão específica; esclarecer que inspeções mantêm snapshot histórico.

### Endpoints

`GET /inspection-templates/{id}/versions`, `GET /inspection-templates/{id}/versions/{versionNumber}`.

### Aceite mínimo

- lista, detalhe, vazio, erro e imutabilidade visual/funcional.

**Estado observado:** componente existe; falta teste e integração comprovada. **Pendente.**

## WEB-12 — Lista e acompanhamento de inspeções

**Rota:** `/app/inspections`  
**Atores:** ADMIN/SUPERVISOR; escopo efetivo calculado pela API.

### Comportamento

- Paginar e filtrar por técnico, supervisor, cliente, local, status, prioridade e intervalo agendado.
- Exibir código, modelo, vínculos, data, prioridade, status, atraso e atualização conforme resposta disponível.
- Atalho de aguardando revisão deve produzir filtro equivalente.
- Filtros de equipamento, atrasadas e texto dependem de suporte de API; não filtrar página parcial no cliente como se fosse consulta completa.

### Endpoints

`GET /inspections`, `GET /inspections/{id}`, `GET /users/assignable`, `GET /clients`, `GET /clients/{clientId}/sites`.

### Aceite mínimo

- filtros suportados, paginação, escopo ADMIN/SUPERVISOR, URL/estado de filtros, empty/no-results e navegação para detalhe/revisão.

**Estado observado:** lista existe; filtros previstos além do contrato atual são `API_GAP`. **Pendente.**

## WEB-13 — Planejamento e atribuição

**Rota:** `/app/inspections/new`  
**Atores:** conforme regra vigente de criação; execução e revisão continuam restritas aos responsáveis atribuídos.

### Comportamento

- Selecionar modelo/versão publicada ativa, cliente, local, equipamento quando aplicável, técnico ativo, supervisor, data/prazo, prioridade e instruções.
- Cliente filtra locais; local filtra equipamentos; mudança do pai limpa seleção incompatível.
- Exibir resumo antes da confirmação e mapear erros de vínculo ao grupo correto.
- Não usar a lista administrativa geral para técnicos atribuíveis; usar `/users/assignable`.

### Endpoints

`GET /inspection-templates`, `GET /inspection-templates/{id}/versions`, `GET /clients`, `GET /clients/{clientId}/sites`, `GET /sites/{siteId}/equipment`, `GET /users/assignable`, `POST /inspections`, `POST /inspections/{id}/assign` quando atribuição posterior for necessária.

### Aceite mínimo

- cascata de dependências, somente ativos, validação de modelo/versão, datas, prioridade, responsáveis, sucesso e conflitos de domínio.

**Estado observado:** tela existe; integração completa e regras de elegibilidade precisam de teste. **Pendente.**

## WEB-14 — Detalhe, histórico e cancelamento de inspeção

**Rota:** `/app/inspections/:inspectionId`

### Comportamento

- Exibir snapshot, vínculos, datas, prioridade, status, respostas, evidências, NC, revisões e auditoria relacionada.
- Carregar seções de forma independente sem ocultar falhas parciais.
- Ações respeitam papel/estado; cancelamento exige motivo e confirmação contextual.
- Evidência abre conteúdo autorizado sem expor URL permanente; PDF e imagem têm fallback adequado.
- Inspeção aprovada/cancelada e dependentes aparecem somente leitura.

### Endpoints

`GET /inspections/{id}`, `GET /inspections/{id}/responses`, `GET /inspections/{id}/evidence`, `GET /inspections/{id}/evidence/{evidenceId}/content`, `GET /inspections/{id}/non-conformities`, `GET /inspections/{id}/reviews`, `GET /inspections/{id}/history`, `POST /inspections/{id}/cancel`.

### Aceite mínimo

- detalhe completo, falhas parciais, download autorizado, imutabilidade e cancelamento permitido/proibido.

**Estado observado:** detalhe existe, mas o serviço web não expõe conteúdo/histórico/respostas integralmente. **Pendente.**

## WEB-15 — Revisão de inspeção

**Rota:** `/app/inspections/:inspectionId/review`  
**Ator:** somente supervisor atribuído.

### Comportamento

- Abrir revisão de `SUBMITTED`, carregando respostas por seção, observações, evidências, NC, localização e histórico.
- `begin-review` é explícito; aprovar e reprovar são ações distintas.
- Reprovação exige motivo; comentários opcionais seguem contrato.
- Exibir erro de concorrência/estado inválido sem perder contexto.
- ADMIN e supervisor não atribuído não executam decisão.

### Endpoints

`GET /inspections/{id}`, `GET /inspections/{id}/responses`, `GET /inspections/{id}/evidence`, `GET /inspections/{id}/evidence/{evidenceId}/content`, `GET /inspections/{id}/non-conformities`, `GET /inspections/{id}/reviews`, `POST /inspections/{id}/begin-review`, `POST /inspections/{id}/approve`, `POST /inspections/{id}/reject`.

### Aceite mínimo

- supervisor atribuído positivo; ADMIN, supervisor alheio e estado inválido negativos; motivo obrigatório; evidência autorizada; feedback e auditoria.

**Estado observado:** tela e ações básicas existem; falta validação integrada e cobertura adversarial. **Pendente.**

## WEB-16 — Não conformidades

**Rota:** `/app/non-conformities` e contexto em detalhe/revisão.

### Comportamento

- Dentro de uma inspeção, listar e consultar NC com severidade, status, descrição, item e evidência.
- Alteração de status somente quando papel e workflow permitirem.
- A listagem global prevista requer filtros por inspeção, severidade, status e período.
- Não implementar plano de ação completo, pois é FUTURE.

### Endpoints

Disponíveis somente no contexto: `GET /inspections/{inspectionId}/non-conformities`, `GET /inspections/{inspectionId}/non-conformities/{id}`, `PATCH /inspections/{inspectionId}/non-conformities/{id}/status`.

### Aceite mínimo

- contexto correto, estados, severidade, evidência, imutabilidade após aprovação/cancelamento e autorização.

**Estado observado:** não existe endpoint global para sustentar `/app/non-conformities`. **Pendente com `API_GAP` para a rota global; consulta contextual pode avançar.**

## WEB-17 — Auditoria

**Rota:** `/app/audit`  
**Atores:** conforme autorização vigente; somente leitura.

### Comportamento

- Paginar e mostrar data, ator, ação, tipo/recurso e identificador.
- Filtros atuais suportados: `entityType`, `entityId`, `actorId`.
- Filtros previstos por período, ação, inspeção e motivo dependem de ampliação da API.
- Não renderizar token, senha, credencial ou conteúdo sensível mesmo que recebido indevidamente.

### Endpoint

`GET /audit-events`.

### Aceite mínimo

- somente leitura, filtros suportados, paginação, timezone, segurança de conteúdo e escopo de acesso.

**Estado observado:** lista existe; filtros completos da especificação são `API_GAP`. **Pendente.**

## WEB-18 — Fundação do design system

**Escopo:** transversal.

### Comportamento

- Implementar Nunito Sans, tokens de cor/spacing/radius/motion e tema Angular Material.
- Criar padrões compartilhados para cabeçalho, estado de página, status/priority badge, tabela, filtros, confirmação e feedback.
- Remover valores locais equivalentes gradualmente, sem reescrita cosmética desconectada dos fluxos.
- Documentar variantes e estados no próprio código/testes; Storybook é opcional, não gate do MVP.

### Aceite mínimo

- nenhuma feature nova usa cor ou métrica avulsa quando há token;
- componentes compartilhados cobrem estados e acessibilidade previstos;
- inspeção visual nos três breakpoints e regressão dos consumidores.

**Estado observado:** o documento existe, mas a aplicação ainda precisa ser auditada e alinhada. **Pendente.**

## WEB-19 — Qualidade, integração e entrega

**Escopo:** transversal.

### Comportamento

- Criar testes de componentes e serviços para todas as superfícies críticas.
- Executar jornada real web → API para login, cadastros, modelo, planejamento, acompanhamento, revisão, auditoria e logout.
- Executar auditoria WCAG 2.1 AA e regressão responsiva.
- Configurar CI com testes, build e Prettier.
- Publicar em S3 privado + CloudFront com OAC, HTTPS, fallback de SPA, cache e configuração externa da API quando a infraestrutura estiver disponível.

### Aceite mínimo

- `npm test -- --watch=false`, `npm run build` e `npx prettier --check src` passam;
- matriz spec → teste → evidência completa;
- API real validada sem alegação baseada em mock;
- rotas funcionam após refresh no ambiente publicado;
- CORS, sessão e erros funcionam via HTTPS.

**Estado observado:** somente um teste frontend foi encontrado; hosting e evidência integrada permanecem pendentes.
