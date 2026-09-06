# ADR-005 — AWS e storage em objeto

## Status
Accepted

## Context

A documentação original e a arquitetura do projeto exigem AWS e storage em objeto como requisito obrigatório. Porém, a infraestrutura real não está implantada neste workspace, e o storage local pode estar sendo usado apenas para evolução inicial.

## Decision

AWS e storage em objeto são requisitos obrigatórios do projeto. Sua implementação física e configuração operacional devem ser tratadas como parte da infraestrutura do produto, não como opção casual.

## Alternatives Considered

- tratar AWS como desenho opcional;
- assumir que o storage local substitui a arquitetura oficial;
- ignorar a exigência da base documental.

## Consequences

- a solução precisa ser planejada e validada em termos de cloud, storage e disponibilidade;
- a estrutura de evidências e upload precisa seguir o modelo de objeto;
- a infraestrutura física deve ser documentada com status explícito e evidência.

## Implementation Notes

- Recomendado usar AWS S3 como backend de objetos.
- Use AWS SDK for Java v2 com `DefaultCredentialsProvider` para que ambientes AWS (EC2, ECS, EKS ou AWS Academy) assumam permissões por IAM Role automaticamente.
- Em desenvolvimento local e CI, pode-se usar credenciais via variáveis de ambiente ou `~/.aws/credentials`, ou emular com LocalStack para testes.
- Configurações esperadas (exemplo):
	- `evidence.storage.type=s3`
	- `aws.region=sa-east-1`
	- `aws.s3.bucket=fieldops-evidence`
	- `aws.s3.key-prefix=evidence/`
- Princípio de menor privilégio: role com permissões estritas `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject` no prefixo necessário; considerar encriptação em repouso (SSE-S3 ou KMS).

## Rationale

Adotar IAM Role + `DefaultCredentialsProvider` garante que a solução seja segura em ambiente AWS e simplifica credenciais para desenvolvedores e runners gerenciados pela plataforma (ex.: AWS Academy).

## Related Components

- evidence storage;
- infrastructure;
- upload and download flow;
- cloud architecture.

## Related Requirements

- evidência e upload;
- object storage;
- infraestrutura do produto.

## References

- [../../architecture/aws.md](../../architecture/aws.md)
- [../../references/base_fieldops/Projeto FieldOps](../../references/base_fieldops/Projeto%20FieldOps)
