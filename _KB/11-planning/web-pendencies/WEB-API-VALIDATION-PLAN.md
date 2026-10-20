# Plano de validação web/API

**Escopo:** validação das integrações do painel administrativo com a API no MVP.  
**Estado:** plano baseado em inspeção estática dos clones atuais; comandos e cenários ainda precisam ser executados e registrados.  
**Bloqueios para aceite integrado:** confirmar resolução de `ARC-027` e `IVH-001` com seus responsáveis.

## Evidência existente no snapshot revisado

| Superfície | Evidência observada | Limite da evidência |
|---|---|---|
| API | Testes automatizados em `AuthenticationTest`, `InspectionWorkflowTest` e `TemplateVersioningTest`; cobrem autenticação, ciclo de vida/revisão e regras de modelo. | Não demonstram que os fluxos web consumam corretamente a API nem cobrem todas as regras de autorização, listagens e evidências. |
| Web | Testes em `app.spec.ts`, `shell.component.spec.ts`, `ui-state.spec.ts` e `status-badge.component.spec.ts`. | Cobrem criação do app, shell e componentes compartilhados; não constituem suíte de serviços HTTP ou de integração das telas de negócio. |
| Mobile | `package.json` define `typecheck`, sem script de teste automatizado. | Typecheck não executa nem comprova comportamento offline/sync. |
| Contrato de sincronização | DEC-013 aprova `POST /api/v1/mobile/sync/push` e `GET /api/v1/mobile/sync/pull`; não foram encontrados controllers dessas rotas no snapshot API revisado. | É bloqueio de integração móvel; não criar mock ou inferir sucesso do fluxo web para cobrir a lacuna. |

Esta tabela é inventário estático, não resultado de execução. Revisar os caminhos e o estado dos repositórios antes de iniciar cada rodada.

## Sequência e critérios

### 1. Verificações locais

Executar de cada clone e registrar versão/commit e saída:

| Repositório | Comando | Critério |
|---|---|---|
| `API/` | `mvn test` | Todos os testes passam; falhas, testes ignorados e migrações são registrados, sem assumir que cobertura existente equivale a aceite. |
| `interface-administrativa/` | `npm test` | Todos os testes existentes passam; registrar resultado do runner e falhas. |
| `interface-administrativa/` | `npm run build` | Build de produção termina sem erro. |
| `aplicativo-mobile/` | `npm run typecheck` | TypeScript termina sem erros; isso não substitui teste funcional. |

### 2. Contrato e autorização

Executar contra ambiente local identificado, com usuários ADMIN, SUPERVISOR, técnico atribuído, técnico não atribuído e supervisor não atribuído:

1. autenticar e consultar sessão; verificar token inválido/expirado e usuário inativo;
2. consultar cadastros/listagens usados por cada papel; confirmar método, URL, parâmetros, paginação e estados de erro;
3. planejar inspeção válida e rejeitar usuário inativo ou papel/vínculo inválido;
4. executar o fluxo permitido e tentar as mesmas operações com usuário fora do vínculo; esperar 403 sem vazamento de dados;
5. enviar decisão de revisão válida/inválida e confirmar motivo obrigatório na reprovação;
6. consultar evidência somente no escopo autorizado; exercitar formatos e limites aprovados;
7. verificar que erros preservam formulário/contexto e que nova tentativa não duplica mutação.

Para toda expectativa que divergir do contrato vigente, abrir ou vincular um `API_GAP`; não adicionar endpoint apenas para satisfazer a UI.

### 3. Integração web/API

Executar os fluxos de login, cadastros contextuais, criação de modelo, planejamento, detalhe e revisão na aplicação web, conectada à API local (não a mocks):

- capturar requisição/resposta sanitizada e resultado visível para loading, vazio, erro, sucesso e 401/403;
- comprovar que filtros e paginação correspondem aos parâmetros realmente suportados;
- comprovar que upload/visualização de evidência usa o contrato autorizado e que falha de rede mantém o contexto;
- testar ADMIN e SUPERVISOR separadamente, inclusive seleção de atribuíveis no planejamento;
- comparar estado final da UI com API e registros de auditoria.

Uma página renderizada ou uma captura de tela não comprova a interação completa.

## Matriz de execução

| ID | Cenário | Automatizado/manual | Resultado | Ambiente/commit | Evidência ou bloqueio |
|---|---|---|---|---|---|
| V-01 | Suítes locais API, web e typecheck mobile | Automatizado | `NOT RUN` | | |
| V-02 | Login e sessão por papel | Automatizado + manual | `NOT RUN` | | |
| V-03 | Cadastros/listagens e paginação consumidos pela web | API + integração | `NOT RUN` | | |
| V-04 | Planejamento e validação de responsáveis | API + integração | `NOT RUN` | | |
| V-05 | Detalhe, respostas, evidências e estados de falha | API + integração | `NOT RUN` | | |
| V-06 | Revisão, motivo e bloqueio por vínculo | API + integração | `NOT RUN` | | |
| V-07 | Regressão web teclado/responsividade/acessibilidade | Manual | `NOT RUN` | | |

Valores permitidos: `PASS`, `FAIL`, `BLOCKED`, `NOT RUN`. Marcar `PASS` apenas quando passos, resultado e evidência estiverem registrados; bugs não resolvidos mantêm o cenário como `FAIL` ou `BLOCKED`.

## Referências

- [Backlog web — WEB-T034..037](BACKLOG.md)
- [Catálogo de endpoints web](ENDPOINTS.md)
- [Plano integrado e critérios ponta a ponta](../mvp-integration-plan.md)
- [Checklist de auditoria WCAG](WEB-ACCESSIBILITY-AUDIT.md)
- [Requisitos não funcionais](../../01-product/requirements-non-functional.md)
