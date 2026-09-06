# Migration Plan — Mover conteúdo para a KB

## Objetivo

Migrar a documentação existente para a estrutura da KB sem perda de histórico e mantendo rastreabilidade.

## Princípios

- Não apagar documentos originais;
- Registrar cada movimento com origem → destino;
- Marcar documentos migrados e manter redirect/links por 1 release;
- Não transformar propostas em fatos; manter status.

## Fases

1. Inventory: listar todos os documentos em `base_fieldops` e `Docs` (com IDs e resumo).
2. Map: para cada documento, decidir: manter, mover, dividir, consolidar, reescrever, depreciar.
3. Draft: criar arquivos canônicos em `Docs/_KB` referenciando `base_fieldops` como fonte.
4. Review: validação humana dos drafts (produto e arquitetura).
5. Cutover: atualizar índices e README da KB; manter originais em `Docs/references/legacy`.
6. Cleanup: após validação e um ciclo de release, arquivar ou remover duplicados com aviso.

## Exemplo de mapeamento inicial

- `base_fieldops/Projeto FieldOps/09 - Regras de Negócio` → `Docs/_KB/01-product/business-rules.md` (mantido como canônico)
- `Docs/architecture/overview.md` → `Docs/_KB/03-architecture/current-architecture.md` (consolidado)
- `Docs/engineering/development-guide.md` → `Docs/_KB/04-development/getting-started.md` (revisar)

## Timeline sugerida

- Inventory & Map: 2-3 dias
- Draft: 1 semana
- Review: 3 dias
- Cutover: 1 dia

## Acompanhamento

- Criar issues com tag `docs:migration` para cada item relevante;
- Registrar responsável por cada documento.
