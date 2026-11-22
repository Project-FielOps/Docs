# Regras de negócio

Fonte normativa: [capítulo 09](../../references/base_fieldops/Projeto%20FieldOps%203022594f77cf8233a662814f75a0600f.md). Prioridade das regras textuais confirmada pelo responsável nesta sessão em 2026-09-05. Os identificadores RN originais permanecem válidos; BR é um alias, não uma regra nova. Regras que remetem a política ainda indefinida continuam condicionais; consultar [decisões pendentes](../../decisions/pending.md).

## AUTH

### BR-AUTH-001 · RN-001

Somente usuários ativos poderão iniciar uma nova sessão.

### BR-AUTH-002 · RN-002

A inativação de um usuário não excluirá seus registros históricos.

### BR-AUTH-003 · RN-003

A API deverá validar o perfil em todas as operações protegidas.

### BR-AUTH-004 · RN-004

O técnico somente poderá consultar integralmente as inspeções atribuídas a ele.

### BR-AUTH-005 · RN-005

O supervisor poderá consultar e revisar inspeções pertencentes ao seu escopo.

### BR-AUTH-006 · RN-006

O administrador poderá gerenciar usuários e cadastros, mas não deverá alterar respostas de inspeção por padrão.

### BR-AUTH-007 · RN-007

Credenciais e tokens não poderão ser gravados em logs.

### BR-AUTH-008 · RN-008

A troca de senha ou o bloqueio do usuário deverá invalidar sessões conforme a política definida pela API.

## MASTER

### BR-MASTER-009 · RN-009

Todo local deverá pertencer a um cliente.

### BR-MASTER-010 · RN-010

Todo equipamento deverá pertencer a um local no MVP.

### BR-MASTER-011 · RN-011

O código QR de um equipamento deverá ser único.

### BR-MASTER-012 · RN-012

Um equipamento inativo não poderá ser utilizado em uma nova inspeção.

### BR-MASTER-013 · RN-013

Registros já utilizados em inspeções deverão ser inativados, não excluídos fisicamente.

### BR-MASTER-014 · RN-014

O local selecionado para a inspeção deverá ser compatível com o equipamento selecionado.

## TEMPLATE

### BR-TEMPLATE-015 · RN-015

Todo modelo deverá possuir título, categoria e ao menos uma seção com item válido antes da publicação.

### BR-TEMPLATE-016 · RN-016

Todo item deverá possuir um tipo de resposta.

### BR-TEMPLATE-017 · RN-017

A ordem de seções e itens deverá ser explícita.

### BR-TEMPLATE-018 · RN-018

Um modelo em rascunho poderá ser alterado livremente por usuário autorizado.

### BR-TEMPLATE-019 · RN-019

Uma versão publicada não poderá ser alterada de forma destrutiva.

### BR-TEMPLATE-020 · RN-020

Alterações em um modelo publicado deverão gerar nova versão.

### BR-TEMPLATE-021 · RN-021

Uma inspeção deverá preservar um snapshot dos itens da versão utilizada.

### BR-TEMPLATE-022 · RN-022

A desativação de um modelo não afetará inspeções existentes.

### BR-TEMPLATE-023 · RN-023

Tipos de resposta não suportados pelo aplicativo não poderão ser publicados para uso no MVP.

## PLAN

### BR-PLAN-024 · RN-024

Uma inspeção deverá estar vinculada a uma versão publicada de modelo.

### BR-PLAN-025 · RN-025

Uma inspeção deverá possuir cliente, local, técnico e data prevista.

### BR-PLAN-026 · RN-026

O equipamento poderá ser opcional somente quando o tipo de inspeção for definido para local ou ambiente.

### BR-PLAN-027 · RN-027

Somente técnico ativo poderá receber nova atribuição.

### BR-PLAN-028 · RN-028

Uma inspeção atribuída deverá aparecer para o técnico após sincronização.

### BR-PLAN-029 · RN-029

O cancelamento deverá registrar usuário, data e justificativa.

### BR-PLAN-030 · RN-030

Uma inspeção aprovada não poderá ser cancelada pelo fluxo comum.

### BR-PLAN-031 · RN-031

O MVP permitirá somente um técnico responsável por inspeção.

## EXEC

### BR-EXEC-032 · RN-032

Somente inspeções atribuídas poderão ser iniciadas.

### BR-EXEC-033 · RN-033

Somente o técnico responsável poderá iniciar e responder a inspeção, salvo permissão administrativa excepcional não prevista no MVP.

### BR-EXEC-034 · RN-034

O início deverá registrar data e hora do dispositivo e, após sincronização, data e hora de recebimento no servidor.

### BR-EXEC-035 · RN-035

Cada resposta deverá estar vinculada a um item do snapshot da inspeção.

### BR-EXEC-036 · RN-036

Respostas deverão ser validadas conforme seu tipo.

### BR-EXEC-037 · RN-037

Itens obrigatórios deverão ser respondidos antes da conclusão.

### BR-EXEC-038 · RN-038

Quando configurado, uma resposta não conforme deverá exigir observação.

### BR-EXEC-039 · RN-039

Quando configurado como crítico, um item não conforme deverá exigir ao menos uma evidência.

### BR-EXEC-040 · RN-040

O progresso deverá considerar itens respondidos em relação aos itens aplicáveis.

### BR-EXEC-041 · RN-041

O aplicativo deverá salvar localmente cada alteração confirmada.

### BR-EXEC-042 · RN-042

A conclusão deverá registrar o momento real informado pelo dispositivo e o momento de recebimento pelo servidor.

### BR-EXEC-043 · RN-043

Após a conclusão local, as respostas ficarão bloqueadas até eventual reprovação ou falha que permita correção.

