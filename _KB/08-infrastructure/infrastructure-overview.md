# Infraestrutura — índice

O documento operacional da infraestrutura é [Arquitetura AWS](../../architecture/aws.md). A sequência executável está em [Plano de implantação](../../engineering/deployment-plan.md) e a automação em [CI/CD](../../engineering/cicd.md).

Este arquivo mantém apenas o ponto de entrada da base de conhecimento para evitar duas descrições divergentes da mesma topologia. A arquitetura definida usa Route 53, ACM, ALB, ECS Fargate, RDS PostgreSQL, S3, Secrets Manager, CloudWatch, ECR e GitHub Actions. O frontend tem hospedagem recomendada em S3 privado + CloudFront.

Estado: desenho definido; recursos AWS e IaC ainda exigem evidência de provisionamento.
