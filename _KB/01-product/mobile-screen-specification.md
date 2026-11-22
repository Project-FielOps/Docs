# Especificação funcional do FieldOps Mobile

**Produto:** FieldOps Mobile  
**Plataforma primária:** Android 10 / API 29 ou superior  
**Tecnologia prevista:** Expo SDK 57 / React Native  
**Usuário principal:** TECHNICIAN — técnico de campo  
**Responsabilidade:** Produto e Projeto (P.O./P.M.)  
**Data:** 2026-09-07  
**Status:** especificação do MVP; itens P1 e FUTURE estão explicitamente separados

## 1. Finalidade

O FieldOps Mobile é a ferramenta operacional do técnico em campo. Ele deve permitir que o técnico consulte apenas inspeções atribuídas a si, prepare o trabalho, execute um checklist dinâmico, registre respostas e evidências, trabalhe sem conexão, retome o trabalho após reinício, sincronize com segurança e acompanhe o resultado da revisão.

O mobile não é o painel administrativo. Não deve conter criação de usuários, gestão de clientes, construção de modelos, planejamento de inspeções ou aprovação/reprovação. Essas responsabilidades permanecem no painel web e na API.

O objetivo de aceite é um fluxo vertical completo:

`login → sincronização → fila atribuída → download → execução offline → respostas/evidências → revisão local → outbox → sincronização → revisão do supervisor → correção ou aprovação`.

## 2. Fontes normativas

Esta especificação complementa e não substitui:

- [Especificação geral de telas e funcionalidades](application-scope-and-screen-specification.md);
- [Escopo do MVP](product-mvp.md);
- [Papéis e permissões](user-roles.md);
- [Regras de negócio](product-business-rules.md);
- [Fluxos principais](user-flows.md);
- [Arquitetura mobile](../03-architecture/mobile.md);
- [Design mobile](../03-architecture/mobile-design.md);
- [Contrato de sincronização](../05-api/specs/synchronization-contract.md);
- [Especificações de inspeção](../05-api/specs/inspections.md);
- [Validação atual do aplicativo](../99-reference/validation/application-surface-validation-2026-09-07.md).

Precedência: decisões aprovadas, KB consolidada, código como evidência AS-IS e referências históricas. A tela nunca deve conceder uma operação que a API proíba.

## 3. Escopo do mobile

### 3.1 P0 — obrigatório para o MVP

- inicialização e sessão segura;
- login real do técnico;
- logout e expiração de sessão;
- Home operacional;
- lista de inspeções atribuídas;
- busca, filtros e estados de conectividade;
- histórico do técnico;
- perfil e diagnóstico de sincronização;
- notificações locais derivadas de eventos relevantes;
- detalhe da inspeção;
- download prévio de inspeção e snapshot;
- checklist dinâmico por versão/snapshot;
- persistência local imediata de respostas;
- observações e não conformidades;
- captura e armazenamento local de evidências;
- confirmação de equipamento por QR quando exigida;
- localização pontual quando exigida e autorizada;
- revisão antes da conclusão;
- conclusão online ou local offline;
- SQLite operacional;
- outbox com dependências, tentativa, idempotência e erro;
- push/pull por cursor;
- retry automático e manual;
- conflitos explícitos sem descarte de dados;
- retomada após reinício;
- correção após reprovação mediante reabertura explícita;
- proteção de inspeção aprovada/cancelada;
- acessibilidade, estados vazios, erro, loading e feedback;
- build e execução Android.

### 3.2 P1 — depois do fluxo vertical

- push remoto completo;
- biometria;
- assinatura desenhada;
- PDF básico;
- comentários por item;
- histórico detalhado de cada resposta;
- suporte assistido à resolução de conflitos;
- tema escuro;
- recursos avançados de notificação.

### 3.3 FUTURE — fora do MVP

