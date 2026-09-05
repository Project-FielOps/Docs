# Arquitetura AWS proposta

## Status

AWS é obrigatória por decisão de 2026-09-05. Esta topologia continua uma proposta de referência: não existe infraestrutura AWS ou IaC configurada no workspace; nomes, regiões, custos e contas precisam de decisão do time antes de provisionamento.

## Topologia proposta

```text
CloudFront ── S3 (Angular estático)
      │
      └── ALB ── ECS Fargate (API Spring Boot) ── RDS PostgreSQL
                         │                 └──── S3 (evidências privadas)
                         └── CloudWatch Logs/Metrics
Secrets Manager ─────────┘
```

O mobile acessa CloudFront/ALB por HTTPS. Evidências são enviadas à API, que valida a autorização e grava metadados no banco. Para uploads maiores, a API pode emitir URL pré-assinada curta; o bucket continua privado e a autorização é verificada antes de emitir a URL.

## Ambientes e segurança

Manter `local`, `integration`, `demo` e `production` logicamente separados, com variáveis e segredos próprios. Em local, usar Docker Compose para PostgreSQL. Em nuvem, secrets ficam no Secrets Manager e são injetados no serviço; nunca entram em repositório, imagem ou bundle Angular. RDS não é público. O ALB força HTTPS e CORS aceita apenas origens do painel autorizadas.

## Operação

CloudWatch reúne logs estruturados, métricas de erro/latência e alarmes de saúde. RDS tem backup automatizado; S3 usa criptografia e regra de ciclo de vida definida pela política de retenção. O deploy deve executar teste, build, migração controlada e verificação de saúde. Escolha ECR/ECS e CDK ou Terraform antes de criar automação, e registre a decisão em `decisions.md`.
