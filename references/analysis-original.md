# Análise Completa — Workspace Notion "Projeto FieldOps"

> Documento gerado a partir da leitura integral de 22 páginas do workspace, incluindo o diagrama de classes (Anexo D). Todas as afirmações seguem a convenção: **[EXPLÍCITO]** (informação diretamente encontrada), **[INFERÊNCIA]** (conclusão derivada), **[CONFLITO]** (informações contraditórias entre páginas), **[NÃO ENCONTRADO]** (lacuna relevante).

---

## 1. Resumo Executivo

**O que é:** FieldOps é um projeto **acadêmico** (curso técnico/superior, disciplina com "Expo/React Native", Java/Spring Boot e Angular/React) que especifica uma plataforma digital de **inspeções técnicas em campo** [EXPLÍCITO — 01, 19, 16]. O workspace não é a documentação de um produto comercial em produção; é um enunciado de projeto de disciplina, com rubricas de nota, cronograma de 16 semanas de aula e critérios de avaliação individual e em equipe [EXPLÍCITO — 16, 19].

**Problema que resolve:** substitui um processo manual e fragmentado — formulários em papel, planilhas, fotos soltas, mensagens informais — por um fluxo digital único que conecta planejamento, execução em campo (inclusive offline) e revisão/aprovação [EXPLÍCITO — 03].

**Quem usa:** quatro perfis — Administrador (ADMIN), Supervisor (SUPERVISOR), Técnico (TECHNICIAN) e Cliente somente leitura (CLIENT_VIEWER, futuro/opcional) [EXPLÍCITO — 05]. Personas fictícias: Carlos (técnico), Marina (supervisora), Ana (administradora), Roberto (cliente) [EXPLÍCITO — 04].

**Como funciona:** um Supervisor cria e publica um **modelo de inspeção versionado**; agenda e atribui uma inspeção a um Técnico; o Técnico baixa a inspeção (mesmo sem internet), identifica o equipamento por QR Code, responde a um checklist dinâmico, tira fotos, registra localização e não conformidades; ao reconectar, o app sincroniza via fila de operações idempotentes (outbox); o Supervisor revisa e aprova ou reprova o resultado [EXPLÍCITO — 07].

**Principais funcionalidades (MVP/P0):** autenticação por perfil; cadastros de usuários/clientes/locais/equipamentos; construtor de modelos de inspeção com versionamento imutável e snapshot; planejamento/atribuição de inspeções; app mobile com checklist dinâmico, câmera, QR Code, localização, operação offline com SQLite e outbox; interface administrativa web com acompanhamento e revisão; API REST versionada e documentada [EXPLÍCITO — 08].

**Principais regras de negócio:** um técnico só inicia/responde inspeções atribuídas a ele; versões publicadas de modelo são imutáveis (mudanças geram nova versão); toda inspeção guarda um snapshot imutável dos itens; operações de sincronização são idempotentes (impedem duplicidade); inspeções aprovadas ficam protegidas contra edição comum; reprovação exige motivo [EXPLÍCITO — 09].

**Arquitetura:** três aplicações independentes que compartilham um contrato REST — app mobile (Expo/React Native + SQLite), interface administrativa (Angular/TypeScript) e API central (Java/Spring Boot + PostgreSQL), mais armazenamento de objetos para evidências [EXPLÍCITO — 11].

**Estado atual:** o workspace descreve **especificação, não implementação**. Não há evidência de código, repositórios funcionando, ou execução real do sistema em nenhuma página [INFERÊNCIA — ausência de qualquer registro de execução, testes reais ou build em todo o conteúdo lido]. O Anexo C mostra apenas um **exemplo ilustrativo** de ficha de inspeção preenchida, não um dado de sistema real [EXPLÍCITO — 21].

**Principais pendências:** nenhuma pergunta em aberto é formalmente listada no Notion (ver seção 18 deste documento, que eu construí a partir das lacunas identificadas). Há decisões de escopo explícitas que empurram diversos recursos para P1/P2 (dashboard avançado, notificações, portal do cliente, assinatura eletrônica, IA, IoT, ERPs) [EXPLÍCITO — 08, 01].

**Principais riscos:** cronograma apertado de 16 semanas efetivas para três times (mobile/web/backend) entregarem um fluxo ponta a ponta complexo com offline-first e sincronização idempotente [EXPLÍCITO — 16]; existe um **conflito estrutural** entre o modelo de dados textual (página 10) e o diagrama de classes (Anexo D) quanto a nomes de entidades, enums e novos perfis de usuário (ver seção 13 — Inconsistências).

---

## 2. Mapa do Notion

```text
Workspace: Projeto FieldOps
├── 01 - Visão Geral                         → visão de produto, componentes, escopo MVP, glossário
├── 02 - Objetivos                            → objetivos de negócio, mobile, admin, API, acadêmicos, indicadores de sucesso
├── 03 - Problema                             → contexto, processo atual, dores por perfil, causas, hipóteses
├── 04 - Personas                             → Carlos, Marina, Ana, Roberto (fictícios)
├── 05 - Perfis de Usuário                    → ADMIN/SUPERVISOR/TECHNICIAN/CLIENT_VIEWER, matriz de permissões
├── 06 - Casos de Uso                         → catálogo UC-01..UC-20 + 12 casos detalhados
├── 07 - Fluxo Geral                          → fluxo ponta a ponta, estados de negócio, estados de sync, transições, exceções
├── 08 - Funcionalidades                      → P0/P1/P2, tipos de resposta, recorte de 16 semanas
├── 09 - Regras de Negócio                    → RN-001 a RN-090 (10 categorias)
├── 10 - Modelo de Dados                      → 17 entidades, cardinalidades, integridade, offline
├── 11 - Arquitetura                          → visão distribuída, camadas, offline-first, segurança, RNF-001..012
├── 12 - API REST                             → convenções, endpoints por domínio, erros, sincronização
├── 13 - Aplicativo Mobile                    → navegação, telas, componentes dinâmicos, estados, testes
├── 14 - Interface Administrativa Web         → navegação, telas, construtor de modelos, testes
├── 15 - Backlog do Produto                   → 10 épicos, PBI-001 a PBI-082
├── 16 - Roadmap                              → 8 sprints / 16 semanas, marcos M1-M8
├── 17 - Critérios de Aceitação               → AC-* (Gherkin) por funcionalidade
├── 18 - Definição de Pronto (DoD)            → DoD por história, mobile, admin, API, sprint, MVP; severidade de defeitos
├── 19 - Critérios de Avaliação               → rubricas de nota (produto, Expo, Java, Angular), pesos por marco
├── 20 - Anexo A - Checklist executivo         → checklist de acompanhamento por frente
├── 20 - Anexo B - Decisões de escopo          → 17 decisões consolidadas
├── 21 - Anexo C - Exemplo de Ficha de Inspeção → inspeção fictícia completa (ilustrativa)
└── 22 - Anexo D - Diagrama de Classe          → imagem UML do modelo de dados
```

**Finalidade de cada página:** conforme listado acima — a numeração segue uma progressão clássica de especificação de software (visão → problema → usuários → requisitos → regras → dados → arquitetura → API → apps → gestão de projeto → qualidade → anexos).

**Relações entre páginas:** todas as páginas se referenciam mutuamente por natureza (não há bancos de dados Notion reais — todas são páginas de texto/markdown, não `Databases`, então "registros de banco" no sentido do Notion não existem neste workspace) [EXPLÍCITO — estrutura de arquivos exportados são todas `.md`, nenhuma indicação de coleção/database Notion nativa]. A página 09 (Regras) é referenciada implicitamente pelas páginas 06, 07, 08, 10, 17; a página 10 (Modelo de Dados) fundamenta 11, 12, 13, 14; a página 15 (Backlog) mapeia diretamente para 08 (Funcionalidades) e 16 (Roadmap); 17-18-19 formam o bloco de qualidade/avaliação.

**Entidades/conceitos mais frequentes:** Inspeção, Modelo/Template, Técnico, Supervisor, Snapshot, Sincronização/Outbox, Evidência, Não conformidade, Estado (de negócio e de sincronização).

**Assuntos centrais:** especificação funcional e de dados do fluxo de inspeção; operação offline-first; versionamento imutável de modelos.

**Assuntos secundários:** gestão pedagógica do projeto (rubricas, cronograma de aula, avaliação individual) — presentes mas paralelos ao produto em si.

---

## 3. Contexto do Projeto

**O que está sendo construído:** uma plataforma de três componentes (app mobile, painel web administrativo, API REST) para digitalizar inspeções técnicas de equipamentos/locais, com suporte a operação offline [EXPLÍCITO — 01].

**Problema resolvido:** fragmentação do processo de inspeção atual — perda de dados, retrabalho, falta de rastreabilidade, dificuldade de auditoria [EXPLÍCITO — 03].

**Usuários:** técnicos de campo (execução), supervisores (planejamento/revisão), administradores (cadastros/acesso), clientes (consulta, fora do MVP) [EXPLÍCITO — 04, 05].

**Objetivo:** demonstrar, ao final de 16 semanas de aula, um fluxo ponta a ponta funcional: criar → atribuir → executar offline → sincronizar → revisar → aprovar/reprovar [EXPLÍCITO — 16.5, Semana 16].

**Escopo dentro do MVP [EXPLÍCITO — 01.6, 01.7]:**
- Autenticação e perfis; cadastros; modelos de inspeção versionados; agendamento com um técnico responsável; execução offline; QR Code; câmera; localização pontual; sincronização idempotente; revisão e aprovação/reprovação.

