# CI/CD do FieldOps

## Objetivo

Automatizar GitHub → validação → artefato → ECR/S3 → ECS/CloudFront, bloqueando publicação quando testes ou build falharem.

## Fluxos

### API

```text
checkout → Java 21/Maven → mvn clean test → package → docker build
→ scan → OIDC AWS → ECR → task definition → ECS → smoke test
```

O workflow deve publicar a imagem com SHA do commit, aguardar o ECS ficar estável e testar HTTPS, health, autenticação e uma chamada autorizada. `latest` não deve ser usado.

### Frontend

```text
checkout → Node → npm ci → testes → build → S3 → invalidação CloudFront → smoke test
```

O bundle não pode conter secrets, credenciais AWS, JWT ou senhas.

## Branches e promoção

- Pull request: testes e build, sem deploy.
- `develop`: deploy no ambiente de desenvolvimento/homologação após aprovação definida.
- `main`/tag: produção com aprovação manual do environment GitHub.
- API e frontend podem ter workflows separados e versões independentes.

## AWS e segredos

Usar OIDC do GitHub Actions para assumir uma IAM Role de escopo mínimo. Não usar access keys permanentes se OIDC estiver disponível. Secrets de runtime ficam no Secrets Manager e são referenciados na task definition. `.env` local nunca é commitado.

## Teste local antes da AWS

```bash
cd API
docker compose up -d
mvn clean test
mvn clean package

cd ../interface-administrativa
npm ci
npm test -- --watch=false
npm run build
```

Depois, subir a API e o frontend localmente e validar login, refresh/logout, autorização, migrations, S3 (quando habilitado) e jornadas críticas no navegador. SES pode usar `noop` ou identidade previamente provisionada.

## Smoke test pós-deploy

1. DNS e certificado HTTPS corretos.
2. `/actuator/health` retorna `UP` sem segredos.
3. Login, refresh e logout funcionam.
4. Usuário sem vínculo recebe 403.
5. Evidências respeitam autorização.
6. Logs chegam ao CloudWatch com correlation ID e sem dados sensíveis.
7. Rollback e alarmes foram testados.

## Estado atual

Este documento define o desenho e a sequência de implementação. O workspace ainda não contém workflow GitHub Actions nem IaC provisionada. Antes de codificar os workflows, confirmar conta, região, domínios, estratégia de IaC e ambiente-alvo.
