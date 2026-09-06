---
source: base_fieldops/Projeto FieldOps/09 - Regras de Negócio cc62594f77cf8318a1f181711ac3f508.md
suggested_target: ../01-product/business-rules.md
status: active
last_reviewed: 2026-09-05
migration_note: ../migration-inventory.csv
---

# Original: 09 - Regras de Negócio

(Copiado de `Docs/references/base_fieldops/Projeto FieldOps/09 - Regras de Negócio cc62594f77cf8318a1f181711ac3f508.md`)


# 9. Regras de Negócio

## 9.1 Usuários e acesso

**RN-001.** Somente usuários ativos poderão iniciar uma nova sessão.

**RN-002.** A inativação de um usuário não excluirá seus registros históricos.

**RN-003.** A API deverá validar o perfil em todas as operações protegidas.

**RN-004.** O técnico somente poderá consultar integralmente as inspeções atribuídas a ele.

**RN-005.** O supervisor poderá consultar e revisar inspeções pertencentes ao seu escopo.

**RN-006.** O administrador poderá gerenciar usuários e cadastros, mas não deverá alterar respostas de inspeção por padrão.

**RN-007.** Credenciais e tokens não poderão ser gravados em logs.

**RN-008.** A troca de senha ou o bloqueio do usuário deverá invalidar sessões conforme a política definida pela API.

## 9.2 Clientes, locais e equipamentos

**RN-009.** Todo local deverá pertencer a um cliente.

**RN-010.** Todo equipamento deverá pertencer a um local no MVP.

**RN-011.** O código QR de um equipamento deverá ser único.

**RN-012.** Um equipamento inativo não poderá ser utilizado em uma nova inspeção.

**RN-013.** Registros já utilizados em inspeções deverão ser inativados, não excluídos fisicamente.

**RN-014.** O local selecionado para a inspeção deverá ser compatível com o equipamento selecionado.

... (document truncated in import for brevity, full content preserved in source file)
