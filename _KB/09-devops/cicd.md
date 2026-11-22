# CI/CD — índice

O plano único de CI/CD está em [cicd-plan.md](cicd-plan.md). Ele cobre testes locais, GitHub Actions, OIDC, ECR, ECS, S3, CloudFront e smoke tests.

O workflow inicial de CI da API agora existe em `API/.github/workflows/ci.yml` e executa testes, empacotamento, upload do JAR e build da imagem Docker. A execução ainda precisa ser confirmada no GitHub Actions. IaC, CD e os recursos AWS continuam planejados e não devem ser descritos como provisionados.