**Escopo fora do MVP [EXPLÍCITO — 01.7]:**
- App exclusivo para clientes; portal público; multiempresa com isolamento completo; pagamento/faturamento; roteirização automática; rastreamento contínuo de localização; chamadas de vídeo; vídeos longos; assinatura eletrônica com validade jurídica; relatórios regulatórios avançados; integração com ERP; IA para diagnóstico automático; sensores IoT; resolução colaborativa de conflitos complexos.

**Principais componentes do sistema:** App Mobile (Expo/RN), Interface Administrativa (Angular), API REST (Spring Boot), Infraestrutura de dados (PostgreSQL + SQLite + armazenamento de objetos) [EXPLÍCITO — 01.4].

**[INFERÊNCIA]** Este é um projeto didático multi-turma/multi-disciplina (uma disciplina de "Expo", implicitamente outras de backend Java e frontend Angular), com entrega em equipes de estudantes — deduzido da presença de rubricas de nota individual/equipe, cronograma de aula com "conteúdo" e "projeto", e penalidades por uso de commits sem compreensão (seção 19).

---

## 4. Requisitos Funcionais

Extraídos principalmente do catálogo de Casos de Uso (página 06) e da lista de Funcionalidades P0 (página 08). Consolidados sem duplicar.

| ID | Nome | Ator | Objetivo | Pré-condições | Resultado esperado | Origem |
|---|---|---|---|---|---|---|
| UC-01 | Autenticar usuário | Todos | Obter acesso autorizado | Usuário ativo | Sessão criada ou acesso negado | 06.2 |
| UC-02 | Gerenciar usuários | Administrador | CRUD + status de usuários | Perfil ADMIN | Usuário criado/editado/inativado | 06.1, 08.2 |
| UC-03 | Gerenciar clientes, locais, equipamentos | Admin/Supervisor | Manter cadastros operacionais | Permissão | Registro persistido | 06.1, 08.2 |
| UC-04 | Criar modelo de inspeção | Supervisor | Padronizar checklist | Permissão | Versão publicada imutável | 06.3 |
| UC-05 | Publicar versão do modelo | Supervisor | Tornar modelo utilizável | Modelo com item válido | Nova versão numerada | 06.3, 09.3 |
| UC-06 | Agendar e atribuir inspeção | Supervisor | Planejar execução | Cliente/local/equip./técnico/modelo válidos | Inspeção em estado ATRIBUÍDA com snapshot | 06.4 |
| UC-07 | Baixar inspeções atribuídas | Técnico | Disponibilizar dados offline | Sessão + conectividade | Base local (SQLite) atualizada | 06.5 |
| UC-08 | Identificar equipamento por QR Code | Técnico | Confirmar ativo inspecionado | Permissão de câmera | Equipamento vinculado à inspeção | 06.6 |
| UC-09 | Iniciar inspeção | Técnico | Registrar início da execução | Inspeção atribuída | Estado local EM_ANDAMENTO | 06.7 |
| UC-10 | Responder checklist | Técnico | Preencher itens do snapshot | Inspeção em andamento | Respostas salvas localmente | 06.8 |
| UC-11 | Registrar evidência | Técnico | Comprovar resposta/ocorrência | Permissão de câmera/arquivos | Evidência associada e na fila de sync | 06.9 |
| UC-12 | Registrar não conformidade | Técnico | Documentar problema encontrado | Inspeção em andamento | NC criada e relacionada ao item | 06.10 |
| UC-13 | Concluir inspeção offline | Técnico | Finalizar execução | Itens obrigatórios respondidos | Estado "aguardando sincronização" | 06.11 |
| UC-14 | Sincronizar alterações | Técnico/Sistema | Enviar/receber dados pendentes | Conectividade + operações pendentes | Outbox processada sem duplicidade | 06.12 |
| UC-15 | Acompanhar inspeções | Supervisor | Monitorar andamento | Permissão | Listagem filtrável | 06.1, 08.2 |
| UC-16 | Revisar inspeção | Supervisor | Analisar resultado | Inspeção enviada | Decisão preparada | 06.13 |
| UC-17 | Aprovar ou reprovar inspeção | Supervisor | Encerrar ciclo de revisão | Inspeção em revisão | Estado APROVADA/REPROVADA | 06.14 |
| UC-18 | Consultar histórico | Admin/Supervisor | Auditoria | Permissão | Histórico exibido | 06.1 |
| UC-19 | Consultar indicadores | Supervisor | Visão gerencial | Permissão | Dashboard exibido | 06.1, 14.5 |
| UC-20 | Consultar resultado como cliente | Cliente | Transparência | Perfil CLIENT_VIEWER (futuro) | Resultado exibido (somente leitura) | 06.1, 04.4 |

**Detalhamento dos casos com fluxos alternativos documentados (UC-01, 04, 06, 07, 08, 09, 10, 11, 12, 13, 14, 16, 17):** ver página 06 na íntegra — cada um possui fluxo principal, fluxos alternativos e (parcialmente) pós-condições [EXPLÍCITO — 06]. **[NÃO ENCONTRADO]** Pós-condições explícitas não foram descritas para UC-10, UC-11, UC-12, UC-16 (apenas para UC-01, 04, 06, 07 e implicitamente para os demais via "fluxo principal").

**[NÃO ENCONTRADO]** Dependências formais entre casos de uso (ex.: "UC-14 depende de UC-13") não são explicitadas como uma matriz; a dependência é apenas textual/sequencial no Fluxo Geral (página 07).

---

## 5. Requisitos Não Funcionais

Fonte principal: página 11.14 (RNF-001 a RNF-012), complementada por menções esparsas em 08, 09, 12, 13, 14.

| ID | Categoria | Requisito | Origem |
|---|---|---|---|
| RNF-001 | Confiabilidade/Offline | App mobile deve preservar dados pendentes após reinicialização | 11.14 |
| RNF-002 | Consistência de API | API deve responder erros em formato padronizado | 11.14, 12.2 |
| RNF-003 | Escalabilidade/Usabilidade | Listagens administrativas devem usar paginação | 11.14 |
| RNF-004 | Usabilidade | App deve apresentar estados de carregamento, vazio, erro e offline | 11.14 |
| RNF-005 | Consistência/Sincronização | Operações de sincronização devem ser idempotentes | 11.14, 09.9 |
| RNF-006 | Compatibilidade | Interface mobile utilizável em telas Android comuns | 11.14, 01.8 |
| RNF-007 | Compatibilidade | Interface administrativa responsiva para notebook/desktop | 11.14 |
| RNF-008 | Documentação/Manutenibilidade | Contrato da API documentado (OpenAPI) | 11.14, 12.1 |
| RNF-009 | Manutenibilidade | Código deve passar por análise de tipos e lint | 11.14, 18.1 |
| RNF-010 | Segurança | Dados sensíveis não incluídos no repositório | 11.14, 09.1, 11.10 |
| RNF-011 | Auditoria | Alterações críticas devem ser auditáveis | 11.14, 09.11 |
| RNF-012 | Operabilidade | Componentes executáveis por instruções documentadas | 11.14 |

**Segurança (complementar, 11.10, 09.1):** HTTPS obrigatório em ambientes publicados; hash seguro de senha; tokens de duração limitada; armazenamento seguro de tokens no mobile (Secure Store); validação de autorização por endpoint na API (não confiar apenas na interface); CORS restrito; logs sem senhas/tokens/arquivos; segredos fora do repositório; erros sem stack trace exposto.

**Performance (13.10):** listas virtualizadas quando necessário; controle de tamanho/qualidade de imagem; consultas locais indexadas; sincronização em lotes controlados.

**Disponibilidade/Offline:** operação sem conexão após download prévio de inspeção (01.8, RN-065, RN-066).

**Observabilidade (11.11):** identificador de correlação por requisição; logs de autenticação e falha de sincronização sem dados sensíveis; auditoria de transições de estado.

**Acessibilidade (13.9, 18.1):** contraste suficiente; não depender exclusivamente de cor para status; texto de erro próximo ao campo; "acessibilidade básica" verificada no DoD — **[NÃO ENCONTRADO]** nenhum padrão específico (WCAG nível, por exemplo) é citado.

**Privacidade:** **[NÃO ENCONTRADO]** não há menção a LGPD, retenção de dados pessoais, ou política de privacidade explícita, apesar de o sistema tratar dados de localização e imagens que podem identificar pessoas/instalações.

---

## 6. Regras de Negócio

O Notion já as numera de RN-001 a RN-090 na página 09, organizadas em 10 categorias. Todas são **[EXPLÍCITO]**, exceto quando indicado.

### 6.1 Usuários e acesso (RN-001 a RN-008)
Apenas usuários ativos iniciam sessão; inativação não apaga histórico; API valida perfil em toda operação protegida; técnico só vê integralmente suas próprias inspeções; supervisor vê as de seu escopo; administrador não altera respostas de inspeção por padrão; credenciais/tokens nunca em log; troca de senha/bloqueio invalida sessões.
**Aplica-se a:** UC-01, UC-02, matriz de permissões (05.2). **Dependência:** RN-004/RN-005 sustentam o design dos endpoints `/mobile/inspections` (12.10).

### 6.2 Clientes, locais e equipamentos (RN-009 a RN-014)
Todo local pertence a um cliente; todo equipamento pertence a um local no MVP; QR Code único; equipamento inativo não pode ser usado em nova inspeção; registros usados em inspeções são inativados, não excluídos; local deve ser compatível com o equipamento selecionado.
**Aplica-se a:** UC-03, UC-06, cadastros (10.5).

