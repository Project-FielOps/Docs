# Requisitos funcionais

Os IDs FR são uma indexação implementável dos UC e PBI existentes; não substituem RN, UC ou AC da base. Cada domínio tem pré-condições, permissões, fluxos, erros e critérios detalhados em `specs/`.

| ID | Requisito | Atores | Aceite principal |
|---|---|---|---|
| FR-AUTH-001 | Autenticar, renovar e encerrar sessão de usuário ativo | Todos | AC-AUTH |
| FR-AUTH-002 | Administrador gerenciar usuários e situação | ADMIN | AC-USERS |
| FR-MASTER-001 | Gerenciar cliente, local e equipamento com vínculos e inativação | ADMIN/SUPERVISOR | AC-MASTER |
| FR-TEMPLATE-001 | Criar, editar e visualizar rascunho de modelo | ADMIN/SUPERVISOR | AC-TEMPLATE |
| FR-TEMPLATE-002 | Publicar versão imutável e gerar snapshot na inspeção | ADMIN/SUPERVISOR/Sistema | AC-TEMPLATE |
| FR-PLAN-001 | Agendar, atribuir e cancelar inspeção com justificativa | SUPERVISOR | AC-SCHEDULING |
| FR-EXEC-001 | Técnico atribuído iniciar e preencher checklist dinâmico | TECHNICIAN | AC-START, AC-CHECKLIST |
| FR-EVID-001 | Técnico capturar e sincronizar fotografia vinculada | TECHNICIAN | AC-EVIDENCE |
| FR-NATIVE-001 | Confirmar equipamento por QR e registrar localização pontual | TECHNICIAN | AC-QR, AC-LOCATION |
| FR-NC-001 | Técnico registrar não conformidade vinculada à inspeção | TECHNICIAN | AC-NONCONFORMITY |
| FR-SYNC-001 | Persistir localmente, enviar push idempotente e receber pull por cursor | TECHNICIAN/Sistema | AC-SYNC |
| FR-REVIEW-001 | Supervisor atribuído revisar, aprovar ou reprovar | SUPERVISOR | AC-REVIEW |
| FR-AUDIT-001 | Registrar e consultar histórico de ações críticas | Sistema/ADMIN/SUPERVISOR | AC-SECURITY |

## Permissões do MVP

ADMIN gerencia usuários e cadastros, mas não executa, responde, revisa ou decide uma inspeção. SUPERVISOR cria modelos, planeja e revisa as inspeções sob sua responsabilidade. TECHNICIAN opera somente inspeções atribuídas a si. CLIENT_VIEWER permanece futuro. Toda regra é validada na API por identidade e vínculo do recurso.
