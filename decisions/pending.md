# Pendências consolidadas do FieldOps

Atualizado em 2026-09-06. Este é o arquivo único para registrar pendências ainda abertas. Decisões aprovadas permanecem em `architecture/decisions.md`; este arquivo registra o que ainda precisa de detalhamento, implementação ou evidência de aceite.

## GAP-013 — evidências e upload

O backend já possui upload real e grava o arquivo fora do banco. No ambiente local, o arquivo é salvo em disco; em produção, o storage S3 já pode ser selecionado por configuração. Entretanto, o GAP continua aberto porque o requisito do MVP exige mais do que a existência do método de upload:

- validar extensão, MIME real e limite de 10 MB por arquivo;
- limitar a 10 evidências por inspeção;
- aceitar somente JPEG/JPG, PNG e PDF, conforme DEC-004;
- oferecer download/visualização autorizado pela API;
- garantir consistência entre o arquivo e a transação/metadados do banco;
- definir comportamento de falha, retry e limpeza de arquivo órfão.

O que já existe não deve ser descrito como “visualização de fotos concluída”: o endpoint atual retorna metadados, e o fluxo completo de acesso ao conteúdo ainda precisa ser implementado e validado.

## GAP-015 — recuperação de senha

Refresh rotativo, `jti`, revogação server-side, logout, alteração de senha e CORS configurável já foram implementados. Ainda falta o fluxo de recuperação de senha previsto em DEC-005:

- endpoint para solicitar recuperação sem revelar se o e-mail existe;
- token único, armazenado de forma segura e expirando em 30 minutos;
- envio por provedor de e-mail configurado externamente;
- endpoint para redefinir a senha usando o token;
- invalidação de todas as sessões após a redefinição;
- política para senha temporária e troca obrigatória no primeiro acesso.

## GAP-019 e DEC-013 — sincronização mobile

A direção de push/pull foi aprovada, mas os endpoints ainda não existem no backend. Antes da implementação, ainda precisam ser fechados:

- esquema JSON final de push e pull;
- paginação, tamanho máximo de lote e limites de payload;
- retenção de `operationId` e política de limpeza;
- backoff, limite de tentativas e estados de retry;
- catálogo final de códigos de conflito;
- autenticação e identificação do dispositivo;
- estratégia de upload privado associada à sincronização;
- testes de replay, idempotência, conflito, dependência e falha parcial.

## GAP-020 — cronograma e governança

O compromisso vigente é entrega acadêmica até o fim de novembro de 2026. Ainda falta evidência de um calendário operacional único, com:

- marcos de desenvolvimento, homologação e demonstração;
- responsáveis por aceite técnico e de produto;
- critério para considerar TAP, EAP e cronograma encerrados;
- orçamento, conta AWS e ambientes efetivamente disponíveis.

## GAP-021 e GAP-022 — documentação e catálogos

Permanecem pendências de governança documental:

- revisar referências históricas e navegação sem tratá-las como requisitos atuais;
- decidir a política de versionamento para `specs/` e demais documentos;
- mapear os 82 PBIs para cards derivados, caso seja necessária equivalência;
- registrar claramente quais catálogos são históricos, derivados ou vigentes.

## Infraestrutura AWS e operação

AWS é requisito do projeto, mas a arquitetura ainda é proposta. Faltam decisões operacionais e evidências de execução:

- serviços escolhidos entre ECR/ECS e alternativa equivalente;
- região, conta, IaC e orçamento;
- ambientes local, homologação e produção;
- bucket S3 privado, lifecycle, criptografia e política de acesso;
- Secrets Manager/IAM Role;
- HTTPS, logs, métricas, health check, backup e restauração testada;
- pipeline de teste, build, migração controlada e deploy.

## Qualidade e aceite

Ainda faltam evidências para concluir o aceite do produto:

- testes frontend e auditoria WCAG 2.1 AA;
- teste mobile de SQLite/outbox e permissões negadas;
- teste de carga para p95 de API ≤ 500 ms, excluindo transferências;
- inspeção de logs estruturados e correlation ID em ambiente executando;
- demonstração ponta a ponta com evidência de aceite;
- matriz completa requisito → aceite → teste → evidência.

## Itens já definidos e não pendentes de decisão

Não são novas perguntas de produto: autorização por responsável, correção pós-reprovação, GPS/QR, formatos e limites de evidência, política de conflitos, prazo de revisão, dashboard mínimo, AWS como requisito e entrega até novembro já estão decididos. O que permanece aberto nesses temas é implementação, detalhamento técnico ou evidência de aceite, conforme as seções acima.