### 6.3 Modelos de inspeção (RN-015 a RN-023)
Modelo precisa de título, categoria e ao menos uma seção com item válido antes de publicar; todo item precisa de tipo de resposta; ordem explícita; rascunho editável livremente; versão publicada não pode ser alterada destrutivamente; alterações geram nova versão; inspeção preserva snapshot; desativação de modelo não afeta inspeções existentes; tipos de resposta não suportados pelo app não podem ser publicados no MVP.
**Aplica-se a:** UC-04, UC-05, construtor de modelos (14.8). **Dependência:** RN-019/020/021 sustentam RN-024 (versão publicada obrigatória) e o próprio conceito de `InspectionItemSnapshot` (10.8.3).

### 6.4 Planejamento e atribuição (RN-024 a RN-031)
Inspeção vinculada a versão publicada; precisa de cliente/local/técnico/data prevista; equipamento opcional só quando a inspeção é de local/ambiente; só técnico ativo recebe atribuição; inspeção atribuída aparece ao técnico após sincronização; cancelamento registra usuário/data/justificativa; inspeção aprovada não pode ser cancelada pelo fluxo comum; **MVP permite apenas um técnico responsável por inspeção**.
**Aplica-se a:** UC-06. **Conflito potencial:** ver seção 13 (RN-031 x matriz de permissões que já prevê múltiplos perfis "Opcional" respondendo).

### 6.5 Execução (RN-032 a RN-044)
Só inspeções atribuídas podem ser iniciadas; só o técnico responsável inicia/responde (exceção administrativa não prevista no MVP); registra data/hora do dispositivo e do servidor; cada resposta vinculada a um item do snapshot; validação por tipo; itens obrigatórios exigidos antes da conclusão; resposta não conforme pode exigir observação; item crítico não conforme pode exigir evidência; progresso considera itens respondidos/aplicáveis; salvamento local imediato; após conclusão local, respostas ficam bloqueadas até reprovação; inspeção enviada precisa ser revisada antes de aprovar.
**Aplica-se a:** UC-09, UC-10, UC-13.

### 6.6 Evidências (RN-045 a RN-051)
Toda evidência vinculada a uma inspeção (e opcionalmente a resposta/NC); só formatos/tamanhos permitidos; evidência não sincronizada não pode ser apagada silenciosamente por limpeza de cache; exclusão de evidência sincronizada é auditada e respeita o estado da inspeção; evidências de inspeção aprovada são somente leitura; nome original do arquivo não é identificador único; metadados sensíveis podem ser removidos.
**Aplica-se a:** UC-11.

### 6.7 Não conformidades (RN-052 a RN-057)
Toda NC pertence a uma inspeção; pode ser associada a um item específico; criticidade em {baixa, média, alta, crítica}; NC crítica exige descrição e evidência; exclusão pós-envio deve virar alteração de estado/operação auditável; tratamento completo pós-aprovação é **versão futura** (mas o registro é MVP).
**Aplica-se a:** UC-12.

### 6.8 Localização e QR Code (RN-058 a RN-064)
Consentimento obrigatório antes de acessar câmera/localização; recusa gera orientação/alternativa quando permitido; localização **não é coletada continuamente** no MVP, apenas no início e na conclusão; armazena latitude/longitude/precisão/horário; leitura de QR não autoriza sozinha acesso a dados não permitidos; divergência entre equipamento previsto e lido deve ser informada antes de prosseguir.
**Aplica-se a:** UC-08.

### 6.9 Offline e sincronização (RN-065 a RN-078)
Inspeções baixadas permanecem acessíveis offline; dados não sincronizados sobrevivem a fechar/reabrir o app; cada operação tem ID idempotente único; reenvio não duplica; operações respeitam dependências; falha em uma não apaga as demais pendentes; app mostra quantidade pendente e resultado da última sincronização; **servidor é fonte oficial após confirmação**; preserva data do dispositivo + data de recebimento; conflitos detectados por versão/data; **dados aprovados no servidor prevalecem e não aceitam atualização tardia comum**; cancelamento no servidor não descarta dados locais pendentes sem aviso; arquivos podem sincronizar separadamente dos dados estruturados.
**Aplica-se a:** UC-14. Esta é a categoria mais extensa e crítica do sistema (14 regras).

### 6.10 Revisão e aprovação (RN-079 a RN-085)
Só supervisor autorizado inicia revisão; reprovação exige motivo; aprovação registra supervisor/data/comentário opcional; inspeção aprovada bloqueada para edição comum; correções pós-reprovação preservam histórico de versões anteriores; supervisor não altera silenciosamente a resposta do técnico; ajustes administrativos são diferenciados de respostas de campo e auditados.
**Aplica-se a:** UC-16, UC-17.

### 6.11 Auditoria e integridade (RN-086 a RN-090)
Alterações críticas registram usuário/data/ação/entidade; datas de criação/atualização mantidas pelo servidor; entidades em registros históricos não são excluídas fisicamente pelo fluxo comum; API rejeita transições de estado inválidas; documentação da API reflete estados/validações/erros implementados.

**Regras inferidas (não numeradas explicitamente, mas decorrentes da leitura cruzada):**
- **[INFERÊNCIA]** Um técnico inativado durante uma inspeção em andamento provavelmente não pode mais operá-la (decorre de RN-001 + RN-027), mas **[NÃO ENCONTRADO]** o comportamento exato (bloquear a inspeção? reatribuir automaticamente?) não é especificado.
- **[INFERÊNCIA]** Como RN-031 limita o MVP a um único técnico responsável, mas a matriz de permissões (05.2) marca "Responder checklist" como "Opcional" para Supervisor, há uma tensão entre regra estrita e exceção administrativa — tratada explicitamente como "não prevista no MVP" em RN-033, o que resolve parcialmente a ambiguidade.

---

## 7. Entidades e Domínio

Baseado na página 10 (Modelo de Dados) e cruzado com o Anexo D (diagrama de classes). Ver seção 13 para conflitos entre as duas fontes.

| Entidade | Finalidade | Cria | Altera | Consulta | Exclui | Regras associadas |
|---|---|---|---|---|---|---|
| **User** | Representar usuários e perfis | Admin | Admin (dados), próprio (senha) | Admin/Supervisor | Nunca (só inativa) | RN-001,002,003,006,008 |
| **Client** | Organização atendida | Admin/Supervisor | Admin/Supervisor | Admin/Supervisor | Nunca (só inativa) | RN-009,013 |
| **InspectionSite** | Local/unidade do cliente | Admin/Supervisor | Admin/Supervisor | Admin/Supervisor/Técnico (leitura) | Nunca (só inativa) | RN-009,013,014 |
| **Equipment** | Ativo físico inspecionável | Admin/Supervisor | Admin/Supervisor | Todos os perfis (conforme escopo) | Nunca (só inativa) | RN-010,011,012,013,014 |
| **InspectionTemplate** | Identidade lógica do checklist | Supervisor | Supervisor (se rascunho) | Todos autorizados | Nunca (só inativa) | RN-015,018,022 |
| **InspectionTemplateVersion** | Publicação imutável do modelo | Supervisor (via publicação) | Nunca (imutável) | Todos autorizados | Nunca | RN-019,020,021,023 |
| **TemplateSection** | Agrupa itens do checklist | Supervisor | Supervisor (se rascunho) | — | — | RN-017 |
| **TemplateItem** | Pergunta/verificação do checklist | Supervisor | Supervisor (se rascunho) | — | — | RN-016,017 |
| **Inspection** | Execução real de um checklist | Supervisor (agendamento) | Técnico (execução)/Supervisor (revisão) | Técnico (própria)/Supervisor/Admin | Nunca (cancelamento é estado) | RN-024 a RN-031 |
| **InspectionItemSnapshot** | Cópia imutável do item no momento da execução | Sistema (na criação da inspeção) | Nunca | Técnico/Supervisor | Nunca | RN-021,035 |
| **InspectionResponse** | Resposta do técnico a um item | Técnico | Técnico (até bloqueio) | Técnico/Supervisor | Não (via fluxo comum) | RN-035 a RN-043 |
| **Evidence** | Metadados de fotos/arquivos | Técnico | — | Técnico/Supervisor | Auditada, condicionada ao estado | RN-045 a RN-051 |
| **NonConformity** | Problema/irregularidade identificada | Técnico | — | Técnico/Supervisor | Vira alteração de estado auditável | RN-052 a RN-057 |
| **InspectionReview** | Ciclo de revisão do supervisor | Supervisor | — (histórico imutável) | Admin/Supervisor | Nunca | RN-079 a RN-085 |
| **AuditEvent** | Log de ações relevantes | Sistema | Nunca | Admin/Supervisor | Nunca (por usuários comuns) | RN-086 a RN-090 |
| **SyncOperation** (local) | Operação pendente na outbox do dispositivo | App mobile | App mobile | App mobile | Após confirmação | RN-067 a RN-070, 078 |
| **SyncMetadata** (local) | Estado geral da sincronização local | App mobile | App mobile | App mobile | — | 10.13.4 |

### Diagrama de relacionamentos (textual, conforme 10.3)

```text
USER
 ├── cria/agenda ──── N INSPECTION
 ├── executa ──────── N INSPECTION
 ├── revisa ───────── N INSPECTION_REVIEW
 └── realiza ações ── N AUDIT_EVENT

CLIENT 1 ── N INSPECTION_SITE
INSPECTION_SITE 1 ── 0..N EQUIPMENT

INSPECTION_TEMPLATE 1 ── N INSPECTION_TEMPLATE_VERSION
INSPECTION_TEMPLATE_VERSION 1 ── N TEMPLATE_SECTION
TEMPLATE_SECTION 1 ── N TEMPLATE_ITEM
INSPECTION_TEMPLATE_VERSION 1 ── N INSPECTION

INSPECTION 1 ── N INSPECTION_ITEM_SNAPSHOT
INSPECTION_ITEM_SNAPSHOT 1 ── 0..1 INSPECTION_RESPONSE
INSPECTION_RESPONSE 1 ── 0..N EVIDENCE

INSPECTION 1 ── 0..N NON_CONFORMITY
INSPECTION_RESPONSE 1 ── 0..N NON_CONFORMITY
NON_CONFORMITY 1 ── 0..N EVIDENCE

INSPECTION 1 ── 0..N INSPECTION_REVIEW
INSPECTION 1 ── N AUDIT_EVENT
```

