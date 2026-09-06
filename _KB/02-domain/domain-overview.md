# Visão de domínio

## Domínio principal

O domínio do FieldOps gira em torno da gestão de inspeções técnicas em campo, com foco em segurança, rastreabilidade e padronização de checklist.

## Entidades centrais

- User
- Client
- Site
- Equipment
- Template
- TemplateVersion
- TemplateSection
- TemplateItem
- Inspection
- InspectionItemSnapshot
- InspectionResponse
- Evidence
- NonConformity
- InspectionReview
- AuditEvent

## Relacionamentos principais

- Cliente possui locais.
- Local possui equipamentos.
- Equipamento pode ser usado em inspeção conforme compatibilidade.
- Modelo gera versões publicadas.
- Inspeção usa um snapshot da versão do modelo.
- Cada resposta pertence a um item do snapshot.
- Evidências e NC vinculam-se à inspeção e, quando aplicável, à resposta.
- Revisão e auditoria registram decisões e eventos críticos.

## Regras de domínio

- o local deve ser compatível com o equipamento;
- a inspeção deve manter um snapshot imutável;
- respostas devem respeitar tipos e regras de obrigatoriedade;
- evidência deve estar associada à inspeção;
- o processo de aprovação ou reprovação exige supervisor atribuído;
- a reabertura de inspeção rejeitada é um evento formal e documentado.

## Fontes

- [../../references/base_fieldops/Projeto FieldOps](../../references/base_fieldops/Projeto%20FieldOps)
- [../01-product/product-business-rules.md](../01-product/product-business-rules.md)
- [../03-architecture/data-model.md](../03-architecture/data-model.md)
