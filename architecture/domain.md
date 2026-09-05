# Domínio, dados e ciclo de inspeção

## Contextos

Os contextos são identidade e acesso, cadastros, modelos, planejamento, execução de campo, evidências, revisão, auditoria e sincronização. As entidades centrais são `User`, `Client`, `InspectionSite`, `Equipment`, `InspectionTemplate`, `InspectionTemplateVersion`, `TemplateSection`, `TemplateItem`, `Inspection`, `InspectionItemSnapshot`, `InspectionResponse`, `Evidence`, `NonConformity`, `InspectionReview` e `AuditEvent`.

`InspectionTemplateVersion` é a versão publicada e imutável. Ao agendar, a API cria `InspectionItemSnapshot`; respostas sempre apontam para esse snapshot, preservando o histórico se o modelo mudar posteriormente.

## Ciclo de negócio

```text
ATRIBUÍDA → EM_ANDAMENTO → ENVIADA → EM_REVISÃO → APROVADA
                 │                           └→ REPROVADA → EM_ANDAMENTO
                 └────────────────────────────→ CANCELADA
```

Cancelamento exige justificativa e é auditado. Reprovação exige motivo. O técnico responsável é o único executor do MVP; a API bloqueia transições inválidas e acessos fora do perfil.

## Dados e integridade

PostgreSQL é a fonte oficial e usa Flyway para evolução do esquema. Registros com histórico são inativados, não removidos pelo fluxo comum. Índices devem atender e-mail, QR code, estado, técnico e datas. Use controle otimista nas entidades concorrentes.

O modelo textual e o diagrama de classes de origem possuem divergências. Antes de criar ou mudar schema, registre a escolha em `decisions.md`; ARC-004 é o item que encerra essa reconciliação.