**[INFERÊNCIA]** O relacionamento entre `Inspection` e `Evidence` diretamente (sem passar por `InspectionResponse`/`NonConformity`) confirma que evidências podem existir soltas na inspeção, não só atreladas a um item — condizente com 10.9.1 ("uma evidência sempre pertencerá a uma inspeção; vínculos com resposta e não conformidade são opcionais").

---

## 8. Funcionalidades

Consolidado de 08 (Funcionalidades), 15 (Backlog) e 17 (Critérios de Aceitação), eliminando duplicidade entre "funcionalidade descrita" e "história de backlog equivalente".

| Nome | Descrição | Usuário | Prioridade | Status | Dependências | Regras de negócio | Requisitos relacionados |
|---|---|---|---|---|---|---|---|
| Login e sessão | Autenticação por e-mail/senha, emissão/renovação de token | Todos | P0 | Especificado | — | RN-001..008 | UC-01, AC-AUTH |
| Gestão de usuários | CRUD + inativação de usuários e perfis | Admin | P0 | Especificado | Login | RN-001..003,006 | UC-02, AC-USERS |
| Cadastros (cliente/local/equip.) | CRUD + inativação lógica, QR único | Admin/Supervisor | P0 | Especificado | Login | RN-009..014 | UC-03, AC-MASTER |
| Construtor de modelos | Rascunho, seções, itens, tipos de resposta, prévia, publicação versionada | Supervisor | P0 | Especificado | Cadastros | RN-015..023 | UC-04/05, AC-TEMPLATE |
| Planejamento/atribuição | Agendar inspeção a partir de modelo publicado | Supervisor | P0 | Especificado | Modelos, cadastros | RN-024..031 | UC-06, AC-SCHEDULING |
| App do técnico (lista/detalhe) | Consulta de inspeções atribuídas, filtros | Técnico | P0 | Especificado | Planejamento, sync | RN-004,028 | UC-07, AC-MOBILE-LIST |
| Checklist dinâmico | Renderização por tipo de resposta, validação, progresso | Técnico | P0 | Especificado | Snapshot | RN-035..041 | UC-10, AC-CHECKLIST |
| Identificação por QR Code | Leitura e vínculo do equipamento | Técnico | P0 | Especificado | Permissão de câmera | RN-063,064 | UC-08, AC-QR |
| Captura de evidências | Foto, prévia, associação a item | Técnico | P0 | Especificado | Permissão de câmera | RN-045..051 | UC-11, AC-EVIDENCE |
| Registro de localização | Captura pontual início/fim | Técnico | P0 | Especificado | Permissão de localização | RN-058..062 | AC-LOCATION |
| Registro de não conformidade | Título/descrição/criticidade/evidência | Técnico | P0 | Especificado | Checklist | RN-052..057 | UC-12, AC-NONCONFORMITY |
| Conclusão de inspeção | Validação final, bloqueio de edição | Técnico | P0 | Especificado | Checklist completo | RN-037,042,043 | UC-13, AC-COMPLETE |
| Operação offline (SQLite) | Persistência local de tudo | Técnico | P0 | Especificado | — | RN-065,066 | RNF-001 |
| Sincronização (outbox) | Envio em lote idempotente, cursor de pull | Técnico/Sistema | P0 | Especificado | Conectividade | RN-067..078 | UC-14, AC-SYNC, RNF-005 |
| Acompanhamento administrativo | Listagem/filtros de inspeções | Supervisor/Admin | P0 | Especificado | Planejamento | — | UC-15, AC-ADMIN-MONITOR |
| Revisão/aprovação/reprovação | Análise item a item, decisão, motivo obrigatório | Supervisor | P0 | Especificado | Inspeção enviada | RN-079..085 | UC-16/17, AC-REVIEW |
| Auditoria de eventos | Log de ações críticas | Sistema | P0 | Especificado | Todas as demais | RN-086..090 | AC-SECURITY |
| Dashboard/indicadores | Resumo por estado/criticidade | Supervisor/Admin | P1 | Planejado (evolução) | Acompanhamento | — | 12.16, 14.5 |
| Notificações (local/push) | Alertas de prazo/atribuição | Técnico | P1 | Planejado | — | — | 08.3 |
| Relatório PDF | Exportação do resultado | Supervisor | P1 | Planejado | — | — | 08.3 |
| Assinatura desenhada | Captura de assinatura no dispositivo | Técnico | P1 | Planejado | — | — | 08.3 |
| Reatribuição de técnico | Trocar técnico responsável | Supervisor | P1 | Planejado | Conflita com RN-031 (ver seção 13) | — | 08.3 |
| Portal do cliente | Acesso self-service ao resultado | Cliente | P2 | Futuro | — | — | 04.4, 01.7 |
| Fluxo completo de NC (plano de ação) | Tratamento pós-aprovação da NC | Supervisor | P2 | Futuro | — | RN-057 (adiada) | 08.4 |

**Duplicidade identificada e consolidada:** "Funcionalidades" (página 08) e "Backlog" (página 15) descrevem essencialmente o mesmo escopo em dois formatos (lista funcional vs. histórias de usuário por sprint). Não há divergência de conteúdo relevante entre elas — apenas granularidade diferente (08 é por capacidade, 15 é por história/sprint). Não configura conflito, apenas visões complementares do mesmo escopo [EXPLÍCITO — comparação cruzada 08 x 15].

---

## 9. Fluxos

### 9.1 Fluxo de valor ponta a ponta [EXPLÍCITO — 07.1]
```text
ADMINISTRAÇÃO → CONFIGURAÇÃO → PLANEJAMENTO → DISTRIBUIÇÃO →
EXECUÇÃO → OPERAÇÃO OFFLINE → SINCRONIZAÇÃO → REVISÃO → DECISÃO → ENCERRAMENTO
```

### 9.2 Fluxo administrativo (10 passos) [EXPLÍCITO — 07.2]
Quem inicia: Administrador/Supervisor. O que acontece: cadastro de usuários e cadastros básicos → criação/organização/publicação de modelo → agendamento e atribuição → acompanhamento → revisão → decisão. Regras aplicadas: RN-015 a RN-031, RN-079 a RN-085. Resultado: inspeção aprovada ou reprovada, com histórico. Componentes envolvidos: Interface Administrativa + API.

### 9.3 Fluxo do técnico (14 passos) [EXPLÍCITO — 07.3]
Quem inicia: Técnico. O que acontece: autenticação → sincronização → consulta → confirmação de equipamento → início → resposta ao checklist → registro de observações/fotos/NC → salvamento local contínuo → conclusão → envio (imediato ou pendente) → acompanhamento da sincronização → (se reprovado) reexecução. Decisões existentes: iniciar ou não conforme estado; localização permitida ou negada; item obrigatório pendente bloqueia conclusão. Regras aplicadas: RN-032 a RN-044. Erro: itens obrigatórios pendentes bloqueiam avanço; evidência obrigatória ausente bloqueia conclusão. Componentes: App mobile + SQLite + API (na sincronização).

### 9.4 Estados de negócio da inspeção [EXPLÍCITO — 07.4, 07.6]
```text
RASCUNHO → (atribuir técnico) → ATRIBUÍDA
ATRIBUÍDA → (iniciar) → EM_ANDAMENTO | (cancelar) → CANCELADA
EM_ANDAMENTO → (concluir+sincronizar) → ENVIADA | (cancelar administrativamente) → CANCELADA
ENVIADA → (iniciar revisão) → EM_REVISÃO
EM_REVISÃO → (aprovar) → APROVADA | (reprovar) → REPROVADA
REPROVADA → (reabrir para correção) → EM_ANDAMENTO | (cancelar) → CANCELADA
```
**Nota importante do próprio Notion:** "O estado de negócio da inspeção não deve ser confundido com o estado de sincronização do dispositivo" [EXPLÍCITO — 07.4].

### 9.5 Estados locais de sincronização [EXPLÍCITO — 07.5]
SINCRONIZADO, PENDENTE, SINCRONIZANDO, ERRO, CONFLITO.

### 9.6 Fluxo offline [EXPLÍCITO — 07.7]
```text
Download (com conexão) → SQLite → perda de conexão → respostas/evidências salvas
localmente → cada alteração gera operação na outbox → conclusão → "Aguardando
sincronização" → conexão retorna → outbox enviada em ordem → API confirma/rejeita
cada operação → app atualiza dados e estado
```

### 9.7 Fluxo de correção (pós-reprovação) [EXPLÍCITO — 07.8]
```text
Supervisor reprova → motivo/itens de correção registrados → técnico sincroniza →
app mostra "Requer correção" → técnico altera somente o liberado → nova versão de
respostas enviada → supervisor revisa novamente
```

### 9.8 Tratamento de exceções (07.9) [EXPLÍCITO]
- App fechado durante preenchimento → respostas confirmadas permanecem salvas.
- Falha ao enviar foto → dados textuais não descartados; foto fica pendente.
- Inspeção cancelada enquanto técnico está offline → sistema preserva alterações locais e informa cancelamento antes de descartar dado algum.
- Sessão expirada → tenta renovar; se falhar, mantém dados locais e solicita nova autenticação.
- Operação reenviada → API reconhece ID idempotente, impede duplicidade.
- Modelo alterado após agendamento → inspeção continua usando o snapshot da versão selecionada.

