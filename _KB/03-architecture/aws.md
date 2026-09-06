# Arquitetura AWS do FieldOps

## Status e escopo

**Status:** arquitetura de referência definida; provisionamento ainda depende de execução e evidência.

Este documento é a fonte de verdade da topologia AWS. Ele descreve responsabilidades e regras, mas não declara que os recursos já existem.

## Topologia definida

```text
Internet → Route 53/DNS → ACM Certificate → Application Load Balancer
                                      HTTPS ↓
                              ECS Fargate — FieldOps API
                                  │       │       │
                                  ▼       ▼       ▼
                            RDS PostgreSQL  S3  Secrets Manager
                                  └─────────┼─────────┘
                                            ▼
                                      CloudWatch

GitHub → GitHub Actions → test → package → docker build → ECR → ECS Fargate
```

### Frontend — complemento recomendado

O desenho acima representa a API. Para o Angular, a opção recomendada é:

```text
GitHub Actions → build Angular → S3 privado → CloudFront → Internet
                                             │
                                             └→ API pelo domínio do ALB
```

O bucket do frontend deve permanecer privado, com Origin Access Control. Se o time optar por servir o Angular em ECS, esta recomendação deve ser substituída por uma decisão explícita.

## Responsabilidade dos serviços

| Serviço | Responsabilidade | Regra principal |
|---|---|---|
| Route 53 | DNS da API e do painel | aponta somente para ALB/CloudFront |
| ACM | certificados TLS | certificado válido para cada domínio |
| ALB | entrada HTTPS e roteamento | HTTP redireciona para HTTPS; ECS não fica público |
| ECS Fargate | execução da API | tasks em subnets privadas; acesso vindo do ALB |
| RDS PostgreSQL | dados transacionais | privado, backup habilitado e sem acesso público |
| S3 | evidências | bucket privado, criptografia e acesso pela task role |
| Secrets Manager | segredos de runtime | nunca no Git, imagem ou bundle |
| CloudWatch | logs, métricas e alarmes | sem senhas, tokens ou credenciais |
| ECR | imagens versionadas | deploy por SHA, nunca por `latest` |

## Fluxo da API

1. Route 53 resolve o domínio.
2. O ALB termina HTTPS com ACM e encaminha para tasks saudáveis.
3. A API lê Secrets Manager e conecta ao RDS privado.
4. A API autoriza o usuário antes de acessar evidências no S3.
5. Logs, métricas e alarmes são enviados ao CloudWatch.

A API continua sendo a autoridade de autorização. S3 não deve ser público; URL pré-assinada só pode ser emitida depois da verificação de vínculo e permissão.

## Ambientes

Cada ambiente possui banco, bucket, secrets, domínio e SES próprios:

| Ambiente | API/banco | Evidências/e-mail |
|---|---|---|
| local | Spring Boot + PostgreSQL Docker | disco local ou S3 de teste; `EmailService=noop` |
| homologação | ECS + RDS | S3 privado e SES com identidade verificada |
| produção | ECS + RDS com backup/alarmes | S3 privado e SES com sandbox liberada quando necessário |

## Regras obrigatórias

- ECS usa IAM Role e o SDK usa a cadeia padrão de credenciais.
- RDS e ECS ficam em subnets privadas; security groups permitem somente os fluxos necessários.
- S3 bloqueia acesso público e usa criptografia.
- CORS, bucket, região, domínio e secrets são configurados por ambiente.
- Flyway é executado de forma controlada antes de liberar uma nova versão.
- Um recurso só é marcado como provisionado após evidência de ARN, endpoint, estado ou log de deploy.

## Evidência SES conhecida

No AWS Academy, a role `voclabs` negou `ses:GetAccount`, `ses:TagResource` e `ses:CreateEmailIdentity`. Isso impede criar/verificar identidade remetente no laboratório, mas não altera SES como provedor oficial. Desenvolvimento pode usar `EmailService=noop` ou identidade previamente provisionada; AWS real exige identidade verificada e IAM Role com `ses:SendEmail`.

## Próximos documentos

- [Plano de implantação](../08-infrastructure/deployment-plan.md)
- [Plano de CI/CD](../09-devops/cicd-plan.md)
- [Arquitetura geral](system-overview.md)
- [Decisões arquiteturais](../12-decisions/architecture-decisions.md)