- execução pelo cliente;
- portal do cliente;
- multiempresa;
- geofencing contínuo;
- roteirização;
- áudio/vídeo;
- OCR e IA;
- IoT;
- integração ERP;
- ordens de serviço e manutenção preventiva;
- planos de ação completos;
- assinatura avançada/regulatória.

## 4. Personas e limites de autorização

### 4.1 Técnico atribuído

É o único usuário que executa a inspeção no mobile. Pode ler e alterar somente inspeções atribuídas ao seu usuário, enquanto o estado permitir. Pode iniciar, responder, adicionar evidências, registrar NC, confirmar equipamento, capturar localização, concluir, sincronizar e corrigir uma reprovação reaberta.

### 4.2 Técnico não atribuído

Não deve localizar, baixar, abrir integralmente, responder ou anexar evidência em inspeção de outro técnico. A API deve impor essa regra mesmo que um identificador seja digitado ou que um registro apareça em cache indevido.

### 4.3 Supervisor

Não usa o mobile para executar como técnico. A revisão, aprovação, reprovação e acompanhamento de escopo são feitos no painel web. Um usuário supervisor autenticado no app deve ser bloqueado ou direcionado para uma experiência não prevista, nunca receber ações de execução por acidente.

### 4.4 ADMIN

Não executa nem decide inspeções no mobile. O app deve rejeitar qualquer tentativa de usar o aplicativo como atalho administrativo.

### 4.5 Cliente visualizador

FUTURE. Não criar navegação de cliente no MVP.

## 5. Princípios de experiência

1. **Ação primeiro:** cada tela responde qual é a próxima ação do técnico.
2. **Estado verdadeiro:** estado oficial da inspeção e estado local de sync aparecem separados.
3. **Offline explícito:** “salvo neste dispositivo” nunca é confundido com “enviado ao servidor”.
4. **Uma mão:** ações primárias largas e próximas da parte inferior.
5. **Baixa carga cognitiva:** uma decisão principal por bloco.
6. **Campo real:** contraste, legibilidade, tolerância a toque e feedback curto.
7. **Não perder trabalho:** respostas, arquivos e operações pendentes nunca desaparecem silenciosamente.
8. **Sem dependência de cor:** status também usa texto, ícone, forma ou rótulo.

## 6. Navegação principal

```text
Inicialização
├── /(auth)/login
└── /(tabs)
    ├── home
    ├── inspections
    ├── history
    └── profile

Acessos transversais
├── /notifications
└── /sync (pode ser modal, bottom sheet ou tela dedicada)

Execução
└── /inspections/:id
    ├── index — detalhe
    ├── checklist
    ├── evidence — evidências e observações
    ├── equipment — QR/confirmação, quando aplicável
    ├── location — localização, quando aplicável
    └── review — revisão antes do envio
```

O retorno de cada etapa deve preservar contexto e alterações locais. Pressionar voltar não pode descartar mudanças sem confirmação.

## 7. Contrato visual e componentes

### 7.1 Componentes obrigatórios

- `Button` e `IconButton`;
- `Field`/`SearchField`;
- `Card` e `InspectionCard`;
- `Badge`, `StatusBadge` e `PriorityBadge`;
- `Progress`/barra de progresso;
- `SyncIndicator`;
- `EmptyState`;
- `Loading/Skeleton`;
- `ErrorState` com retry;
- `Toast`/feedback breve;
- `BottomSheet` de confirmação;
- `ChecklistItem`;
- `EvidenceCard` com thumbnail e status;
- `ConflictCard`;
- `Scout` apenas em estados contextuais.

### 7.2 Regras visuais

- touch target mínimo de 44 × 44 pontos;
- corpo preferencial de 15 px ou maior;
- contraste compatível com WCAG 2.1 AA quando possível;
- nenhuma ação crítica apenas por ícone;
- botões destrutivos/irreversíveis com label e confirmação;
- loading não deve permitir duplo envio;
- erros devem ficar próximos do campo/ação que falhou;
- texto de status deve informar ação: “Tentar novamente”, “Revisar conflito”, “Aguardando conexão”.