---

## 10. Arquitetura

### 10.1 Visão geral [EXPLÍCITO — 11.1]
```text
App Mobile (Expo/RN + SQLite + Secure Store)
        │ HTTPS/JSON/Multipart
        ▼
API REST (Java/Spring Boot — Segurança + Regras)
        │
   ┌────┴─────┐
   ▼          ▼
PostgreSQL   Armazenamento de evidências
   ▲
   │ HTTPS/JSON
Interface Administrativa (Angular/TypeScript)
```

### 10.2 Contextos funcionais [EXPLÍCITO — 11.3]
Identidade e acesso · Cadastros · Modelos de inspeção · Planejamento · Execução de campo · Sincronização · Revisão · Auditoria e indicadores.

### 10.3 Estrutura sugerida por aplicação
- **Mobile** [EXPLÍCITO — 11.4]: `app/` (rotas Expo Router), `features/`, `components/`, `design-system/`, `application/`, `domain/`, `infrastructure/{api,database,repositories,storage,sync}/`, `hooks/`, `schemas/`, `utils/`, `config/`.
- **Admin web** [EXPLÍCITO — 11.5]: `core/{auth,guards,interceptors,http,layout}/`, `shared/`, `features/{dashboard,users,clients,sites,equipment,templates,inspections,reviews,non-conformities}/`.
- **API** [EXPLÍCITO — 11.6]: pacotes por domínio (`auth, user, client, site, equipment, template, inspection, evidence, synchronization, review, audit, shared`), cada um com `controller/application/domain/repository/dto/mapper/validation`.

### 10.4 Persistência [EXPLÍCITO — 11.7, 10.1]
PostgreSQL como fonte oficial pós-sincronização; SQLite como persistência **operacional** (não cache descartável) no dispositivo; migrações versionadas; índices para e-mail, QR Code, estados, técnico, datas; controle otimista via campo `version`; exclusão lógica para históricos.

### 10.5 Armazenamento de evidências [EXPLÍCITO — 11.8]
Metadados + chave do objeto no PostgreSQL; arquivo em armazenamento compatível com objetos (S3 ou equivalente); acesso via endpoint autorizado ou URL temporária (nunca exposição pública permanente).

### 10.6 Estratégia offline-first [EXPLÍCITO — 11.9]
Dados baixados = subconjunto autorizado ao técnico; escrita local sempre primeiro (SQLite); outbox com ID idempotente, tipo, payload, dependências, tentativas, estado; download por cursor, atualizado só após persistência local bem-sucedida; conflitos: inspeções aprovadas não aceitam alteração tardia comum, versões divergentes geram conflito explícito, app nunca descarta valor local silenciosamente.

### 10.7 Segurança e observabilidade [EXPLÍCITO — 11.10, 11.11]
Ver seção 5 (Requisitos Não Funcionais) para detalhamento — não duplicado aqui.

### 10.8 Ambientes e CI/CD [EXPLÍCITO — 11.12, 11.13]
4 ambientes (Local, Integração, Teste/Demonstração, Produção acadêmica opcional); pipelines separados por aplicação (compilação/teste/lint/build/publicação), com EAS Build para o mobile no marco final.

**[INFERÊNCIA]** A arquitetura descrita é apropriada para fins didáticos (separação clara de camadas, DDD leve por feature), mas não há menção a mensageria assíncrona, filas de processamento de upload em background no servidor, nem estratégia de escalabilidade horizontal — consistente com o escopo de "produto acadêmico", não corporativo (01.5, 02.8 "não objetivos").

---

## 11. Backlog e Tarefas

Fonte: página 15 (72 itens P0 organizados em 10 épicos + 10 itens P1 sugeridos), cruzada com 16 (Roadmap/Sprints) e 08.6 (recorte de escopo recomendado).

### 11.1 Épicos e resultado esperado [EXPLÍCITO — 15.2]

| Épico | Resultado esperado | Sprint(s) |
|---|---|---|
| EP-01 Fundação técnica | Projetos executáveis, padronizados, integráveis | 1 |
| EP-02 Identidade e acesso | Usuários autenticados e autorizados | 2 |
| EP-03 Cadastros operacionais | Clientes/locais/equipamentos disponíveis | 2 |
| EP-04 Modelos de inspeção | Checklists configuráveis e versionados | 3 |
| EP-05 Planejamento | Inspeções agendadas e atribuídas | 3-4 |
| EP-06 Execução de campo | Técnico executa checklist pelo mobile | 4 |
| EP-07 Recursos nativos e evidências | QR/câmera/localização integrados | 5 |
| EP-08 Offline e sincronização | Operação sem rede + envio confiável | 6 |
| EP-09 Revisão e decisão | Supervisor revisa/aprova/reprova | 7 |
| EP-10 Qualidade e entrega | Testes, docs, builds, demonstração | 7-8 |

**Todos os 72 itens (PBI-001 a PBI-072) estão marcados como P0** e distribuídos nas 8 sprints [EXPLÍCITO — 15.3]. Há ainda 10 itens P1 sugeridos (PBI-073 a PBI-082) não alocados a sprint específica [EXPLÍCITO — 15.4].

### 11.2 Status
**[NÃO ENCONTRADO]** Nenhum item do backlog possui status real preenchido (Backlog/Em andamento/Concluído) — a página 15.1 apenas *sugere a estrutura de propriedades* que um banco de dados Notion deveria ter (incluindo uma coluna "Status"), mas não existe, no export lido, um banco de dados populado com esse campo. Ou seja: **o backlog descrito é um plano, não um rastreamento real de execução** [INFERÊNCIA — decorre da ausência de qualquer instância populada; a própria página diz "estrutura recomendada... (Sugestão)"].

### 11.3 Observações sobre o backlog
- **Tarefas muito grandes:** PBI-072 ("demonstrar o fluxo ponta a ponta com dados reais de teste") é essencialmente um épico disfarçado de item único de sprint 8 — **[INFERÊNCIA]** deveria provavelmente ser desmembrado em subtarefas de teste por trilha (mobile/admin/API).
- **Dependências implícitas não formalizadas:** o modelo de propriedades sugerido inclui uma coluna "Dependências" (15.1), mas nenhuma dependência entre PBIs é explicitada linha a linha — apenas a ordem cronológica por sprint sugere dependência (ex.: PBI-018 a 024 do EP-04 precisam existir antes de PBI-025 do EP-05).
- **Política de refinamento** (15.5) define critérios de entrada em sprint (história compreensível, responsável, componente, prioridade, dependências, critérios de aceitação, contrato de API, tamanho adequado, dados/mock) — mas, como não há histórico de sprints reais, não é possível avaliar aderência.
- **Gestão de risco de cronograma** (16.7) já prevê ordem de corte de escopo em caso de atraso: 1) dashboard avançado; 2) tipos de resposta reduzidos aos essenciais; 3) só foto capturada (sem galeria); 4) construtor sem drag-and-drop; 5) conflito limitado a detectar+bloquear; 6) adiar notificações/PDF/assinatura; 7) núcleo inegociável = autenticação, planejamento, checklist, foto, offline, sincronização, revisão.

### 11.4 Recorte de escopo recomendado (08.6) [EXPLÍCITO]
Reforça o núcleo mínimo: um único técnico por inspeção; um único fluxo de revisão; tipos de resposta essenciais; fotografia como evidência principal; Android como validação mobile; sincronização em fila sem edição colaborativa simultânea; dashboard simples; portal do cliente fora do MVP.

---

## 12. Decisões

Fonte primária: Anexo B (20 - Decisões de escopo consolidadas), complementada por decisões pontuais espalhadas em outras páginas.

