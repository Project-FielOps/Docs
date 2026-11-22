# Plano de implantação do FieldOps

Este é um plano executável, não uma confirmação de infraestrutura existente. Marque uma etapa como concluída somente com comando, resultado e evidência registrados.

## 1. Pré-requisitos

- conta AWS, região, domínio e ambientes definidos;
- VPC, subnets e IAM Roles planejadas;
- Docker, Java 21, Maven e Node.js para validação local;
- política de custos, backup, retenção de logs e alarmes aprovada.

## 2. Backend: API, banco e evidências

### B1 — preparar a aplicação

1. Confirmar variáveis de banco, JWT, CORS, storage e SES.
2. Em produção, usar `FIELDOPS_STORAGE_MODE=s3`, bucket privado e `FIELDOPS_MAIL_PROVIDER=ses`.
3. Garantir porta 8080, health check e graceful shutdown.
4. Construir imagem sem secrets e com usuário não-root.

### B2 — provisionar dependências

1. Criar VPC com subnets públicas para ALB e privadas para ECS/RDS.
2. Criar security groups: Internet→ALB 443; ALB→ECS 8080; ECS→RDS 5432.
3. Criar RDS PostgreSQL privado com backup e retenção.
4. Criar S3 privado, bloqueio público, criptografia e lifecycle.
5. Criar secrets no Secrets Manager.
6. Criar ECR, log group e roles de execução/task do ECS.

### B3 — publicar

1. Publicar imagem no ECR com SHA do commit.
2. Criar task definition com CPU/memória, porta, roles, secrets, health check e logs.
3. Criar service Fargate atrás do target group do ALB.
4. Configurar ACM e listener HTTPS, redirecionando HTTP.
5. Configurar Route 53 para o ALB.
6. Executar Flyway contra o RDS de modo controlado.
7. Validar health, login, autorização e upload/download S3.

### Aceite do backend

Task saudável, ALB verde, migration aplicada, RDS conectado, S3 autorizado, secrets carregados, logs no CloudWatch, CORS/HTTPS testados e rollback para imagem anterior validado.

## 3. Frontend Angular

O frontend não acessa RDS/S3 diretamente. A estratégia recomendada é build estático em S3 privado, distribuído pelo CloudFront com Origin Access Control.

### F1 — preparar

1. Fixar `apiUrl` por ambiente sem secrets no bundle.
2. Executar `npm ci`, testes e build de produção.
3. Configurar fallback de rotas Angular.

### F2 — publicar

1. Criar bucket S3 separado por ambiente.
2. Criar CloudFront, OAC, certificado ACM e domínio Route 53.
3. Usar cache curto para `index.html` e longo para assets versionados.
4. Publicar o build e invalidar somente arquivos necessários.

### Aceite do frontend

Validar login, refresh/logout, permissões, CORS, rotas após refresh, upload/download autorizado e configuração correta do ambiente.

## 4. Rollback e evidências

- API: voltar para a task definition/imagem anterior.
- Banco: não usar rollback destrutivo; corrigir com nova migration compatível.
- Frontend: restaurar o artefato estável anterior.
- S3: não apagar evidências automaticamente.

Por ambiente, registrar data, commit, imagem, task definition, endpoint, versão Flyway, health check, alarmes e testes. Nunca registrar secrets ou tokens.
