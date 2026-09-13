# Proposta de decisões operacionais para entrega do MVP

**Data:** 2026-09-12  
**Status:** Proposed — recomendações recebidas, aguardando aprovação formal e preenchimento dos valores operacionais.  
**Relação:** [ADR-009 aprovado](../_KB/12-decisions/ADR-009-mvp-cross-app-integration.md), [plano integrado](../_KB/11-planning/mvp-integration-plan.md) e [plano de infraestrutura](../_KB/08-infrastructure/deployment-plan.md). A aprovação de D01–D08 não preenche os valores operacionais abaixo.

## AWS-01 — Conta, região, orçamento e ambientes

Proposta: uma conta AWS responsável, uma região principal e um limite mensal de custo formalizados; ambientes local, homologação e produção/demonstração com configuração e segredos separados. Sempre que possível, usar a mesma região para RDS, ECS, ECR, S3, SES e observabilidade. Credenciais nunca são versionadas.

**Valores ainda não informados:** identificador/nome da conta e responsável; região; teto mensal e responsável por acompanhar custo; domínios; distinção efetiva entre homologação e produção/demonstração. Esses valores precisam constar do registro aprovado antes de provisionar recursos.

## AWS-02 — Infrastructure as Code

Proposta: Terraform como fonte de verdade, com módulos de rede, IAM, RDS, S3, ECR, ECS, ALB e observabilidade; ambientes `homolog` e `production`; estado remoto, revisão de `plan`, variáveis por ambiente e nenhum segredo em `.tfvars` versionado. Estrutura sugerida: `infra/modules/<componente>` e `infra/environments/<ambiente>`.

**Detalhes a fechar:** backend do estado remoto e locking, responsáveis por `plan/apply`, conta/ambiente de bootstrap, política de aprovação, estratégia de rollback e tratamento de recursos existentes. Ferramenta proposta não significa infraestrutura já criada.

## OPS-01 — Gates e responsáveis

Proposta de três gates:

1. **Aceite técnico:** Backend, Frontend, Mobile e Validation & QA verificam testes, builds, contratos, lint/typecheck, segurança básica e ausência de regressão P0 conhecida.
2. **Homologação integrada:** ADMIN cadastra → SUPERVISOR agenda → técnico recebe/trabalha offline/sincroniza → supervisor revisa → decisão volta ao mobile, com evidência de navegador, dispositivo, API, logs, banco e storage.
3. **Aceite do MVP:** responsável de produto/PO valida requisitos, rastreabilidade, jornada, pendências aceitas, riscos e documentação final.

**Nomeações e marcos ainda não informados:** pessoas responsáveis por cada gate, autoridade de aprovação de produto, datas de freeze/homologação/demonstração e formato/local da evidência. O compromisso vigente continua sendo entrega até o fim de novembro de 2026.

## SES-01 — Identidade autorizada

Proposta: Amazon SES em homologação/produção; desenvolvimento usa provider `noop` ou equivalente enquanto não há conta autorizada. Homologação/produção exigem identidade/domínio verificado, remetente configurado, role com `ses:SendEmail`, região compatível e saída da sandbox quando necessária. A limitação da role do AWS Academy não altera a arquitetura oficial.

**Valores ainda não informados:** domínio/remetente, conta e região SES, responsável pela verificação e condição de sandbox. Recuperação de senha em ambiente publicado só pode ser aceita após teste real de entrega.

## Critério de fechamento operacional

As recomendações podem orientar preparo e estimativas; infraestrutura e gates não são marcados `DONE` até haver valores, aprovação e evidência. O aceite integrado exige os 17 passos do [critério final do plano](../_KB/11-planning/mvp-integration-plan.md): da preparação pelo ADMIN à preservação de pendências, falhas e conflitos no mobile após decisão do supervisor.
