# Backlog da interface administrativa web

## Estados e fluxo

- `BACKLOG`: identificada, mas ainda não pronta para execução.
- `READY`: especificação e dependências permitem iniciar.
- `IN_PROGRESS`: há implementação parcial ou execução corrente, sem handoff aprovado.
- `VALIDATION`: Frontend Agent entregou `READY_FOR_WEB_QA`.
- `BLOCKED`: depende de contrato, decisão, ambiente ou outra task.
- `DONE`: Web QA aprovou com evidência e o commit local foi criado.

Fluxo: `BACKLOG → READY → IN_PROGRESS → VALIDATION → DONE`. `CHANGES_REQUIRED` retorna para `IN_PROGRESS`.

## Visão geral

| ID | Spec | Task | Prioridade | Dependências | Status |
| --- | --- | --- | --- | --- | --- |
| WEB-T001 | WEB-18 | Implementar tokens, Nunito Sans e tema Angular Material | P0 | — | `READY` |
| WEB-T002 | WEB-18 | Criar shell responsivo conforme design system | P0 | WEB-T001 | `IN_PROGRESS` |
| WEB-T003 | WEB-18 | Criar componentes compartilhados de estado, badges, tabela, filtros e confirmação | P0 | WEB-T001 | `READY` |
| WEB-T004 | WEB-01 | Completar login, `/auth/me` e erros de autenticação | P0 | WEB-T001 | `IN_PROGRESS` |
| WEB-T005 | WEB-01/02 | Implementar refresh concorrente, expiração e retorno seguro | P0 | WEB-T004 | `IN_PROGRESS` |
| WEB-T006 | WEB-01/02 | Integrar logout remoto e limpeza local resiliente | P0 | WEB-T005 | `READY` |
| WEB-T007 | WEB-01 | Implementar recuperação, reset e troca obrigatória de senha | P0 | WEB-T004 | `READY` |
| WEB-T008 | WEB-02 | Fechar guards, 401/403 e matriz de navegação ADMIN/SUPERVISOR | P0 | WEB-T005 | `IN_PROGRESS` |
| WEB-T009 | WEB-03 | Integrar e validar dashboard mínimo | P0 | WEB-T003, WEB-T008 | `IN_PROGRESS` |
| WEB-T010 | WEB-04 | Completar usuários, paginação, status e validações | P0 | WEB-T003, WEB-T008 | `IN_PROGRESS` |
| WEB-T011 | WEB-04 | Usar técnicos e supervisores autorizados no planejamento | P0 | API-GAP-007, WEB-T010 | `IN_PROGRESS` |
| WEB-T012 | WEB-05 | Completar clientes e drill-down para locais | P0 | WEB-T003 | `IN_PROGRESS` |
| WEB-T013 | WEB-06 | Completar locais no contexto do cliente | P0 | WEB-T012 | `IN_PROGRESS` |
| WEB-T014 | WEB-06 | Resolver UX/contrato da rota global de locais | P1 | API-GAP-001 | `BLOCKED` |
| WEB-T015 | WEB-07 | Completar equipamentos no contexto do local e QR | P0 | WEB-T013 | `IN_PROGRESS` |
| WEB-T016 | WEB-07 | Resolver UX/contrato da rota global de equipamentos | P1 | API-GAP-002 | `BLOCKED` |
| WEB-T017 | WEB-08 | Completar lista, criação, status e publicação de modelos | P0 | WEB-T003 | `IN_PROGRESS` |
| WEB-T018 | WEB-09 | Completar construtor de seções e itens | P0 | WEB-T017 | `IN_PROGRESS` |
| WEB-T019 | WEB-09 | Implementar ordenação acessível e estado de edição | P0 | WEB-T018 | `READY` |
| WEB-T020 | WEB-10 | Validar prévia fiel de rascunho/versão | P0 | WEB-T018 | `IN_PROGRESS` |
| WEB-T021 | WEB-11 | Completar histórico e detalhe de versões | P0 | WEB-T017 | `IN_PROGRESS` |
| WEB-T022 | WEB-12 | Completar lista, paginação e filtros suportados de inspeção | P0 | WEB-T003, WEB-T008 | `IN_PROGRESS` |
| WEB-T023 | WEB-12 | Resolver filtros de equipamento, atraso e revisão | P1 | API-GAP-003 | `BLOCKED` |
| WEB-T024 | WEB-13 | Completar planejamento com seleções em cascata | P0 | WEB-T011, WEB-T013, WEB-T015, WEB-T017 | `IN_PROGRESS` |
| WEB-T025 | WEB-14 | Completar detalhe com respostas, evidências, NC, revisões e histórico | P0 | WEB-T022 | `IN_PROGRESS` |
| WEB-T026 | WEB-14 | Implementar visualização/download autorizado de evidência | P0 | WEB-T025 | `READY` |
| WEB-T027 | WEB-14 | Implementar cancelamento justificado | P0 | WEB-T025 | `READY` |
| WEB-T028 | WEB-15 | Completar revisão por supervisor atribuído | P0 | WEB-T025, WEB-T026 | `IN_PROGRESS` |
| WEB-T029 | WEB-15 | Cobrir decisões e autorização adversarial | P0 | WEB-T028 | `READY` |
| WEB-T030 | WEB-16 | Completar NC no contexto de detalhe/revisão | P0 | WEB-T025 | `IN_PROGRESS` |
| WEB-T031 | WEB-16 | Implementar listagem global de NC | P1 | API-GAP-004 | `BLOCKED` |
| WEB-T032 | WEB-17 | Completar auditoria com filtros atualmente suportados | P0 | WEB-T003, WEB-T008 | `IN_PROGRESS` |
| WEB-T033 | WEB-17 | Implementar filtros avançados de auditoria | P1 | API-GAP-005 | `BLOCKED` |
| WEB-T034 | WEB-19 | Criar suíte de testes de serviços HTTP e interceptors | P0 | WEB-T004..033 conforme área | `READY` |
| WEB-T035 | WEB-19 | Criar suíte de testes de componentes e estados | P0 | WEB-T003..033 conforme área | `READY` |
| WEB-T036 | WEB-19 | Executar auditoria WCAG 2.1 AA e regressão responsiva | P0 | WEB-T001..035 | `BLOCKED` |
| WEB-T037 | WEB-19 | Executar jornada integrada completa contra API local | P0 | WEB-T004..035 | `BLOCKED` |
| WEB-T038 | WEB-19 | Configurar CI web com test, build e Prettier | P0 | WEB-T034, WEB-T035 | `READY` |
| WEB-T039 | WEB-19 | Publicar SPA em S3 privado + CloudFront | P1 | infraestrutura/ambiente | `BLOCKED` |
| WEB-T040 | WEB-19 | Executar smoke/E2E HTTPS no ambiente publicado | P1 | WEB-T039 | `BLOCKED` |