## 8. Estados globais do aplicativo

### 8.1 Sessão

| Estado | Comportamento |
|---|---|
| sem sessão | direcionar ao login |
| autenticando | bloquear duplo envio e mostrar loading |
| autenticado | carregar perfil e dados autorizados |
| sessão expirada | preservar mensagem, remover credenciais e retornar ao login |
| logout | remover tokens locais e impedir acesso protegido |
| usuário inativo | rejeitar login e não baixar dados |

### 8.2 Conectividade

| Estado | Mensagem | Ação |
|---|---|---|
| online | Online | operar e sincronizar |
| offline | Sem conexão — alterações ficam no dispositivo | continuar trabalho permitido |
| reconectando | Recuperando conexão | aguardar ou continuar navegando |
| sincronizando | Sincronizando alterações… | não interromper sem confirmação |
| erro | Não foi possível sincronizar agora | tentar novamente |
| conflito | Conflito para revisar | abrir contexto, não sobrescrever |

### 8.3 Estado oficial × estado local

Exemplo obrigatório para conclusão offline:

```text
workflowStatus = IN_PROGRESS
syncStatus = pending
locallyCompletedAt = <data local>
```

Somente a confirmação do servidor pode mudar o estado oficial para `SUBMITTED`/`UNDER_REVIEW` ou outro estado previsto pelo contrato.

## 9. Especificação das telas

### MOB-01 — Inicialização

**Rota:** `/`.  
**Objetivo:** carregar configuração, fonte, armazenamento seguro e sessão sem exibir uma tela incompleta.

Comportamento:

1. carregar fontes e tema;
2. verificar sessão Secure Store;
3. validar expiração/refresh;
4. direcionar ao login ou Home;
5. se houver migração local, executá-la antes de disponibilizar dados;
6. se houver outbox pendente, preservá-la e exibir aviso após a entrada.

Critérios de aceite:

- reiniciar online mantém sessão válida quando permitido;
- reiniciar offline permite acesso aos dados locais autorizados, sem inventar sessão nova;
- migração interrompida é retomável;
- erro de inicialização tem mensagem e suporte a retry.

### MOB-02 — Login

**Rota:** `/(auth)/login`.  
**Atores:** usuário ativo autorizado; no MVP, técnico.

Elementos:

- marca FieldOps;
- campo e-mail com teclado apropriado;
- campo senha com ocultação;
- Entrar;
- loading no botão;
- mensagem de credencial inválida;
- mensagem de usuário inativo/bloqueado sem enumeração indevida;
- recuperação de senha conforme contrato quando o fluxo mobile for exposto;
- estado de indisponibilidade da API.

Critérios:

1. autenticar pela API real;
2. salvar tokens somente em Secure Store;
3. carregar identidade e papel;
4. não guardar senha;
5. não permitir duplo envio;
6. não usar e-mail/senha de demonstração em produção;
7. limpar sessão ao logout/expiração;
8. informar quando a rede é necessária para primeiro login.

**Validação AS-IS:** a tela atual possui loading e alerta, mas indica “Ambiente de demonstração · dados mockados”. Classificação: parcial/mock.

### MOB-03 — Home

**Rota:** `/(tabs)/home`.  
**Objetivo:** orientar o técnico sobre a próxima ação.

Conteúdo:

- saudação e identidade curta;
- próxima inspeção, código, local e horário;
- contadores de pendentes, em andamento, aguardando envio e conflitos;
- progresso do dia;
- conectividade;
- última sincronização real;
- botão Sincronizar agora;
- acesso a notificações;
- estado vazio quando não há atribuições;
- alerta para operação pendente/conflito.

Critérios:

- todos os números vêm do armazenamento sincronizado;
- toque em cartão abre o detalhe correto;
- conflito nunca aparece como concluído;
- retorno da rede inicia sync automático;
- a Home funciona offline com dados locais;
- erro da API não transforma a Home em tela vazia sem explicação.

### MOB-04 — Lista de inspeções