| # | Decisão | Contexto | Motivo (quando explícito) | Alternativas consideradas | Origem |
|---|---|---|---|---|---|
| 1 | Produto terá 3 aplicações (mobile, admin web, API) | Definição arquitetural inicial | Separação de responsabilidades | [NÃO ENCONTRADO] | Anexo B.1 |
| 2 | Interface administrativa é MVP, não substituível por Swagger | Evitar dependência de acesso técnico | Autonomia operacional para não-técnicos | Uso direto do Swagger/banco (rejeitada, penalizada — 19.9) | Anexo B.2 |
| 3 | Mobile prioritariamente Android | Validação obrigatória | Foco de esforço em 16 semanas | Multiplataforma total (permitida via Expo, mas não obrigatória) | Anexo B.3, 01.8 |
| 4 | App deve operar offline após baixar inspeção | Contexto de campo com conectividade instável | Continuidade operacional | [NÃO ENCONTRADO] | Anexo B.4 |
| 5 | SQLite é persistência operacional, não cache descartável | Fundamenta toda a estratégia offline-first | Evitar perda de dados | Cache volátil (rejeitada implicitamente) | Anexo B.5, 13.6 |
| 6 | Sincronização usa operações idempotentes | Evitar duplicidade em reenvios | Confiabilidade em rede instável | [NÃO ENCONTRADO] | Anexo B.6, RN-067 |
| 7 | Modelo de inspeção será versionado | Preservar integridade histórica | Rastreabilidade | Edição direta do modelo (rejeitada — RN-019) | Anexo B.7 |
| 8 | Cada inspeção preserva snapshot dos itens | Consequência da decisão 7 | Consistência de relatórios históricos | [NÃO ENCONTRADO] | Anexo B.8, 10.8.3 |
| 9 | MVP terá um técnico responsável por inspeção | Simplificação de escopo | Reduzir complexidade de coautoria | Múltiplos técnicos (adiada — RN-031, 08.6) | Anexo B.9 |
| 10 | Portal do cliente fica fora do MVP | Priorização de esforço | Foco no fluxo operacional | Portal completo (P2) | Anexo B.10, 01.7 |
| 11 | Fotografia é a evidência principal | Simplicidade e cobertura de caso de uso | [NÃO ENCONTRADO] | Vídeo, áudio (adiados — P2) | Anexo B.11 |
| 12 | QR Code e localização são obrigatórios no projeto mobile | Reforço pedagógico de recursos nativos | Aplicação de conteúdo da disciplina Expo | [NÃO ENCONTRADO] | Anexo B.12 |
| 13 | Coleta de localização é pontual (não contínua) | Privacidade/escopo | Evitar rastreamento contínuo (fora do MVP) | Rastreamento contínuo (explicitamente fora do escopo — 01.7) | Anexo B.13, RN-060 |
| 14 | Revisão é feita pela interface administrativa | Papel do supervisor centrado na web | Consistência com perfil de uso (Marina usa notebook) | [NÃO ENCONTRADO] | Anexo B.14 |
| 15 | 16 semanas efetivas são o limite de planejamento do MVP | Calendário acadêmico real (20 semanas nominais, mas com perdas) | Realismo de cronograma | [NÃO ENCONTRADO] | Anexo B.15, 16.1 |
| 16 | Semanas institucionais restantes não são reserva de desenvolvimento | Mesmo contexto acima | Evitar dependência de tempo não garantido | [NÃO ENCONTRADO] | Anexo B.16 |
| 17 | Avaliação combina produto da equipe e domínio individual | Natureza pedagógica do projeto | Evitar que só "quem entrega" seja avaliado, sem entender | [NÃO ENCONTRADO] | Anexo B.17, 19.7 |
| 18 | Perfil Cliente pode existir no modelo de autorização, mas portal completo é futuro | Especificação de perfis | Manter extensibilidade sem aumentar escopo do MVP | [NÃO ENCONTRADO] | 04.4 |
| 19 | Arrastar-e-soltar no construtor de modelos é desejável, não obrigatório | Tela administrativa de maior complexidade | Reduzir risco de esforço de UI | Ordenação por botões/campo de ordem (aceita como alternativa) | 14.8 |

**Todas as decisões acima são [EXPLÍCITO]** — nenhuma foi inferida pela análise, pois o próprio workspace as declara como decisões consolidadas.

---

## 13. Inconsistências

### 13.1 [CONFLITO] Modelo de dados textual (página 10) x Diagrama de Classes (Anexo D)

O diagrama de classes (Anexo D) introduz diferenças estruturais relevantes em relação ao modelo descrito textualmente na página 10:

- **Perfis de usuário divergentes:** a página 05 e 10.6.1 listam `ADMIN, SUPERVISOR, TECHNICIAN, CLIENT_VIEWER`. O Anexo D lista `UserRole: ADMIN, MANAGER, INSPECTOR, REVIEWER` — nomes e quantidade diferentes, sem menção a `TECHNICIAN`, `SUPERVISOR` ou `CLIENT_VIEWER`.
  **Impacto:** ambiguidade sobre qual nomenclatura de perfil deve ser efetivamente implementada no backend.
  **Recomendação:** consolidar em uma única fonte de verdade — provavelmente a página 05/09, por ser mais detalhada e citada por dezenas de regras de negócio (RN-001 a RN-090 usam "técnico"/"supervisor" nominalmente).

- **Entidade `EquipmentType` presente apenas no diagrama:** o Anexo D mostra uma classe `EquipmentType` (com `id, name, description, active`) relacionada a `Equipment`, que **não existe** na modelagem textual da página 10 (onde `Equipment` tem apenas um campo texto livre `manufacturer`/`model`, sem tipo estruturado).
  **Impacto:** se implementada conforme o diagrama, há uma entidade adicional não coberta pelas regras de negócio (RN-009 a RN-014) nem pelos endpoints da API REST (12.8).
  **Recomendação:** decidir formalmente se `EquipmentType` existe no MVP; se sim, atualizar página 10, regras de negócio e contrato de API.

- **Enums divergentes:** `InspectionStatus` no Anexo D lista `DRAFT, IN_PROGRESS, SUBMITTED, UNDER_REVIEW, APPROVED, REJECTED, CANCELED` (sem `ASSIGNED`), enquanto a página 10.8.1 e a página 07.4 listam `DRAFT/RASCUNHO, ASSIGNED/ATRIBUÍDA, IN_PROGRESS/EM_ANDAMENTO, SUBMITTED/ENVIADA, UNDER_REVIEW/EM_REVISÃO, APPROVED/APROVADA, REJECTED/REPROVADA, CANCELED/CANCELADA` (8 estados, incluindo `ASSIGNED`).
  **Impacto:** o estado `ASSIGNED`/`ATRIBUÍDA` é crítico — é a transição citada em RN-028 ("uma inspeção atribuída deverá aparecer para o técnico") e na tabela de transições (07.6). Sua ausência no diagrama pode ser uma omissão gráfica, mas gera risco de implementação incompleta.
  **Recomendação:** verificar se o diagrama está desatualizado em relação ao texto (mais provável) e corrigi-lo.

- **Novas entidades no diagrama sem contrapartida textual:** `NonConformityEvidence`, `NonConformityComment`, `TemplateItemOption` aparecem no Anexo D como classes próprias, enquanto a página 10 modela opções de seleção como campo `options_json` (JSON embutido em `TemplateItem`) e evidências de NC apenas via relacionamento direto `NonConformity 1 ── 0..N Evidence` (sem uma tabela de associação nomeada).
  **Impacto:** duas abordagens de modelagem incompatíveis (JSON embutido vs. tabelas normalizadas) para o mesmo conceito.
  **Recomendação:** decidir uma abordagem única antes da implementação do banco.

- **Novos status de não conformidade:** o Anexo D lista `NonConformityStatus: OPEN, IN_ANALYSIS, ACTION_REQUIRED, RESOLVED, CANCELED`, enquanto a página 10.10.1 define apenas `status: OPEN no MVP` (um único valor válido).
  **Impacto:** o diagrama sugere um fluxo de tratamento de NC mais completo que a própria página 09 (RN-057) diz ser "uma versão futura" (fora do MVP).
  **Recomendação:** manter `OPEN` como único status no MVP, conforme já decidido em RN-057 e 14.12 ("O acompanhamento completo de plano de ação será P2"), e simplificar o diagrama.

### 13.2 [CONFLITO] Regra de técnico único (RN-031) x funcionalidade de reatribuição P1

RN-031 declara "O MVP permitirá somente um técnico responsável por inspeção", e o campo `technician_id` no modelo de dados (10.8.1) é singular. Já a página 08.3 lista, como funcionalidade P1, "reatribuição de técnico" — que pressupõe trocar o técnico responsável de uma inspeção já atribuída.
**Impacto:** não é uma contradição lógica grave (reatribuir ainda mantém "um único técnico" por vez), mas não há regra de negócio nem critério de aceitação definindo o que ocorre com respostas/evidências já registradas pelo técnico original ao reatribuir.
**Recomendação:** ao especificar a funcionalidade P1, criar regras de negócio explícitas (ex.: "reatribuição só é permitida em estado ATRIBUÍDA, antes do início").

### 13.3 [CONFLITO] Matriz de permissões (05.2) x RN-033

A matriz de permissões (05.2) marca "Iniciar inspeção", "Responder checklist", "Registrar evidência", "Registrar não conformidade" e "Enviar para revisão" como **"Opcional"** para o Supervisor. Porém RN-033 afirma: "Somente o técnico responsável poderá iniciar e responder a inspeção, salvo permissão administrativa excepcional **não prevista no MVP**".
**Impacto:** a matriz sugere uma capacidade que a própria regra de negócio nega para o MVP — pode confundir quem lê apenas a matriz (05.2) sem chegar à regra específica (09.5).
**Recomendação:** marcar essas células da matriz como "Fora do MVP" ou "Futuro", em vez de "Opcional", para eliminar a ambiguidade.

### 13.4 [CONFLITO] Nomenclatura de estados em português vs. inglês

Ao longo do documento, os estados de negócio da inspeção aparecem ora em português (`RASCUNHO, ATRIBUÍDA, EM_ANDAMENTO...` — página 07) ora em inglês (`DRAFT, ASSIGNED, IN_PROGRESS...` — página 10.8.1, 12 e Anexo D). Não há uma definição explícita de que são sinônimos formais, embora o contexto deixe isso implícito.
**Impacto:** risco de implementação inconsistente entre os componentes (ex.: front-end em português, back-end em inglês) sem uma tabela de mapeamento formal.
**Recomendação:** declarar explicitamente, em um glossário técnico, que os termos em português são apenas descritivos/didáticos e que o valor de enum oficial é o inglês (conforme sugerido pelo contrato de API, que já usa inglês).

### 13.5 Endpoint de sincronização não decidido definitivamente

A página 12.15 apresenta três variações possíveis (`/mobile/sync/push`, `/mobile/sync/pull`, `/mobile/sync`) e conclui: "o projeto poderá utilizar um endpoint combinado ou endpoints separados. **O contrato escolhido deverá ser único e documentado**." Isso não é uma inconsistência entre páginas, mas uma decisão de design deliberadamente **não fechada** — sinalizada como pendência dentro do próprio texto.
**Impacto:** times de mobile e backend podem implementar contratos incompatíveis se não houver alinhamento explícito antes da Sprint 6.
**Recomendação:** tratar como item de decisão obrigatória antes do EP-08 (Sprint 6), com registro formal da escolha.

