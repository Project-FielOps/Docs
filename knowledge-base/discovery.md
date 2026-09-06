# Diagnóstico da documentação FieldOps

Data da análise: 2026-09-05. Fases 1 e 2 realizadas antes da reestruturação.

## Método e limites

Foram lidos os 23 capítulos e anexos Markdown de `base_fieldops`, seu índice, o diagrama de classes, os documentos existentes em `Docs`, o texto do TAP Word, os READMEs das aplicações, `AGENTS.md` e a análise secundária `workflow_cards_github/base.md`. O TAP foi extraído para leitura de conteúdo; não houve alteração nem avaliação de sua diagramação.

O cruzamento técnico incluiu controllers, serviços de inspeção, modelos, autenticação, evidências, não conformidades, revisão e auditoria, DTOs, migração inicial, configurações, testes existentes, sessão e planejamento Angular. Trata-se de inspeção estática: existência de código ou teste não comprova funcionamento, aprovação acadêmica ou implantação. Não foi consultado o board remoto, nem o vídeo externo do índice; não foram usados como evidência.

## Informações bem definidas

- Problema: fragmentação de formulários, fotografias e mensagens entre planejamento, execução e revisão.
- Produto: três aplicações; painel administrativo real, API central e mobile Android com execução offline após download.
- Operação inicial de uma organização prestadora para vários clientes, sem multiempresa no MVP.
- Cliente → local → equipamento; versão publicada → inspeção → snapshot → resposta; revisões sucessivas e auditoria.
- Um técnico responsável, modelos versionados, snapshot preservado, reprovação com motivo e proteção após aprovação.
- A base contém 90 RN, 12 RNF, 20 UC, 82 PBI (72 P0 e 10 P1), critérios de aceitação por domínio e DoD.
- Java 21/Spring Boot/PostgreSQL e Angular já possuem código. Mobile, contrato de sync e infraestrutura publicada não foram encontrados no workspace.

## Informações superficiais

- “Escopo do supervisor” não define se consulta e planejamento são globais ou vinculados ao responsável.
- Validação por tipo não define formato de alternativas, limites numéricos, tamanho de texto nem semântica de não aplicável.
- Correção preserva histórico na regra, mas não define quais itens podem ser liberados nem como representar revisões de respostas.
- QR divergente e GPS negado remetem a uma política da inspeção que não foi modelada.
- Requisitos de desempenho e acessibilidade não estabelecem carga, dispositivos ou metas mensuráveis.
- Política de retenção, primeiro acesso, recuperação de senha e sessão offline não está fechada.

## Informações ausentes

- Contrato único de sync: payload final, cursores, idempotência persistida no servidor, retenção de chaves, conflitos e tentativas.
- Critérios de validação de upload além do MIME informado, download autorizado de arquivo e consistência entre arquivo e transação de banco.
- Evidência atual de testes ponta a ponta, pipeline, deploy, restauração de backup e aprovação do TAP.
- Matriz completa de objetivo → requisito → aceite → teste e vínculo entre PBI do produto, cards derivados e EAP.
- Responsáveis técnicos por decisão, orçamento aprovado, ambientes publicados e calendário confirmado.

## Contradições e desvios encontrados