**Rota:** `/(tabs)/inspections`.  
**Objetivo:** permitir encontrar rapidamente uma inspeção atribuída.

Busca por título, local ou código. Filtros interativos:

- todas;
- pendentes/atribuídas;
- em andamento;
- aguardando envio;
- em revisão;
- rejeitadas/correção;
- aprovadas;
- conflito/erro de sync.

Card mínimo:

- código;
- título/modelo;
- local/endereço;
- data/horário;
- prioridade;
- status oficial;
- progresso;
- quantidade de evidências;
- indicador de sync;
- conflito ou pendência acionável.

Critérios:

- dados limitados ao técnico autenticado;
- busca não altera escopo;
- filtros realmente modificam a lista;
- pull-to-refresh não apaga dados locais;
- cada estado possui loading, vazio, erro e offline;
- inspeção fora da atribuição não abre integralmente.

**Validação AS-IS:** busca e cards existem; os rótulos de filtros aparecem, mas os filtros de status não estão implementados como interação no código observado. Dados são fornecidos pelo store/mock.

### MOB-05 — Histórico

**Rota:** `/(tabs)/history`.  
**Objetivo:** consultar inspeções anteriores e correções.

Mostrar inspeções `SUBMITTED`, `UNDER_REVIEW`, `APPROVED`, `REJECTED` e `CANCELED`, além de correções/reenvios quando disponíveis. Cada item deve exibir decisão, motivo de reprovação, última atualização, evidências e estado de sync.

Regras:

- histórico é filtrado pelo técnico autenticado;
- inspeções concluídas e sincronizadas podem ser retidas por 30 dias conforme política;
- pendentes, erros, conflitos e evidências não confirmadas nunca são limpos automaticamente;
- histórico anterior à correção permanece visível ou acessível.

**Validação AS-IS:** tela e filtro por status existem; retenção e SQLite não estão comprovados.

### MOB-06 — Perfil e sessão

**Rota:** `/(tabs)/profile`.

Exibir nome, avatar, papel, código do colaborador, versão do app, ambiente, última sincronização, quantidade de operações pendentes e estado da sessão. Ações: Sincronizar agora, abrir diagnóstico e Sair da conta.

Logout:

- confirmar se houver operações pendentes;
- remover tokens;
- preservar ou remover dados locais conforme política de segurança aprovada;
- não deixar tela protegida acessível pelo voltar.

**Validação AS-IS:** perfil e logout existem; “Última sincronização: Agora” é valor estático no código observado.

### MOB-07 — Notificações

**Rota:** `/notifications`.

Tipos mínimos: nova atribuição, sincronização concluída/falha, lembrete, prioridade, reprovação para correção e aprovação. Cada notificação contém título, mensagem, data, lida/não lida e destino.

Critérios:

- marcar lida persiste localmente e sincroniza quando necessário;
- toque leva à inspeção relacionada;
- notificações não expõem conteúdo sensível em tela bloqueada sem política;
- notificação de atribuição não implica download concluído;
- ausência de notificações tem estado vazio.

**Validação AS-IS:** lista visual existe com dados mockados; leitura, persistência e deep links não comprovados. Push completo é P1.

### MOB-08 — Detalhe da inspeção

**Rota:** `/inspections/:id`.

Seções:

- código, título e status;
- prioridade, data prevista e prazo;
- cliente, local e endereço;
- equipamento e QR esperado, quando aplicável;
- descrição/instruções;
- progresso e itens respondidos;
- evidências;
- estado de sync;
- conflito/erro;
- ações por estado.

Matriz de ações:

| Status | Ações |
|---|---|
| `ASSIGNED` | iniciar, baixar/atualizar dados |
| `IN_PROGRESS` | continuar checklist, evidências, QR/localização, revisar |
| `SUBMITTED` | leitura, sincronização/estado |
| `UNDER_REVIEW` | leitura; aguardar decisão |
| `REJECTED` | ver motivo, reabrir correção explicitamente |
| `APPROVED` | leitura; nenhuma mutação comum |
| `CANCELED` | leitura; nenhuma execução |

