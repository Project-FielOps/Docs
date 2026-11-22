Scaffold: Evidence Storage Adapter
=================================

Resumo
------

Este diretório contém notas e orientação para a implementação do adaptador de storage de evidências usado pela aplicação. O projeto já introduziu a interface `EvidenceStorage` e uma implementação local (`FileStorageService`).

Recomendação (produção)
------------------------

- Backend de objetos: AWS S3.
- Credenciais: preferir IAM Role e `DefaultCredentialsProvider` (AWS SDK v2) — isso é especialmente relevante no contexto AWS Academy.
- Configuração de fallback: variáveis de ambiente ou `~/.aws/credentials` para dev/CI; LocalStack para testes locais.

Configuração esperada
---------------------

- `evidence.storage.type` — valores: `local` (padrão), `s3`.
- Properties relacionados ao S3: `aws.region`, `aws.s3.bucket`, `aws.s3.key-prefix`.

Implementação sugerida para `S3EvidenceStorageService`
-----------------------------------------------------

- Usar AWS SDK v2 (`software.amazon.awssdk:s3`).
- Construir `S3Client` com `S3Client.builder().region(Region.of(region)).credentialsProvider(DefaultCredentialsProvider.create()).build()`.
- Operações mínimas: `store(UUID inspectionId, MultipartFile file)` -> retorna `StoredFile(storageKey, checksum, size, mimeType)`, `delete(String storageKey)`.
- Gerar `storageKey` com prefixo configurável e `inspectionId` + UUID para evitar colisões.
- Calcular checksum (SHA-256) antes do upload quando possível, ou usar ETag como verificação adicional.

Segurança e boas práticas
------------------------

- Role IAM com escopo mínimo (nomes de operações: `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject`).
- Usar KMS se houver requisitos de criptografia administrada.
- Considerar políticas de expiração de objetos (lifecycle) para dados temporários.

Testes
-----

- Emite testes de integração que usam LocalStack ou um bucket de teste isolado.
- Mockar `S3Client` em testes unitários para validar comportamento sem chamada real.

Migração
-------

Ao migrar de `local` para `s3`: garantir migração controlada das evidências existentes, ou manter `local` como fallback para artefatos legados até que uma migração em lote seja executada.

Referências
----------

- ADR-005 — [../../12-decisions/ADR-005-aws-storage.md]
- Código: `API/src/main/java/com/fieldops/evidence` (e.g. `EvidenceStorage`, `FileStorageService`, `S3EvidenceStorageService`)