| ID | Evidências em conflito | Impacto e tratamento |
|---|---|---|
| GAP-001 | Página 05 e RN-033/RN-079 restringem execução/revisão; `InspectionService` e controllers admitem ADMIN | Matriz definitiva depende de DEC-002; não legitimar exceção apenas porque existe no código |
| GAP-002 | RN-004 restringe consulta ao técnico; GET de inspeções/respostas/evidências/revisões não verifica atribuição | Desvio de autorização observado; precisa de correção e teste de acesso por objeto |
| GAP-003 | Página 07 reabre REJECTED → IN_PROGRESS; API edita e submete diretamente em REJECTED; DRAFT existe no enum sem fluxo de criação | DEC-003 fecha estados e correção antes de contrato mobile |
| GAP-004 | RN-083 exige histórico anterior; upsert sobrescreve resposta e auditoria é chamada sem valores anteriores/novos | Histórico de decisões existe; histórico de conteúdo não está demonstrado |
| GAP-005 | Texto usa SUPERVISOR/TECHNICIAN e ASSIGNED; diagrama usa MANAGER/INSPECTOR/REVIEWER e omite ASSIGNED | DEC-001 reconcilia modelo e mantém diferenças visíveis |
| GAP-006 | Diagrama adiciona EquipmentType, TemplateItemOption e entidades de tratamento de NC; texto usa opções JSON e registro básico de NC | Não gerar schema extra sem decisão de produto |
| GAP-007 | Ficha tem vídeo, documento, plano de ação, assinatura, cálculo automático e aprovação com restrições | São exemplos, não requisitos confirmados do MVP; ficha também situa snapshot na abertura, texto no agendamento |
| GAP-008 | Dashboard P1 em páginas 08/15 e mínimo MVP em 14.5; já há código de dashboard | DEC-009 define obrigação de entrega sem confundir existência com prioridade |
| GAP-009 | API REST de origem usa resposta por responseId e POST responses:batch; código usa item do snapshot e PUT da coleção | Documentar contrato observado separadamente do contrato pretendido |
| GAP-010 | Exemplo de login inclui user/expiresIn; código retorna tokenType/expiresInSeconds e busca usuário em /auth/me | Corrigir instrução de integração; exemplo não é contrato vigente |
| GAP-011 | Base e Angular usam fieldErrors/requestId; API usa details/error, sem requestId no erro | Erros por campo podem não aparecer; correlação prevista não foi implementada |
| GAP-012 | Planejamento Angular busca usuários por /users; controller restringe toda a rota a ADMIN | Supervisor pode não carregar técnicos/supervisores; propor consulta mínima de atribuíveis sem abrir gestão de usuários |
| GAP-013 | EAP/cronograma diziam que upload real não existia; EvidenceService já grava fora do banco | Implementado e validado: tipos, MIME real, limites, download autorizado e compensação storage/banco |
| GAP-014 | Resumos tratam object storage como atual; FileStorageService usa disco local | S3/AWS é proposta, não infraestrutura instalada |
| GAP-015 | RN-008 e segurança pretendem invalidação/restrição; JWT logout não revoga, filtro de acesso não verifica status ativo, CORS aceita qualquer origem | Documentar lacunas; não chamar segurança concluída com base no login |
| GAP-016 | RN-036 valida por tipo; AnswerUpsertRequest e upsert aceitam campos sem compatibilidade com snapshot | Testes de obrigatoriedade não cobrem validação tipada |
| GAP-017 | RN-055 exige descrição/evidência crítica; DTO permite descrição ausente e serviço não valida evidência, estado ou atribuição na criação de NC | Desvio de integridade e autorização; não inventar fechamento automático da NC |
| GAP-018 | Proteção de aprovadas; upload permite exceção ADMIN, NC aceita alterações sem bloqueio por estado | Imutabilidade ainda parcial; testar todas as entidades dependentes |
| GAP-019 | EAP diz modelo/integração concluídos; testes cobrem recortes, não mobile, upload completo ou toda a autorização | Manter status históricos com ressalva temporal; não reproduzir como conclusão atual |
| GAP-020 | Cronograma registra TAP aprovado em julho e encerramento 09/10; TAP de 15/08 tem aprovação em branco e prazo de 16 semanas | DEC-008 requer confirmação do calendário e governança |
| GAP-021 | README aponta TAPs Markdown e base.md inexistentes; specs é ignorado pelo Git | Corrigir navegação e versionar conhecimento durável |
| GAP-022 | Resumo cita 80 cards derivados; produto tem 82 PBI | São catálogos diferentes; não renumerar nem declarar equivalência sem mapeamento |
| GAP-023 | TemplateService valida seção do caminho, mas updateItem/deleteItem não conferem vínculo do item com ela | Risco de alterar item de outra seção/versão; teste de caminho cruzado necessário |
| GAP-024 | Publicação desabilita versões antigas; agendamento com número explícito valida publicação, sem validar activeForNewInspections | Escolha de versões antigas e inativação requerem regra consistente |
| GAP-025 | Filtros do painel incluem prioridade/período; controller não os recebe e serviço escolhe filtros por precedência | Acompanhamento não demonstra todos os filtros especificados |

## Decisões pendentes e perguntas prioritárias

As decisões iniciais e DEC-004 a DEC-012 foram respondidas em 2026-09-05/06; estão registradas em [decisões](../architecture/decisions.md). As perguntas futuras, propostas e impactos ficam em [decisões pendentes](../decisions/pending.md). Respostas futuras devem ser adicionadas como decisão datada, não inferidas do código.

## Riscos