Critérios:

- não mostrar ação incompatível;
- aprovação/cancelamento bloqueiam mutação local indevida;
- conflito abre fluxo de conflito sem sobrescrever;
- detalhe offline mostra última versão local e sua data;
- tentativa de abrir ID fora do escopo falha com segurança.

**Validação AS-IS:** status, progresso, conflito e ações locais existem; integração e autorização não comprovadas.

### MOB-09 — Download e preparação

Pode ser bottom sheet no detalhe ou ação da Home. Deve baixar a inspeção autorizada, versão publicada e snapshot, instruções, itens, vínculos necessários e políticas de evidência/QR/localização.

Estados:

- pronto para baixar;
- baixando com progresso;
- disponível offline;
- atualização disponível;
- falha parcial com retry;
- conflito de versão.

Critérios:

- download é idempotente;
- só dados do técnico entram no dispositivo;
- inspeção não fica “offline pronta” antes de persistir o snapshot;
- falha não corrompe uma versão já disponível;
- o usuário sabe quanto falta e pode continuar navegando quando seguro.

### MOB-10 — Checklist

**Rota:** `/inspections/:id/checklist`.

Estrutura:

- cabeçalho com código, seção e progresso;
- agrupamento por seção;
- título/instrução do item;
- badge Obrigatório;
- opções compatíveis com `responseType`;
- campo de observação quando exigido;
- indicação de evidência obrigatória;
- estado salvo localmente/sincronizado;
- navegação entre itens/seções;
- ação Evidências e observações;
- ação Revisar envio.

Tipos essenciais do MVP devem ser fechados pelo contrato e podem incluir conformidade, booleano, texto, número, seleção e data. O app não deve renderizar tipos não suportados para publicação.

Ao responder:

1. atualizar visualmente o item;
2. persistir no SQLite;
3. criar/atualizar operação idempotente na outbox;
4. validar regra local simples;
5. tentar sincronizar sem bloquear o trabalho;
6. distinguir “salvo no dispositivo” de “confirmado pelo servidor”.

Regras:

- item obrigatório ausente bloqueia conclusão;
- não conforme pode exigir observação e/ou evidência;
- N/A só aparece quando permitido pelo modelo;
- resposta de inspeção aprovada/cancelada não pode ser alterada;
- resposta tardia após mudança remota vira conflito explícito.

**Validação AS-IS:** itens, opções e progresso existem, mas a resposta é alterada em estado de tela; persistência/outbox não estão comprovadas.

### MOB-11 — Observação e não conformidade

Pode existir dentro do item ou em tela vinculada. Deve permitir descrição, severidade quando suportada, item de origem, estado, evidência requerida e confirmação.

Critérios:

- NC crítica exige descrição e evidência quando a regra se aplicar;
- NC sempre pertence à inspeção e, quando aplicável, ao item;
- usuário fora da atribuição não cria/edita;
- inspeção aprovada/cancelada bloqueia alteração;
- criação entra na outbox e é auditável;
- erro de validação identifica o item/NC sem apagar texto.

### MOB-12 — Evidências e observações

**Rota:** `/inspections/:id/evidence`.

Funções:

- abrir câmera;
- capturar foto;
- selecionar arquivo quando permitido;
- mostrar thumbnail/nome/tamanho/tipo;
- vincular a inspeção, item ou NC;
- adicionar descrição/observação;
- remover antes do envio, se permitido;
- mostrar pendente, enviando, sincronizada, erro ou conflito.

Políticas:

- JPEG/JPG, PNG e PDF;
- máximo de 10 MB por arquivo;
- máximo de 10 arquivos por inspeção;
- validar assinatura binária, não apenas extensão;
- salvar arquivo local antes do upload;
- nunca apagar pendência em falha;
- arquivo associado a inspeção aprovada/cancelada fica protegido;
- compensar arquivo local se a operação de banco falhar.