### 13.6 Duplicidade não conflitante: "Funcionalidades" x "Backlog"

Como já mencionado na seção 8, as páginas 08 e 15 descrevem o mesmo escopo em formatos diferentes. Não há conteúdo contraditório, apenas duplicidade de granularidade — não requer correção, mas atenção para manter as duas páginas sincronizadas se uma for atualizada.

### 13.7 Não foram encontradas (nesta leitura)
- Requisitos conflitantes adicionais entre páginas de RN e páginas de AC (Critérios de Aceitação) — os critérios de aceitação (17) parecem fielmente derivados das regras de negócio (09), sem contradição aparente.
- Status incompatíveis entre backlog e roadmap — como o backlog não tem status real preenchido (seção 11.2), não há como comparar.

---

## 14. Lacunas de Informação

### 14.1 Requisitos sem definição completa
- **[NÃO ENCONTRADO]** Política exata de retenção/expurgo de dados (quanto tempo evidências, respostas e eventos de auditoria são mantidos).
- **[NÃO ENCONTRADO]** Tamanho máximo de arquivo e formatos aceitos para evidências (RN-046 diz "formatos e tamanhos permitidos", mas não especifica os valores).
- **[NÃO ENCONTRADO]** Regras detalhadas de expiração de token (apenas "duração limitada", sem valor numérico, exceto o exemplo de payload em 12.4 que sugere `expiresIn: 900` segundos — mas isso está em um *exemplo* de resposta, não formalizado como requisito).
- **[NÃO ENCONTRADO]** Política de troca/recuperação de senha (existe endpoint `POST /users/{id}/reset-password`, mas o fluxo — quem dispara, e-mail de redefinição, validade do link — não é descrito).

### 14.2 Usuários não definidos
- **[NÃO ENCONTRADO]** Processo de primeiro acesso/onboarding de um usuário recém-cadastrado (como ele recebe a senha inicial?).
- **[NÃO ENCONTRADO]** Regras de "esquecimento" de senha para o Técnico em campo, sem acesso a e-mail corporativo.

### 14.3 Fluxos incompletos
- **[NÃO ENCONTRADO]** O que ocorre quando uma inspeção fica presa em `EM_REVISÃO` sem decisão por tempo prolongado (SLA de revisão)?
- **[NÃO ENCONTRADO]** Comportamento exato quando duas fotos são tiradas para o mesmo item e depois uma é excluída antes da sincronização versus depois — RN-047/048 cobrem o "não sincronizada" e o "já sincronizada", mas não o caso de exclusão parcial durante uma sincronização em andamento.
- **[NÃO ENCONTRADO]** Fluxo de resolução de conflito complexo é citado como fora do MVP ("casos complexos poderão exigir nova inspeção ou ação administrativa" — 11.9), mas o processo de "ação administrativa" não é detalhado.

### 14.4 Regras de negócio ausentes
- **[NÃO ENCONTRADO]** Regra de negócio explícita sobre o que ocorre com uma inspeção `ATRIBUÍDA` quando o supervisor que a criou é inativado.
- **[NÃO ENCONTRADO]** Regra sobre limite de tentativas de sincronização (`attempt_count` existe no modelo de `SyncOperation`, mas não há regra de negócio que defina o que ocorre após N tentativas falhas).

### 14.5 APIs sem contrato completo
- **[NÃO ENCONTRADO]** Contrato completo de resposta para `GET /api/v1/inspections/{id}/history` (apenas o endpoint é listado em 12.10, sem exemplo de payload).
- **[NÃO ENCONTRADO]** Detalhamento de paginação (formato exato de `page/size/sort`) — mencionado como "ou padrão equivalente documentado" (12.1), ou seja, deliberadamente em aberto.

### 14.6 Entidades sem atributos totalmente definidos
- **[NÃO ENCONTRADO]** `EquipmentType` (aparece só no Anexo D) não tem seus atributos formalmente descritos em texto — já registrado como conflito na seção 13.1.

### 14.7 Critérios de aceite inexistentes
- **[NÃO ENCONTRADO]** Não há critérios de aceitação (`AC-*`) para o Dashboard/indicadores (P1), Notificações (P1), nem para nenhuma funcionalidade P2 — compreensível, já que são fora do MVP, mas vale registrar como lacuna caso essas funcionalidades avancem.

### 14.8 Erros não tratados / Permissões não especificadas
- **[NÃO ENCONTRADO]** Comportamento da API ao receber um `EquipmentType` ou tipo de resposta desconhecido em uma requisição de sincronização (RN-023 fala de bloqueio na *publicação*, não no *envio de sincronização*).
- **[NÃO ENCONTRADO]** Regra de autorização específica para o endpoint `DELETE /api/v1/evidence/{id}` (existe o endpoint em 12.12, e a regra geral RN-048 diz que a exclusão "deverá ser auditada e respeitar o estado da inspeção", mas não define quem — só o técnico autor? qualquer supervisor? — pode excluir).

### Perguntas que precisam ser respondidas (priorizadas por impacto)

1. **[Alta prioridade]** Qual é a fonte de verdade para o modelo de dados — a página 10 (texto) ou o Anexo D (diagrama)? As divergências de enums e entidades (seção 13.1) precisam ser resolvidas antes de qualquer início de implementação do backend.
2. **[Alta prioridade]** Qual será o contrato definitivo de sincronização (endpoint único combinado ou push/pull separados)? Bloqueia o planejamento técnico do EP-08.
3. **[Alta prioridade]** Quais são os valores concretos de limites técnicos (tamanho máximo de arquivo, formatos aceitos, duração de token de acesso e refresh)? Impacta diretamente RN-046 e a segurança do sistema.
4. **[Média prioridade]** Como funciona o processo de primeiro acesso e recuperação de senha, especialmente para o técnico de campo?
5. **[Média prioridade]** Existe SLA para revisão de inspeções (tempo entre "enviada" e decisão do supervisor)?
6. **[Média prioridade]** Qual é o tratamento formal para "ação administrativa" em casos de conflito de sincronização complexo?
7. **[Baixa prioridade]** Os nomes de estados devem ser padronizados oficialmente em inglês (para persistência/API) com tradução apenas na camada de apresentação?
8. **[Baixa prioridade]** Existe necessidade de conformidade formal com LGPD dado que o sistema coleta localização geográfica e fotos que podem conter pessoas?

---

## 15. Riscos

| Risco | Probabilidade | Impacto | Causa | Mitigação recomendada | Nível |
|---|---|---|---|---|---|
| Modelo de dados inconsistente entre página 10 e Anexo D chega à implementação sem reconciliação | Média-Alta (documentos já divergem) | Alto (retrabalho de schema/API a meio do projeto) | Diagrama produzido separadamente do texto, sem sincronização posterior | Consolidar um único modelo de dados oficial antes da Sprint 3 (EP-04) | 🔴 Alto |
| Cronograma de 16 semanas efetivas é insuficiente para o escopo P0 completo (72 PBIs) em times de estudantes | Média | Alto (compromete o fluxo ponta a ponta na demonstração final) | Alta complexidade técnica (offline-first, idempotência, versionamento) para nível acadêmico | Seguir rigorosamente a ordem de corte de escopo já definida em 16.7 | 🔴 Alto |
| Contrato de sincronização não decidido a tempo gera integração incompatível entre mobile e API | Média | Alto (bloqueia toda a Sprint 6 — EP-08) | Página 12.15 deixa a decisão explicitamente em aberto | Fixar decisão formal e documentá-la no OpenAPI antes do início da Sprint 6 | 🟡 Médio |
| Perda de dados offline por falha de implementação da outbox | Baixa-Média | Crítico (viola RN-066, RN-070, RN-077; é listado como "defeito crítico" que bloqueia entrega — 18.7) | Complexidade de sincronização idempotente com dependências ordenadas | Testes automatizados específicos de outbox (já previstos em 13.11); simulação de falhas de rede | 🔴 Alto |
| Uso de Swagger/banco diretamente como substituto da interface administrativa durante demonstração | Média (citada explicitamente como penalidade conhecida) | Médio (afeta nota, não o funcionamento) | Pressão de prazo pode levar equipes a atalhos não permitidos | Reforço da Decisão de Escopo #2 (Anexo B) e da penalidade (19.9) | 🟡 Médio |
| Ambiguidade de perfis de usuário (matriz 05.2 x RN-033) causa implementação de "supervisor respondendo checklist" fora do escopo do MVP | Baixa-Média | Médio (funcionalidade extra não testada/não avaliada) | Célula "Opcional" na matriz sem nota de rodapé explicativa | Corrigir a matriz de permissões (seção 13.3) | 🟡 Médio |
| Ausência de tratamento formal de dados pessoais (LGPD) para localização/fotos | Baixa (não é foco de avaliação, mas é risco real de produto) | Médio (se o sistema fosse usado em produção real) | Escopo acadêmico não contemplou privacidade formalmente | Adicionar nota de escopo explícita ("fora do MVP: conformidade regulatória de dados pessoais") ou tratar minimamente | 🟢 Baixo |
| Equipes menos maduras tecnicamente podem falhar em publicar builds executáveis (Android, contêiner da API) no marco final | Média | Alto (é pré-requisito do DoD do MVP — 18.6) | Complexidade de build/deploy (EAS, contêineres, ambientes) | Antecipar tentativas de build a partir da Sprint 6-7, não deixar para a Sprint 8 | 🟡 Médio |

---

## 16. Análise de Qualidade da Documentação

