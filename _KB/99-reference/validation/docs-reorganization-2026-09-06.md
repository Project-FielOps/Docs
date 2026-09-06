# Validação da reorganização documental

Data: 2026-09-06

## Escopo

Foi consolidado `_KB/` como fonte atual para humanos e agentes de IA. Documentos de produto, requisitos, arquitetura, engenharia, especificações, planejamento, diagnóstico e validação foram movidos para as áreas canônicas. Os diretórios antigos foram removidos.

## Verificações executadas

| Verificação | Resultado |
|---|---|
| Links Markdown relativos, ignorando somente acervo importado | `BROKEN_MARKDOWN_LINKS=0` |
| Diretórios legados | removidos; `legacy_dirs=0` |
| Rotas antigas nos arquivos dos agentes | nenhuma ocorrência |
| Configuração `agents/openai.yaml` dos dois agentes | presente e preservada |
| `git diff --check` | sem erros |

## Fluxo dos agentes após a mudança

1. O `fieldops-project-context` lê `_KB/README.md`, diagnóstico em `_KB/99-reference/diagnostics`, produto em `_KB/01-product`, decisões em `_KB/12-decisions`, contratos em `_KB/05-api/specs` e rastreabilidade em `_KB/01-product`.
2. O Backend Agent continua usando o contexto compartilhado e seu workflow de handoff.
3. O Validation & QA Agent continua independente e mantém os gates `mvn clean test` e `mvn clean package` para mudanças de backend.
4. Os arquivos `openai.yaml` não tiveram sua interface alterada.

## Limites conhecidos

O diretório `plans/` continua ignorado e serve para rascunhos locais. `references/` conserva material histórico e alguns arquivos são deliberadamente tratados como acervo, não como contratos atuais. A implementação de IaC e GitHub Actions continua planejada, não declarada como existente.
