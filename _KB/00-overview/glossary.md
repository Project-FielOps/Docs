# Glossário canônico

Este glossário define os termos que devem ser usados de forma consistente em toda a documentação da KB.

## Termos centrais

- inspeção: atividade de verificação técnica em campo vinculada a cliente, local, equipamento e template.
- template: modelo de checklist usado como base para uma inspeção.
- versão de template: instância publicada de um modelo, que não pode ser alterada de forma destrutiva.
- snapshot: cópia dos itens do template no momento da inspeção.
- item: pergunta, critério ou campo do checklist.
- resposta: resposta preenchida para um item do snapshot.
- evidência: arquivo ou metadado que sustenta uma resposta ou não conformidade.
- não conformidade: ocorrência registrada durante a inspeção e vinculada ao processo de revisão.
- revisão: etapa em que o supervisor avalia as respostas e decide aprovação ou reprovação.
- auditoria: registro histórico de ações críticas e transições de estado.
- atribuição: vínculo de uma inspeção a um técnico ou supervisor responsável.
- agendamento: etapa de planejamento da inspeção e definição de responsáveis e local.

## Papéis

- ADMIN: cadastros, usuários e permissões.
- SUPERVISOR: acompanhamento e revisão de inspeções do seu escopo.
- TECHNICIAN: execução da inspeção atribuída.
- CLIENT_VIEWER: visualização de resultados em futuro.

## Estados principais

- ASSIGNED: inspeção atribuída.
- IN_PROGRESS: inspeção em execução.
- SUBMITTED: inspeção enviada para revisão.
- UNDER_REVIEW: inspeção em análise pelo supervisor.
- APPROVED: inspeção aprovada.
- REJECTED: inspeção reprovada e retornando para correção.
- CANCELED: inspeção cancelada.

## Convenções

- O termo “MVP” representa a visão funcional e documental dos capítulos originais, não o estado de implementação total do repositório.
- Termos em inglês devem ser usados apenas quando refletem o contrato, a implementação ou a regra documental original.
- Quando houver divergência entre documentação antiga e código, a KB deve registrar a inconsistência e classificar o código como desvio pendente de correção.

## Fontes

- [../../references/base_fieldops/Projeto FieldOps](../../references/base_fieldops/Projeto%20FieldOps)
- [../01-product/business-rules.md](../01-product/business-rules.md)
- [../12-decisions/ADR-001-source-of-truth.md](../12-decisions/ADR-001-source-of-truth.md)