**Validação AS-IS:** a tela possui lista, observação e “Simular captura de foto”; câmera, permissões, persistência e upload real não comprovados.

### MOB-13 — Confirmação de equipamento por QR

**Rota sugerida:** `/inspections/:id/equipment`, modal ou etapa dentro do detalhe/checklist.

Fluxo:

1. explicar por que o QR é solicitado;
2. pedir permissão de câmera no momento de uso;
3. abrir leitor;
4. ler código;
5. consultar cache/API conforme conectividade;
6. validar equipamento, local, situação e QR esperado;
7. mostrar confirmado, divergente, não encontrado, inativo ou indisponível;
8. registrar confirmação com data/dispositivo/local quando exigido;
9. colocar evento na outbox.

Política para QR divergente deve ser definida por configuração da inspeção. O app nunca deve aceitar silenciosamente equipamento de outro local.

**Validação AS-IS:** nenhum fluxo nativo QR foi identificado no inventário atual.

### MOB-14 — Localização pontual

**Rota sugerida:** `/inspections/:id/location`, modal ou etapa contextual.

Solicitar permissão somente quando necessário e explicar finalidade. Estados: aguardando permissão, capturando, capturada, negada, indisponível, baixa precisão, pendente e sincronizada.

Dados mínimos: latitude, longitude, precisão, data/hora, fonte e inspeção. A localização deve ser pontual, não rastreamento contínuo no MVP.

Regras:

- se `requiresLocation = false`, não pedir permissão;
- se obrigatória e negada, bloquear somente o ponto definido pela política da inspeção;
- nunca fingir localização;
- preservar localização pendente;
- proteger informação de localização na revisão e auditoria.

**Validação AS-IS:** `requiresLocation` aparece no domínio/mock, mas integração nativa não foi encontrada.

### MOB-15 — Revisão antes da conclusão

**Rota:** `/inspections/:id/review`.

Mostrar:

- progresso total;
- itens obrigatórios pendentes;
- não conformidades;
- observações;
- evidências e status de upload;
- equipamento/QR;
- localização;
- conexão;
- outbox pendente;
- conflito;
- aviso de conclusão offline.

Fluxo:

- pendência obrigatória: bloquear e navegar ao item;
- evidência obrigatória ausente: bloquear;
- online: enviar e aguardar confirmação;
- offline: concluir localmente, marcar pendente e voltar ao detalhe;
- erro: manter dados e oferecer retry;
- conflito: exigir revisão antes de concluir.

**Validação AS-IS:** tela, bloqueio de obrigatórios, resumo e mensagem offline existem; conclusão altera store local e não comprova submissão real.

### MOB-16 — Central de sincronização

Pode ser tela, bottom sheet ou seção de Perfil/Home, mas deve ser acessível sempre que houver pendência, erro ou conflito.

Exibir por operação:

- tipo: resposta, evidência, NC, conclusão, confirmação QR/localização;
- inspeção/item relacionado;
- estado;
- tentativa mais recente;
- próxima tentativa;
- erro legível;
- dependências;
- ação tentar novamente;
- ação abrir contexto;
- identificador técnico não sensível.

Não permitir limpar manualmente uma operação sem política de descarte e confirmação. Não esconder falha atrás de “última sincronização: agora”.

**Validação AS-IS:** tipos/textos de status aparecem no domínio/UI, mas central, SQLite e outbox não foram identificados.

### MOB-17 — Conflito

Exibir conflito quando uma mudança local não puder ser aplicada por alteração remota, inspeção aprovada, versão diferente ou transição incompatível.

MVP:

- preservar valor local;
- exibir estado remoto conhecido;
- informar motivo;
- impedir sobrescrita automática;
- permitir descartar/retornar somente mediante confirmação e política;
- registrar decisão;
- encaminhar para suporte/revisão quando não houver resolução automática.

## 10. Modelo operacional offline-first

### 10.1 Dados persistidos localmente

