---
status: draft
validated_at: 2026-09-07
scope: static repository validation; no mobile code changes
---

# Validação de páginas e funcionalidades do FieldOps

## 1. Método

Inspeção estática realizada em 2026-09-07, comparando rotas/componentes Angular, rotas/store/repositórios/mocks do mobile, controllers da API e a Knowledge Base. Nenhum arquivo de `mobile/` foi alterado. A existência de uma rota ou componente é evidência de superfície, não aceite funcional integrado.

Legenda: **superfície presente** = rota/componente encontrado; **parcial/mock** = há tela, mas com simulação, estado local ou dependência não comprovada; **ausente/não comprovado** = não encontrado ou sem evidência suficiente; **aceite pendente** = requer API, banco, dispositivo ou teste.

## 2. Resultado

O painel web possui cobertura estrutural ampla do MVP: login, shell, dashboard, cadastros, templates, inspeções, revisão, NC e auditoria. O mobile possui a casca navegável de técnico, mas ainda não comprova o núcleo obrigatório offline-first: autenticação real, SQLite, outbox, recuperação após reinício, câmera, QR, localização e sincronização consumindo a API. A aplicação não pode ser declarada pronta para aceite de MVP apenas pela presença das telas.

## 3. Matriz do painel web

| ID | Página/rota | Evidência observada | Classificação |
|---|---|---|---|
| WEB-01 | `/login` | componente de login e rota | presente; integração pendente |
| WEB-02 | `/app` | shell, auth guard, role guard e interceptors | presente |
| WEB-03 | `/app/dashboard` | status, atrasadas, revisão, NC e atalhos | presente; escopo/teste pendente |
| WEB-04 | `/app/users` | tabela, filtros, diálogo, status e papéis | presente; ADMIN-only requer teste |
| WEB-05 | `/app/clients` | rota, listagem e serviço | presente; aceite de formulário pendente |
| WEB-06 | `/app/sites`, `/:clientId/sites` | rotas/listagens aninhadas | presente; vínculo pendente |
| WEB-07 | `/app/equipment`, `/:siteId/equipment` | tabela, filtros, QR, diálogo e status | presente; unicidade/compatibilidade pendente |
| WEB-08 | `/app/inspection-templates` | lista, criação, prévia, versões e status | presente |
| WEB-09 | `/:templateId/edit` | builder e formulário de seção | presente; validações cruzadas pendentes |
| WEB-10 | `/:templateId/preview` | componente de prévia | presente |
| WEB-11 | `/:templateId/versions` | histórico e estado vazio | presente |
| WEB-12 | `/app/inspections` | lista e filtros | presente; cobertura de escopo pendente |
| WEB-13 | `/app/inspections/new` | componente e atalho no dashboard | presente; integração pendente |
| WEB-14 | `/app/inspections/:id` | detalhe | presente |
| WEB-15 | `/:id/review` | componente de revisão | presente; transições/escopo pendentes |
| WEB-16 | `/app/non-conformities` | rota e lista | presente; regras de estado pendentes |
| WEB-17 | `/app/audit` | rota e lista | presente; política de leitura pendente |

## 4. Matriz do mobile

| ID | Página/fluxo | Evidência observada | Classificação |
|---|---|---|---|
| MOB-01 | `/` | redirect por usuário local | parcial; sessão real não comprovada |
| MOB-02 | `/(auth)/login` | login, loading e alerta; “dados mockados” | parcial/mock |
| MOB-03 | `/(tabs)/home` | home e cards | parcial; sync/conectividade não comprovados |
| MOB-04 | `/(tabs)/inspections` | busca e cards | parcial; filtros status não interativos |
| MOB-05 | `/(tabs)/history` | histórico filtrado | parcial; SQLite/retenção não comprovados |
| MOB-06 | `/(tabs)/profile` | perfil, texto de sync e logout | parcial; última sync estática |
| MOB-07 | `/notifications` | cards com dados mockados | parcial/mock |
| MOB-08 | `/inspections/:id` | detalhe, status, progresso e conflito local | parcial; API/escopo não comprovados |
| MOB-09 | `/inspections/:id/checklist` | opções e progresso em memória | parcial; persistência/outbox não comprovadas |
| MOB-10 | `/inspections/:id/evidence` | lista/observação e “Simular captura de foto” | parcial/mock; câmera/upload não comprovados |
| MOB-11 | QR | nenhum fluxo nativo identificado | ausente/não comprovado |
| MOB-12 | localização | `requiresLocation` em domínio/mock, sem integração identificada | ausente/não comprovado |
| MOB-13 | `/inspections/:id/review` | bloqueio de obrigatórios e conclusão local | parcial; envio real não comprovado |
| MOB-14 | sync/conflito | tipos/textos, sem central/outbox identificada | ausente/não comprovado |

