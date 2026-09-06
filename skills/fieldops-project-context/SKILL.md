---
name: fieldops-project-context
description: Aplica a política documental compartilhada a mudanças no FieldOps que envolvam regras de negócio, contratos, arquitetura, segurança, dados ou integrações. Não use para edições isoladas sem impacto de domínio.
---

# Política compartilhada do FieldOps

Trate documentação e especificação como origem do comportamento esperado. Código e testes são evidência do estado implementado, não autoridade para redefinir requisitos.

Siga esta ordem:

```text
Documentation / Specification
            ↓
     Expected Behavior
            ↓
        Test Cases
            ↓
      Implementation
```

Nunca altere a expectativa ou um teste correto apenas para acomodar a implementação.

## Fontes e precedência

Antes de trabalho de domínio, leia integralmente:

1. `../../knowledge-base/README.md` para autoridade e manutenção;
2. `../../knowledge-base/discovery.md` para estado observado, gaps e conflitos;
3. `../../product/mvp.md`;
4. `../../architecture/decisions.md`;
5. a especificação relevante em `../../specs/`;
6. `../../requirements/traceability.md` e as RN/FR/AC relacionadas.

Consulte arquitetura, engenharia e decisões pendentes conforme o impacto. Use `../../knowledge-base/discovery.md` para distinguir AS-IS de TO-BE. Documentos em `../../references/`, ficha, diagrama, TAP, EAP e cronogramas são históricos quando conflitarem com a base consolidada.

## Vocabulário obrigatório

- `DOCUMENTATION_GAP`: informação necessária ausente ou superficial. Não invente a resposta.
- `DOCUMENTATION_CONFLICT`: fontes relevantes determinam comportamentos incompatíveis. Registre fontes e impacto.
- `DOCS_CHANGE_REQUIRED`: a mudança precisa de decisão ou atualização documental. Informe documento, comportamento atual, esperado, motivo e impacto; não altere silenciosamente.

Se uma lacuna ou conflito permitir implementações materialmente diferentes, interrompa a decisão afetada e solicite definição humana. Continue apenas nas partes não bloqueadas.

## Invariantes

- A API aplica autorização por papel e vínculo, regras e transições.
- No MVP, somente o técnico atribuído executa; somente o supervisor atribuído revisa.
- ADMIN não recebe exceção operacional.
- Versões publicadas e snapshots são imutáveis.
- Inspeção aprovada não aceita edição comum.
- Rejeição volta explicitamente a `IN_PROGRESS` e preserva histórico.
- PostgreSQL é a fonte oficial após sincronização.
- Sync usa push/pull separados, `operationId` idempotente e upload separado; não invente payloads ou limites pendentes.
- Migrações Flyway aplicadas não são editadas.
- Senhas, tokens, conteúdo de evidências e segredos não entram em logs ou commits.

Preserve alterações locais preexistentes e execute comandos Git dentro do repositório correto: `API/`, `interface-administrativa/` ou `Docs/`.
