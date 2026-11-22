# Pendências do backend

Atualizado em 2026-09-06.

O acompanhamento operacional das tasks está em [BACKLOG.md](BACKLOG.md).

## Status geral

Não há GAP funcional de código backend atualmente classificado como aberto. A API possui autenticação, autorização, inspeções, evidências, storage S3, recuperação de senha, sincronização, auditoria e testes automatizados.

As pendências atuais concentram-se em infraestrutura AWS, automação de entrega, validação operacional e integração com as demais aplicações.

## 1. Infraestrutura AWS

Provisionar e validar os componentes definidos na arquitetura:

- ECS Fargate para execução da API;
- RDS PostgreSQL privado;
- S3 privado para evidências;
- Secrets Manager para segredos de runtime;
- Application Load Balancer;
- certificado ACM;
- Route 53/DNS;
- CloudWatch para logs, métricas e alarmes;
- ECR para imagens Docker;
- IAM Roles para ECS e pipeline.

### Critério de conclusão

Cada ambiente deve possuir evidência de recursos provisionados, conexão da API com RDS, acesso autorizado ao S3, secrets carregados, health check do ALB e logs disponíveis no CloudWatch.

## 2. Infraestrutura como código

Definir e implementar uma estratégia de IaC, incluindo:

- ferramenta adotada, como Terraform, CDK ou CloudFormation;
- separação entre local, homologação e produção;
- estado remoto e controle de acesso;
- módulos de rede, ECS, RDS, S3, IAM e observabilidade;
- política de revisão e aprovação das mudanças de infraestrutura.

## 3. CI/CD

Implementar GitHub Actions para:

1. executar testes da API com PostgreSQL/Testcontainers;
2. gerar o pacote Spring Boot;
3. construir e verificar a imagem Docker;
4. publicar a imagem no ECR;
5. atualizar a task definition do ECS;
6. aguardar a estabilização do serviço;
7. executar smoke tests HTTPS, health, autenticação e autorização;
8. permitir rollback para a versão anterior.

O pipeline deve usar OIDC com IAM Role de menor privilégio. Não utilizar access keys permanentes se OIDC estiver disponível.

## 4. Amazon SES

Configurar em uma conta AWS com permissões adequadas:

- identidade ou domínio verificado;
- remetente configurado externamente;
- região compatível;
- IAM Role com `ses:SendEmail`;
- saída da sandbox quando necessária.

No AWS Academy, a role `voclabs` negou `ses:GetAccount`, `ses:TagResource` e `ses:CreateEmailIdentity`. Essa restrição é específica do laboratório. Enquanto ela existir, usar `EmailService=noop` ou uma identidade SES previamente provisionada.

## 5. Validação operacional e qualidade

- executar teste de carga e confirmar p95 da API ≤ 500 ms, sem transferências;
- validar logs estruturados e correlation ID no CloudWatch;
- configurar alarmes de saúde, erro e indisponibilidade;
- testar backup e restauração do RDS;
- validar rollback da API;
- executar demonstração ponta a ponta;
- completar a matriz requisito → teste → evidência.

## 6. Integrações fora do backend

Estes itens dependem de outras frentes e não devem ser tratados como defeitos atuais da API:

- aplicação mobile;
- SQLite/outbox e testes em dispositivo;
- integração mobile com sync e upload em ambiente real;
- testes completos do frontend Angular;
- auditoria WCAG 2.1 AA;
- hospedagem do frontend em S3 + CloudFront, conforme arquitetura recomendada.

## 7. Ordem recomendada

```text
1. definir conta, região, domínios e IaC
2. provisionar rede, RDS, S3, Secrets Manager, ECR e IAM
3. configurar ALB, ACM, Route 53 e CloudWatch
4. publicar manualmente uma primeira imagem validada
5. testar API na AWS
6. implementar GitHub Actions com OIDC
7. automatizar deploy e rollback
8. publicar frontend e executar teste ponta a ponta
```

## Referências

- [Arquitetura AWS](../../08-infrastructure/README.md)
- [Plano de implantação](../../08-infrastructure/deployment-plan.md)
- [Plano de CI/CD](../../09-devops/cicd-plan.md)
- [Decisões pendentes](../../../decisions/pending.md)
- [Diagnóstico](../../99-reference/diagnostics/discovery.md)