## Lacunas de API relacionadas

Estas lacunas são dependências, não autorização para o Frontend Agent alterar `API/`.

| ID | Necessidade | Decisão possível | Bloqueia |
| --- | --- | --- | --- |
| API-GAP-001 | listagem global de locais | criar `GET /sites` ou exigir cliente antes da lista | WEB-T014 |
| API-GAP-002 | listagem global de equipamentos | criar `GET /equipment` ou exigir local antes da lista | WEB-T016 |
| API-GAP-003 | filtros de inspeção por equipamento/atraso/revisão | ampliar `GET /inspections` ou remover filtros não contratuais | WEB-T023 |
| API-GAP-004 | listagem global de não conformidades | criar endpoint paginado autorizado ou retirar rota global do MVP | WEB-T031 |
| API-GAP-005 | auditoria por período/ação/inspeção/motivo | ampliar `GET /audit-events` | WEB-T033 |
| API-GAP-006 | busca textual de usuário | ampliar `GET /users` ou limitar UI a papel/status | melhoria de WEB-T010 |
| API-GAP-007 | seleção de técnicos e supervisores por SUPERVISOR | Resolvido para o MVP de organização única por `/users/assignable?role=...&status=...&page=...&size=...&sort=...`; escopo por cliente/local permanece fora do modelo atual | WEB-T011/024 e filtros de WEB-T022 |

