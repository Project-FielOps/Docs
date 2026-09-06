# Infrastructure Overview

## Requisitos

- Provisionamento em AWS (requisito obrigatório);
- Storage de objeto para evidências (S3);
- Ambientes: dev, staging, prod;
- IaC recomendada (terraform, cloudformation ou equivalente);
- Observability: logs centralizados e métricas.

## Componentes

- API (EC2/ECS/EKS/whatever);
- PostgreSQL (RDS ou equivalente);
- Storage S3 para evidências;
- CDN/opção de distribuição para downloads de evidências;
- Secrets manager para credenciais;
- CI/CD pipeline para deploy.

## Observação

A infraestrutura precisa ser documentada e provisionada; o workspace atual não contém infraestrutura provisionada.

## Referências

- [../12-decisions/ADR-005-aws-storage.md](../12-decisions/ADR-005-aws-storage.md)