## 5. Validação por persona

### ADMIN

**Permitido:** login, dashboard, usuários, clientes, locais, equipamentos, modelos e auditoria.  
**Proibido:** executar checklist, responder, anexar como executor, revisar, aprovar, reprovar ou alterar resposta para “corrigir”.  
**Resultado:** telas administrativas encontradas; 403/escopo e ausência de execução devem ser comprovados por testes. Não elevar ADMIN a executor apenas porque há rotas de inspeção.

### SUPERVISOR

**Permitido:** modelos, planejamento, acompanhamento e revisão de inspeções do seu escopo; aprovação/reprovação justificada.  
**Proibido:** executar como técnico, revisar fora do escopo, decidir sem vínculo ou alterar aprovada.  
**Resultado:** rotas existem; formulário de planejamento, vínculo, escopo e transições requerem integração.

### TECHNICIAN

**Permitido:** ver atribuições próprias, baixar, iniciar, responder, fotografar, registrar NC/QR/localização, concluir offline, sincronizar e corrigir reprovação reaberta.  
**Resultado:** telas existem, mas store/repositório/mocks e ações simuladas não comprovam SQLite, outbox, câmera, QR, localização ou sync real. Veredito: UX parcial/mock, fluxo de produção não validado.

### CLIENT_VIEWER

É FUTURE. O papel aparece no cadastro, mas não existe portal/tela de cliente no escopo atual.

## 6. Casos obrigatórios de sincronização

| Caso | Aceite |
|---|---|
| download | inspeção, snapshot e dados autorizados persistem localmente |
| perda de rede | resposta/evidência permanecem e entram na outbox |
| reinício | dados e outbox são recuperados |
| retry | operação idempotente não duplica |
| pull | cursor avança somente após persistência |
| push parcial | sucesso/falha por operação |
| conflito | valor local preservado e conflito acionável |
| aprovação remota tardia | alteração local não sobrescreve aprovação |
| rede recuperada | retry automático sem descarte |
| logout | tokens removidos e pendências seguem política segura |

O backend possui `SyncController` e classes de sync no inventário, mas isso não comprova que o mobile atual consome o contrato.

## 7. Gaps críticos

1. Autenticação mobile real e remoção do mock de demonstração.
2. SQLite para inspeções, respostas, evidências e outbox.
3. Reidratação após reinício e retenção.
4. Câmera real, permissões e limites de arquivo.
5. QR/equipamento e tratamento de divergência.
6. Localização pontual e permissão negada.
7. Push/pull, idempotência, cursor, retry e conflito consumidos pelo mobile.
8. Filtros de status interativos.
9. Notificações reais, lida e deep link.
10. Testes integrado por persona, incluindo acessos proibidos.
11. Build e execução em Android 10/API 29+.
12. Fluxo de correção após reprovação com histórico.

## 8. Evidências necessárias antes do aceite

- `npm run build` e testes do painel;
- `mvn test` com autorização, workflow, evidências e sync;
- build instalável Android;
- roteiro em dispositivo/emulador com perda/retorno de rede;
- banco local com reinício e outbox;
- upload real JPEG/PNG/PDF dentro/fora dos limites;
- matriz permitido/proibido por endpoint e tela;
- logs sem token/senha/conteúdo sensível;
- estados vazio, erro, offline, sync e conflito validados visualmente.

## 9. Veredito

Há base web suficiente para validação funcional incremental e uma superfície mobile navegável. O mobile deve permanecer classificado como parcialmente implementado até comprovar o fluxo offline-first e as capacidades nativas. Nenhum arquivo de `mobile/` foi alterado.