| Risco | Evidência | Mitigação proposta |
|---|---|---|
| Acesso indevido e quebra de histórico | GAP-001/002/004/015/018/023 | Testes de autorização por objeto e proteção de todas as entidades dependentes antes da demonstração |
| Retrabalho mobile/API | GAP-003/009 e sync indefinido | Fechar contratos com casos de falha e replay antes da implementação dependente |
| Perda de dados ou duplicidade | Outbox apenas planejada; upload sem operação idempotente | Testar reinício, falha parcial, reenvio e cancelamento remoto |
| Escopo maior que capacidade | 72 PBIs P0, mobile ausente e calendários distintos | Confirmar marcos reais; preservar fluxo vertical e adiar extras conforme base |
| Documentação falsa de conclusão | GAP-019/020 | Distinguir plano, implementação encontrada, teste executado e aceite confirmado |
| Arquivos órfãos ou indisponíveis | Escrita/remoção em disco não participa da transação SQL | Especificar confirmação e recuperação de arquivos antes do deploy |

Na análise inicial não foram estimadas probabilidades, horas ou responsáveis sem evidência, nem feitas alterações no código das aplicações.

## Revalidação das correções — 2026-09-06

Os GAPs abaixo foram corrigidos no backend e revisados pelo fluxo independente de Backend Agent → Validation & QA Agent. A suíte `mvn test` passou com Java 21, PostgreSQL 16 em Testcontainers e 10 testes executados. A evidência detalhada está em [validation-qa-2026-09-06.md](validation-qa-2026-09-06.md).

| GAP | Estado validado | Evidência principal |
|---|---|---|
| GAP-001 | Implementado corretamente | ADMIN não inicia, responde, revisa, aprova ou reprova; vínculos são exigidos no serviço |
| GAP-002 | Implementado corretamente | Leituras de inspeção, respostas, revisões, evidências e NCs verificam papel/vínculo e escopo do caminho |
| GAP-003 | Implementado corretamente | `REJECTED` exige `POST /reopen-correction` antes de resposta ou submissão |
| GAP-004 | Implementado corretamente | Atualizações de resposta registram valores anterior/novo em auditoria sem conteúdo sensível adicional |
| GAP-011 | Implementado corretamente | Erros expõem `requestId` e `fieldErrors` conforme contrato canônico |
| GAP-012 | Implementado corretamente | `GET /api/v1/users/assignable` retorna somente técnicos ativos para ADMIN/SUPERVISOR; gestão permanece ADMIN-only |
| GAP-014 | Implementado corretamente | Storage selecionável por configuração: disco local no desenvolvimento e S3 privado em produção, com bucket/região externos e credenciais AWS padrão |
| GAP-013 | Implementado corretamente | JPEG/JPG, PNG e PDF com assinatura binária, 10 MB/arquivo, 10/inspeção, download autorizado e compensação transacional; `EvidenceServiceTest` e suíte completa passaram |
| GAP-015 | Implementado corretamente | Recuperação por token opaco com hash SHA-256, 30 minutos, uso único, Amazon SES configurável, resposta anti-enumeração, reset transacional com revogação de sessões e senha temporária obrigatória |
| GAP-016 | Implementado corretamente | Campos de resposta incompatíveis com `responseType` são rejeitados |
| GAP-017 | Implementado corretamente | NC crítica exige descrição e evidência antes de submissão; vínculos são conferidos |
| GAP-018 | Implementado corretamente | Exceção ADMIN removida e NC/evidência ficam imutáveis em inspeção aprovada/cancelada |
| GAP-019 | Parcialmente implementado | Backend possui push/pull, idempotência, cursor e testes de integração; SQLite/outbox mobile e testes no dispositivo ainda não estão neste workspace |
| GAP-023 | Implementado corretamente | Item só pode ser atualizado/removido pela seção a que pertence |
| GAP-024 | Implementado corretamente | Agendamento rejeita versão publicada não ativa para novas inspeções |
| GAP-025 | Implementado corretamente | Listagem aceita prioridade e intervalo de período, preservando escopo do ator |

GAP-005..010 e GAP-020..022 permanecem documentais, históricos, de frontend ou dependentes de decisão/infraestrutura. GAP-012, GAP-013, GAP-014 e GAP-015 foram implementados conforme decisões aprovadas em 2026-09-06. GAP-019 permanece parcial apenas pela ausência do mobile neste workspace; o backend de sync e seus cenários de replay, idempotência, cursor, conflito e retry foram implementados e validados.

## Revalidação do item 4 — 2026-09-06

O backend implementa health check público, correlation ID seguro no header `X-Request-ID`, propagação para `ApiError` e MDC dos logs. Os cenários foram cobertos por testes de integração e a suíte limpa passou com 23 testes; o empacotamento também passou. A medição de p95 sob carga, inspeção em ambiente AWS e demonstração ponta a ponta permanecem evidências operacionais pendentes, conforme registrado em `decisions/pending.md`.