- sessão/identidade mínima segura;
- inspeções atribuídas baixadas;
- snapshot da versão;
- respostas;
- observações;
- NCs;
- metadados e arquivos de evidência;
- confirmações QR;
- localização pontual;
- notificações;
- outbox;
- cursor de pull;
- conflitos e erros.

Tokens devem usar Secure Store; dados de trabalho usam SQLite; arquivos devem ter armazenamento protegido e referência transacional.

### 10.2 Outbox

Cada operação deve conter:

- `operationId` idempotente;
- tipo;
- inspeção/recurso relacionado;
- payload;
- dependências;
- estado `PENDING`, `IN_FLIGHT`, `SUCCEEDED`, `FAILED`, `CONFLICT`;
- contagem de tentativas;
- última tentativa;
- próximo retry;
- erro sanitizado;
- versão/baseVersion quando aplicável.

Ordem recomendada: download/base → resposta → evidência/NC → conclusão → mudança de workflow. Uma falha não pode apagar operações independentes.

### 10.3 Push

1. selecionar operações prontas;
2. respeitar dependências;
3. enviar idempotency key;
4. interpretar sucesso, validação, 401/403, conflito e erro transitório;
5. persistir resultado;
6. marcar cada operação individualmente;
7. atualizar estado oficial somente após confirmação;
8. manter o restante da fila.

### 10.4 Pull

1. enviar cursor atual;
2. receber mudanças autorizadas;
3. aplicar transação local;
4. somente após commit avançar cursor;
5. detectar conflito com alterações locais;
6. atualizar Home, lista e notificações.

### 10.5 Retry

Erro transitório usa backoff. Erro de autorização, validação ou conflito não deve ser repetido infinitamente; deve virar estado acionável. Retry manual deve ser seguro e idempotente.

## 11. Contratos de erro no mobile

O app deve interpretar o contrato canônico:

```json
{
  "status": 422,
  "code": "INSPECTION_REQUIRED_ITEMS_MISSING",
  "message": "A inspeção possui itens obrigatórios sem resposta.",
  "requestId": "req-123",
  "fieldErrors": []
}
```

Mapeamento mínimo:

- 401: sessão expirada, tentar refresh/login;
- 403: sem permissão/vínculo, não repetir silenciosamente;
- 404: inspeção não disponível ao usuário ou removida do escopo;
- 409: conflito de estado/versão/idempotência;
- 413/415: arquivo acima do limite/formato inválido;
- 422: regra de negócio, apontar campo/item;
- 5xx/offline: conservar dados e retry.

## 12. Critérios de aceite por fluxo

### AC-MOBILE-01 — Login e sessão

Usuário ativo entra, token é protegido, identidade é carregada, logout remove sessão e usuário inativo falha. Reinício offline não cria autenticação falsa.

### AC-MOBILE-02 — Escopo

Técnico A não vê nem abre integralmente inspeção do Técnico B; manipular ID, cache ou payload não contorna a API.

### AC-MOBILE-03 — Download

Inspeção atribuída e snapshot ficam disponíveis offline; inspeção não atribuída não baixa; falha parcial é recuperável.

### AC-MOBILE-04 — Checklist

Cada resposta é persistida antes da rede, recuperada após reinício, enviada idempotentemente e vinculada ao item correto.

### AC-MOBILE-05 — Evidência

Foto real é salva localmente, validada por formato/tamanho/quantidade, exibida com status e enviada sem duplicidade.

### AC-MOBILE-06 — QR/localização

Permissões são solicitadas no contexto; equipamento/localização são validados; divergência/negação são visíveis; nenhum dado é inventado.

### AC-MOBILE-07 — Conclusão

Itens obrigatórios e evidências exigidas bloqueiam; offline conclui localmente sem declarar envio; online confirma pelo backend.

### AC-MOBILE-08 — Sincronização

Push/pull, retry, cursor, dependências e conflito funcionam após queda de rede, reinício e repetição.

### AC-MOBILE-09 — Correção