| Critério | Nota (0-10) | Justificativa |
|---|---|---|
| Clareza | 9 | Redação consistentemente clara, direta, com exemplos concretos (fluxos textuais, JSON de exemplo, tabelas). Poucos trechos ambíguos. |
| Organização | 9 | Estrutura numerada (01 a 22) segue uma progressão lógica clássica de especificação de software; páginas se referenciam de forma coerente. |
| Completude | 7 | Muito completo para um documento de especificação de MVP acadêmico, mas com lacunas relevantes em contratos de API, limites técnicos e alguns fluxos de exceção (seção 14). |
| Consistência | 6 | A maior fragilidade do conjunto: o Anexo D (diagrama de classes) diverge do texto (página 10) em múltiplos pontos (seção 13.1), e há pequenas inconsistências pontuais na matriz de permissões. |
| Rastreabilidade | 8 | Regras de negócio numeradas (RN-*) e critérios de aceitação (AC-*) mapeiam-se bem entre si e com os casos de uso; falta apenas uma matriz explícita formal (que este documento supre na seção 17). |
| Especificação funcional | 9 | Casos de uso bem detalhados com fluxos alternativos; funcionalidades e backlog bem alinhados. |
| Especificação técnica | 8 | Arquitetura, modelo de dados e API bem descritos; boa cobertura de RNFs; prejudicada pela inconsistência do diagrama de classes. |
| Regras de negócio | 9 | Extremamente bem numeradas e organizadas por domínio (90 regras em 11 categorias), com boa cobertura dos cenários de offline e sincronização, que são o núcleo de complexidade do projeto. |
| Qualidade do backlog | 7 | Bem estruturado em épicos e histórias com formato "Como... quero... para...", mas sem dependências explícitas linha a linha e sem status real de execução (é um plano, não um rastreador ativo). |

**Média geral aproximada: 8,0/10** — documentação de alta qualidade para um projeto acadêmico, com um ponto de atenção estrutural claro (a reconciliação do diagrama de classes com o texto).

---

## 17. Matriz de Rastreabilidade

Relação simplificada Requisito → Regra → Funcionalidade → Fluxo → Entidade → Tarefa(s) do backlog, para os principais pontos do sistema.

| Requisito (UC) | Regras (RN) | Funcionalidade | Fluxo | Entidade(s) | Backlog (PBI) |
|---|---|---|---|---|---|
| UC-01 Autenticar | RN-001..008 | Login e sessão | Fluxo do técnico (passo 1), Fluxo admin (implícito) | User | PBI-007..012 |
| UC-04/05 Criar/publicar modelo | RN-015..023 | Construtor de modelos | Fluxo administrativo (passos 3-5) | InspectionTemplate, InspectionTemplateVersion, TemplateSection, TemplateItem | PBI-018..024 |
| UC-06 Agendar/atribuir | RN-024..031 | Planejamento/atribuição | Fluxo administrativo (passos 6-7) | Inspection, InspectionItemSnapshot | PBI-025..030 |
| UC-07 Baixar inspeções | RN-028, 065, 066 | App do técnico (lista) | Fluxo offline (download inicial) | Inspection (local), SyncMetadata | PBI-031, 048 |
| UC-08 QR Code | RN-011, 063, 064 | Identificação por QR | Fluxo do técnico (passo 5) | Equipment | PBI-041 |
| UC-09 Iniciar | RN-032..034 | App do técnico | Fluxo do técnico (passo 6) | Inspection | PBI-034 |
| UC-10 Responder checklist | RN-035..041 | Checklist dinâmico | Fluxo do técnico (passo 7) | InspectionItemSnapshot, InspectionResponse | PBI-035..039 |
| UC-11 Registrar evidência | RN-045..051 | Captura de evidências | Fluxo do técnico (passo 8) | Evidence | PBI-042..044 |
| UC-12 Registrar NC | RN-052..057 | Registro de NC | Fluxo do técnico (passo 8) | NonConformity | PBI-046 |
| UC-13 Concluir offline | RN-037, 042, 043 | Conclusão de inspeção | Fluxo offline (conclusão) | Inspection | PBI-040 |
| UC-14 Sincronizar | RN-065..078 | Sincronização (outbox) | Fluxo offline (envio da outbox) | SyncOperation, SyncMetadata | PBI-048..055 |
| UC-16/17 Revisar/decidir | RN-079..085 | Revisão/aprovação/reprovação | Fluxo administrativo (passos 9-10), Fluxo de correção | InspectionReview | PBI-056..063 |
| — | RN-086..090 | Auditoria de eventos | (transversal a todos) | AuditEvent | PBI-063 |

### Achados da rastreabilidade
- **Requisitos sem tarefa explícita:** UC-15 (Acompanhar inspeções), UC-18 (Consultar histórico) e UC-19 (Consultar indicadores) não têm PBI individual claramente dedicado no backlog listado (15.3) — **[INFERÊNCIA]** provavelmente cobertos implicitamente por PBI-030 ("acompanhar a inspeção em uma listagem com filtros") e PBI-073 (indicadores, mas classificado como P1).
- **Tarefas sem requisito formal individual:** PBI-001 a PBI-006 (EP-01 — Fundação técnica) não correspondem a nenhum caso de uso específico — são infraestrutura de projeto, o que é esperado e não configura lacuna real.
- **Funcionalidades sem regra de negócio numerada:** Dashboard/indicadores (P1) e Notificações (P1) não têm RN-* dedicadas — coerente com seu status de funcionalidade futura, não still uma lacuna crítica.
- **Entidades sem uso aparente fora do modelo:** `EquipmentType` (Anexo D) não é referenciada por nenhuma regra de negócio, caso de uso ou endpoint de API — reforça que é uma inconsistência a ser resolvida (seção 13.1), não uma entidade "esquecida" de utilização.

---

## 18. Perguntas que precisam ser respondidas

(Consolidado da seção 14, para referência rápida, em ordem de prioridade)

1. Qual documento é a fonte de verdade do modelo de dados: página 10 ou Anexo D?
2. Qual será o contrato final de sincronização (endpoint único vs. separados)?
3. Quais são os limites técnicos concretos (tamanho/formato de arquivo, duração de tokens)?
4. Como funciona o primeiro acesso e a recuperação de senha do técnico?
5. Existe SLA para o ciclo de revisão do supervisor?
6. Como é definida formalmente a "ação administrativa" em conflitos complexos de sincronização?
7. Os nomes de estado devem ser padronizados oficialmente (inglês no back-end, tradução só na UI)?
8. Há necessidade de tratamento de conformidade com LGPD para localização e fotos capturadas?
9. Quem tem permissão para excluir uma evidência já sincronizada — apenas o técnico autor, ou também supervisores?
10. O que ocorre quando uma sincronização atinge o número máximo de tentativas falhas (`attempt_count`)?

---

## 19. Recomendações

1. **Reconciliar o modelo de dados** — escolher entre o texto (página 10) e o diagrama (Anexo D) como fonte de verdade, atualizando o outro documento em conformidade, antes do início do EP-04 (Sprint 3).
2. **Fechar o contrato de sincronização** formalmente no OpenAPI antes da Sprint 6 (EP-08), eliminando a ambiguidade atualmente registrada em 12.15.
3. **Corrigir a matriz de permissões (05.2)** para refletir com precisão que "Supervisor responder checklist" está fora do MVP, evitando a leitura de "Opcional" como algo implementável.
4. **Detalhar limites técnicos concretos** (tamanho de arquivo, formatos aceitos, duração de tokens) em uma seção dedicada de "parâmetros de configuração", hoje ausente.
5. **Formalizar o processo de recuperação de senha e primeiro acesso**, especialmente pensando no técnico de campo sem acesso fácil a e-mail corporativo.
6. **Adicionar critérios de aceitação e regras de negócio míninas para as funcionalidades P1 mais prováveis de avançar** (dashboard, reatribuição de técnico), já que a reatribuição hoje conflita levemente com RN-031 (seção 13.2).
7. **Registrar explicitamente o tratamento de dados pessoais (LGPD)** como "fora do escopo do MVP" (se for o caso), para que a ausência seja uma decisão deliberada e documentada, não uma lacuna silenciosa.
8. **Formalizar o backlog em um Notion Database real** (a página 15.1 já sugere a estrutura de propriedades) para permitir rastreamento real de status, em vez de apenas texto estático — isso destravaria acompanhamento real de progresso ao longo das 16 semanas.
9. **Antecipar tentativas de build/deploy** (Android e contêiner da API) a partir da Sprint 6-7, dado que é um requisito do DoD do MVP (18.6) e risco identificado na seção 15.
10. **Manter a ordem de corte de escopo já definida (16.7)** como plano de contingência ativo, revisando-a a cada marco (M1-M8) em vez de apenas no fim do projeto.

---

## 20. Resumo Final

O workspace **"Projeto FieldOps"** documenta, de forma extensa e majoritariamente bem estruturada, a especificação de um **projeto acadêmico** de plataforma de inspeções técnicas em campo, composta por app mobile (Expo/React Native), interface administrativa (Angular) e API REST (Spring Boot/PostgreSQL), com forte ênfase em **operação offline-first** e **sincronização idempotente** — o núcleo de complexidade técnica e pedagógica do projeto.

A documentação é **rica em regras de negócio (90 regras numeradas)**, casos de uso detalhados, critérios de aceitação em formato Gherkin, definição de pronto por camada e rubricas de avaliação — um nível de detalhe pedagógico incomum e valioso. O maior ponto de atenção é a **divergência entre o modelo de dados textual e o diagrama de classes (Anexo D)**, que precisa ser reconciliada antes da implementação, sob risco de retrabalho de schema a meio do cronograma de 16 semanas. Não há evidência, no conteúdo lido, de que o sistema tenha sido implementado, testado ou executado — este é um documento de especificação, não um retrato de sistema em produção.