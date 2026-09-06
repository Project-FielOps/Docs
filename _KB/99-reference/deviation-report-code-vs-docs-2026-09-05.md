---
generated_by: automation
generated_at: 2026-09-05
status: draft
---

# Relatório de divergências: Código vs Documentação (2026-09-05)

Resumo: varri o código backend em `API/src/main/java` para identificar diferenças relevantes entre o que está documentado na KB e o que está implementado. Abaixo seguem evidências e recomendações.

**Principais divergências encontradas**

- Sincronização mobile: a documentação sugere endpoints dedicados como `/api/v1/mobile/sync/push` e `/api/v1/mobile/sync/pull`, porém **não existe** implementação com esse namespace no backend. O servidor expõe endpoints por recurso (ex.: `PUT /api/v1/inspections/{id}/responses` para batch upsert) que podem atender parte do fluxo, mas não fornecem o contrato de lote/pull/push descrito na KB.
  - Referências de código: [InspectionResponseController](API/src/main/java/com/fieldops/inspection/controller/InspectionResponseController.java), [TemplateController](API/src/main/java/com/fieldops/template/controller/TemplateController.java).

- Armazenamento de evidências: a KB/ADRs indicam uso de objeto (S3) como destino de evidências; o código atual implementa armazenamento local em disco via `FileStorageService` configurado por `EvidenceStorageProperties.evidenceDir()`.
  - Referências de código: [FileStorageService](API/src/main/java/com/fieldops/evidence/application/FileStorageService.java), [EvidenceService](API/src/main/java/com/fieldops/evidence/application/EvidenceService.java).

- Autorização: o projeto usa anotações de método (`@PreAuthorize`) e JWT no fluxo de autenticação, o que cobre autorização por papel (roles). Existem verificações pontuais de autorização por objeto (ex.: somente o técnico atribuído pode anexar/remover evidência), implementadas dentro de services (e.g., `EvidenceService`), mas não existe um padrão uniforme documentado/consistente para autorização por objeto em todos os casos.
  - Referências de código: [SecurityConfig](API/src/main/java/com/fieldops/auth/security/SecurityConfig.java), ocorrências de `@PreAuthorize` em controllers.

- Endpoints documentados vs implementados: a KB apresenta exemplos / contratos de alto nível para várias rotas mobile (ex.: `/api/v1/mobile/inspections`), enquanto o backend implementa endpoints generalistas (ex.: `/api/v1/inspections`, `/api/v1/inspections/{id}/responses`, `/api/v1/inspections/{id}/evidence`). Isso cria ambiguidade sobre qual contrato o mobile deve consumir.
  - Referências de código: [InspectionController](API/src/main/java/com/fieldops/inspection/controller/InspectionController.java), [EvidenceController](API/src/main/java/com/fieldops/evidence/controller/EvidenceController.java).

**Impacto**

- Mobile clients podem não ter um contrato único e simples para sincronização — isto dificulta implementação e testes offline/online consistentes.
- Armazenamento local de evidências requer migração para S3 ou adaptação (adapter) para cumprir ADRs e requisitos de produção.
- Falta de padrão único para autorização por objeto pode gerar inconsistência de segurança e superfícies de erro (alguns fluxos protegem, outros não).

**Evidências (pontos de referência no código)**

- Batch de respostas (pode servir como push): [InspectionResponseController](API/src/main/java/com/fieldops/inspection/controller/InspectionResponseController.java)
- Upload/listagem/remoção de evidências (armazenamento local): [EvidenceController](API/src/main/java/com/fieldops/evidence/controller/EvidenceController.java) e [FileStorageService](API/src/main/java/com/fieldops/evidence/application/FileStorageService.java)
- Verificações de autorização por papel: múltiplos `@PreAuthorize` em controllers (ex.: `UserController`, `TemplateController`, `InspectionController`).
- Verificações de autorização por objeto (ex.: assigned technician): [EvidenceService.upload/delete](API/src/main/java/com/fieldops/evidence/application/EvidenceService.java)
- Configuração de segurança e filtros JWT: [SecurityConfig](API/src/main/java/com/fieldops/auth/security/SecurityConfig.java)

**Recomendações (ações sugeridas)**

1. Definir abordagem imediata para sincronização mobile:
   - Opção A: documentar claramente que o mobile deve usar os endpoints existentes (ex.: batch `PUT /api/v1/inspections/{id}/responses`, `/inspections/{id}/evidence`) e publicar um contrato de cliente (ex.: `Docs/_KB/05-api/specs/synchronization.md`) com exemplos de lote/resultado/cursor; OU
   - Opção B: implementar endpoints dedicados `/api/v1/mobile/sync` (push/pull) que façam orquestração de lote e retornem o formato esperado pela KB.

2. Armazenamento de evidências: implementar um adaptador de storage com implementação baseada em propriedade:
   - `FileStorageService` (atual) permanece para desenvolvimento local;
   - adicionar `S3StorageService` que implemente a mesma interface e ler `spring.profiles`/config para escolha em produção;
   - automatizar migração de arquivos locais para S3 quando necessário.

3. Autorizações e políticas: consolidar um padrão de autorização por objeto:
   - documentar onde o controle por objeto é obrigatório;
   - mover verificações de negócio sensíveis para camada de serviço (já é parcialmente o caso);
   - considerar uso de um componente `AuthorizationService` reutilizável para reduzir duplicação.

4. Atualizar a KB: incluir um trecho em `Docs/_KB/05-api/specs/synchronization.md` referenciando os endpoints reais e alterando o contrato se optarem por usar endpoints existentes.

5. Criar um ticket/ADR (se já não existir) para formalizar as decisões: `ADR-005` (storage) e `ADR-006` (mobile sync) devem refletir a decisão final e o plano de migração.

**Próximos passos que posso executar agora**

- `scaffold-storage-adapter`: gerar esqueleto de `S3StorageService` + interface e testes básicos;
- `update-sync-spec`: atualizar `Docs/_KB/05-api/specs/synchronization.md` para refletir o contrato real do backend;
- `create-adrs`: criar rascunho de ADRs para storage e sync.

Indique qual ação prefere que eu execute.