Reprovação aparece com motivo; reabertura é explícita; correção preserva histórico e novo envio mantém vínculo com a inspeção.

### AC-MOBILE-10 — Imutabilidade

Inspeção aprovada/cancelada é somente leitura; respostas, evidências e NC dependentes não sofrem alteração comum.

## 13. Matriz de validação de telas atuais

| Tela/fluxo | Local atual | Resultado da inspeção |
|---|---|---|
| Entrada | `mobile/app/index.tsx` | existe redirect por usuário local; sessão real não comprovada |
| Login | `mobile/app/(auth)/login.tsx` | existe, mas indica mock/demonstração |
| Home | `mobile/app/(tabs)/home.tsx` | existe; sync/conectividade real não comprovados |
| Inspeções | `mobile/app/(tabs)/inspections.tsx` | existe busca/cards; filtros status não interativos |
| Histórico | `mobile/app/(tabs)/history.tsx` | existe; retenção/SQLite não comprovados |
| Perfil | `mobile/app/(tabs)/profile.tsx` | existe; última sync é estática no código observado |
| Notificações | `mobile/app/notifications.tsx` | existe; dados mockados, sem lida/deep link comprovado |
| Detalhe | `mobile/app/inspections/[id]/index.tsx` | existe; transições locais, integração pendente |
| Checklist | `mobile/app/inspections/[id]/checklist.tsx` | existe; resposta em memória, persistência pendente |
| Evidências | `mobile/app/inspections/[id]/evidence.tsx` | existe; captura é simulada |
| Revisão | `mobile/app/inspections/[id]/review.tsx` | existe; conclusão local, envio pendente |
| QR | nenhum fluxo identificado | ausente/não comprovado |
| Localização | nenhuma integração identificada | ausente/não comprovado |
| Central sync | nenhuma central identificada | ausente/não comprovado |

## 14. Roteiro de validação por persona

### Técnico com conexão

1. login;
2. sincronizar;
3. conferir apenas suas inspeções;
4. abrir uma atribuída;
5. iniciar;
6. responder todos os itens;
7. registrar evidência;
8. revisar e enviar;
9. confirmar que o servidor mudou o estado;
10. conferir histórico.

### Técnico sem conexão

1. baixar previamente;
2. ativar modo avião;
3. iniciar;
4. responder e sair da tela;
5. adicionar foto e observação;
6. fechar/reabrir o app;
7. confirmar recuperação;
8. revisar;
9. concluir localmente;
10. verificar mensagem “envio pendente”;
11. reativar rede;
12. confirmar push/pull e status oficial.

### Técnico com reprovação

1. receber notificação/atualização;
2. abrir motivo;
3. reabrir correção explicitamente;
4. alterar somente o necessário;
5. preservar resposta anterior no histórico;
6. reenviar;
7. confirmar nova revisão.

### Usuário proibido

Executar login como ADMIN, SUPERVISOR ou técnico não atribuído e tentar abrir, alterar, anexar e concluir uma inspeção fora do escopo. Todas as tentativas devem falhar sem vazamento e sem mutação.

## 15. Definition of Done do mobile

O mobile só pode ser marcado como pronto quando:

- todas as telas P0 têm rota e estados completos;
- não há dados mockados ou ações “Simular” em caminhos de produção;
- API real está integrada;
- Secure Store, SQLite, outbox e arquivos foram testados;
- offline, reinício, retry, cursor, idempotência e conflito foram testados;
- câmera, QR e localização funcionam com permissões reais;
- técnico só acessa atribuições próprias;
- inspeção aprovada/cancelada fica protegida;
- `mvn test`, testes do painel e build Android passam;
- teste em Android 10/API 29+ foi executado;
- logs não contêm tokens, senhas ou conteúdo sensível;
- acessibilidade e estados de erro foram revisados;
- evidências e screenshots do fluxo completo estão anexadas à validação;
- nenhum aceite é inferido apenas por existir um arquivo de rota.