O detalhamento interaplicações e a decisão D02 estão no [plano integrado do MVP](../mvp-integration-plan.md). A tela de planejamento usa a consulta operacional paginada de atribuíveis; a jornada integrada contra API real ainda é coberta por WEB-T037.

## Critérios por grupo

### Fundação e sessão — WEB-T001..009

- tema usa tokens e Nunito Sans sem overrides frágeis do Material;
- shell funciona nos três breakpoints e por teclado;
- login, refresh, logout, recuperação e troca usam API real;
- 401 encerra sessão de forma segura; 403 preserva aplicação e mostra falta de acesso;
- dashboard não fabrica valores em erro.

### Cadastros — WEB-T010..016

- paginação e filtros são server-side;
- formulários preservam dados em falha e exibem `fieldErrors`;
- relações cliente → local → equipamento são validadas;
- status inativo é respeitado no planejamento;
- rotas globais não chamam endpoints inexistentes.

### Modelos — WEB-T017..021

- rascunho, publicação e versão ficam visualmente distintos;
- seções/itens não atravessam modelos;
- tipos e regras condicionais aparecem na prévia;
- ordenação possui alternativa acessível;
- versão publicada permanece somente leitura.

### Inspeções e revisão — WEB-T022..031

- filtros suportados são enviados corretamente à API;
- criação respeita cascata e ativos;
- detalhe agrega dados sem ocultar falhas parciais;
- evidência é consultada por endpoint autorizado;
- somente supervisor atribuído revisa;
- reprovação exige motivo e decisões inválidas preservam contexto;
- inspeção aprovada/cancelada é somente leitura.

### Auditoria e qualidade — WEB-T032..040

- auditoria é somente leitura e não mostra informação sensível;
- cada serviço HTTP possui teste direto de método/URL/parâmetros/payload/erro;
- componentes críticos cobrem estados alternativos, papel e processamento;
- test, build e Prettier passam em execução limpa;
- integração real registra ambiente, papel, dados e resultado;
- WCAG e breakpoints têm evidência; scanner não substitui teclado/manual;
- deploy comprova HTTPS, refresh de rota, CORS, cache e rollback.

## Ordem recomendada

```text
1. WEB-T001..008  — fundação visual e sessão
2. WEB-T003       — padrões compartilhados
3. WEB-T010..021  — cadastros e modelos
4. WEB-T022..030  — inspeções e revisão
5. WEB-T009/032   — dashboard e auditoria consolidados
6. WEB-T034..037  — testes, acessibilidade e integração local
7. resolver API-GAPs P1 conforme decisão de produto
8. WEB-T038..040  — CI, publicação e smoke em ambiente
```

## Registro de execução

| ID | Data | Executor | Ação/evidência | Resultado | Próxima ação |
| --- | --- | --- | --- | --- | --- |
| — | — | — | — | — | — |

## Regras para agents

1. Ler [README](README.md), spec afetada e [ENDPOINTS](ENDPOINTS.md) antes de implementar.
2. Não iniciar task `BLOCKED` sem resolver e documentar a dependência.
3. O Frontend Agent atualiza para `VALIDATION` somente com `READY_FOR_WEB_QA` e gates registrados.
4. Somente um veredito do Web QA Agent autoriza `DONE`; o status é registrado após aprovação e criação do commit local.
5. Um endpoint marcado `API_GAP` não pode ser simulado no caminho de produção.
6. Preservar alterações locais alheias e limitar o commit aos arquivos validados.
7. Criar commit local após aprovação; nunca executar `git push`.

## Referências

- [Especificações web](WEB-SPECS.md)
- [Catálogo de endpoints](ENDPOINTS.md)
- [Workflow dos agentes web](../../04-development/web-agent-workflow.md)
- [Design System Industrial Web](../../03-architecture/WEB_DESIGN_SYSTEM.md)