### BR-EXEC-044 · RN-044

A inspeção enviada deverá ser revisada antes da aprovação.

## EVIDENCE

### BR-EVIDENCE-045 · RN-045

Toda evidência deverá estar vinculada a uma inspeção e, quando aplicável, a uma resposta ou não conformidade.

### BR-EVIDENCE-046 · RN-046

O sistema aceitará somente formatos e tamanhos permitidos.

### BR-EVIDENCE-047 · RN-047

Uma evidência ainda não sincronizada não poderá ser apagada silenciosamente por limpeza de cache.

### BR-EVIDENCE-048 · RN-048

A exclusão de uma evidência já sincronizada deverá ser auditada e respeitar o estado da inspeção.

### BR-EVIDENCE-049 · RN-049

Evidências de inspeções aprovadas serão somente leitura.

### BR-EVIDENCE-050 · RN-050

O nome original do arquivo não será utilizado como único identificador.

### BR-EVIDENCE-051 · RN-051

Metadados sensíveis desnecessários poderão ser removidos ou ignorados conforme política do produto.

## NC

### BR-NC-052 · RN-052

Toda não conformidade deverá pertencer a uma inspeção.

### BR-NC-053 · RN-053

A não conformidade poderá ser associada a um item específico.

### BR-NC-054 · RN-054

A criticidade deverá assumir um valor válido: baixa, média, alta ou crítica.

### BR-NC-055 · RN-055

Não conformidades críticas deverão exigir descrição e evidência.

### BR-NC-056 · RN-056

A exclusão de uma não conformidade após envio deverá ser substituída por alteração de estado ou operação auditável.

### BR-NC-057 · RN-057

O tratamento completo da não conformidade após a aprovação pertence a uma versão futura, mas seu registro fará parte do MVP.

## NATIVE

### BR-NATIVE-058 · RN-058

O aplicativo deverá solicitar consentimento antes de acessar câmera ou localização.

### BR-NATIVE-059 · RN-059

A recusa da permissão deverá produzir orientação e alternativa quando a regra permitir.

### BR-NATIVE-060 · RN-060

A localização não será coletada continuamente no MVP.

### BR-NATIVE-061 · RN-061

A localização poderá ser registrada no início e na conclusão.

### BR-NATIVE-062 · RN-062

O sistema deverá armazenar latitude, longitude, precisão e horário quando disponíveis.

### BR-NATIVE-063 · RN-063

A leitura de QR Code não deverá, sozinha, autorizar acesso a dados não permitidos ao usuário.

### BR-NATIVE-064 · RN-064

Divergência entre o equipamento previsto e o lido deverá ser informada antes da continuidade.

## SYNC

### BR-SYNC-065 · RN-065

Inspeções previamente baixadas deverão permanecer acessíveis sem conexão.

### BR-SYNC-066 · RN-066

Dados não sincronizados deverão sobreviver ao fechamento e à reabertura do aplicativo.

### BR-SYNC-067 · RN-067

Cada operação enviada deverá possuir identificador idempotente único.

### BR-SYNC-068 · RN-068

O reenvio da mesma operação não poderá criar duplicidade.

### BR-SYNC-069 · RN-069

Operações deverão ser processadas respeitando dependências.

### BR-SYNC-070 · RN-070

Falha em uma operação não deverá apagar outras operações pendentes.

### BR-SYNC-071 · RN-071

O aplicativo deverá exibir a quantidade de operações pendentes.

### BR-SYNC-072 · RN-072

O aplicativo deverá informar data e resultado da última sincronização.

### BR-SYNC-073 · RN-073

O servidor será a fonte oficial após a confirmação da sincronização.

### BR-SYNC-074 · RN-074

O aplicativo deverá preservar a data de execução capturada no dispositivo, além da data de recebimento no servidor.

### BR-SYNC-075 · RN-075

Conflitos deverão ser detectados por versão, data ou outro mecanismo explícito.

### BR-SYNC-076 · RN-076

No MVP, dados aprovados no servidor prevalecerão e não aceitarão atualização tardia comum.

### BR-SYNC-077 · RN-077

Uma inspeção cancelada no servidor não deverá ter seus dados locais pendentes descartados sem aviso.

### BR-SYNC-078 · RN-078

Arquivos poderão ser sincronizados separadamente dos dados estruturados, mantendo rastreamento do estado individual.

## REVIEW

### BR-REVIEW-079 · RN-079

Somente supervisor autorizado poderá iniciar a revisão.

### BR-REVIEW-080 · RN-080

A reprovação deverá possuir motivo.

### BR-REVIEW-081 · RN-081

A aprovação deverá registrar supervisor, data e comentário opcional.

### BR-REVIEW-082 · RN-082

Uma inspeção aprovada ficará bloqueada para edição comum.

### BR-REVIEW-083 · RN-083

Correções após reprovação deverão preservar histórico das versões anteriores.

### BR-REVIEW-084 · RN-084

O supervisor não deverá alterar silenciosamente a resposta fornecida pelo técnico.

### BR-REVIEW-085 · RN-085

Ajustes administrativos deverão ser diferenciados de respostas de campo e auditados.

## AUDIT

### BR-AUDIT-086 · RN-086

Alterações críticas deverão registrar usuário, data, ação e entidade.

### BR-AUDIT-087 · RN-087

Datas de criação e atualização deverão ser mantidas pelo servidor.

### BR-AUDIT-088 · RN-088

Entidades utilizadas em registros históricos não poderão ser excluídas fisicamente pelo fluxo comum.

### BR-AUDIT-089 · RN-089

A API deverá rejeitar transições de estado inválidas.

### BR-AUDIT-090 · RN-090

A documentação da API deverá refletir os estados, validações e erros implementados.
