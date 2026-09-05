# Rastreabilidade do MVP

| Objetivo | Requisito | Regras | Aceite | Implementação/teste |
|---|---|---|---|---|
| OBJ-001 | FR-AUTH-001/002, FR-MASTER-001 | RN-001..014 | AC-AUTH/USERS/MASTER | API `auth`, `user`, `client`, `site`, `equipment`; `AuthenticationTest` |
| OBJ-002 | FR-TEMPLATE-001/002, FR-PLAN-001 | RN-015..031 | AC-TEMPLATE/SCHEDULING | API `template`, `inspection`; `TemplateVersioningTest` |
| OBJ-003 | FR-EXEC-001, FR-EVID-001, FR-NATIVE-001 | RN-032..078 | AC-START/CHECKLIST/EVIDENCE/QR/LOCATION/COMPLETE/SYNC | API parcial; mobile ausente; `InspectionWorkflowTest` cobre online |
| OBJ-004 | FR-REVIEW-001, FR-AUDIT-001 | RN-079..090 | AC-REVIEW/SECURITY | API `review`, `audit`; revisão integrada existente |
| OBJ-005 | Todos | RNF-001..012 | AC-RELEASE | EAP/cronograma histórico; evidência final pendente |

## Critérios de aceite por domínio

Os critérios originais estão em [capítulo 17](../references/base_fieldops/Projeto%20FieldOps/17%20-%20Crit%C3%A9rios%20de%20Aceita%C3%A7%C3%A3o%20acf2594f77cf82d599e501edefa62b42.md). `AC-*` é a âncora oficial; não duplicar cenários em cada PBI. O [roteiro de demonstração](../engineering/demo.md) seleciona os cenários necessários ao MVP.

## Estado de implementação observado

API e painel têm código; mobile e sync ainda não. Testes Java existem para autenticação, versionamento e fluxo online. Não há teste frontend encontrado além do smoke de criação da aplicação. Esse estado é uma fotografia da leitura em 2026-09-05, não um aceite.
