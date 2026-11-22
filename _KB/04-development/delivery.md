# Entrega, qualidade e governança

## Definition of Ready

Um item entra em execução quando tem objetivo, escopo, critérios de aceite, responsável, dependências, contrato afetado, dados ou mock necessário e tamanho compreensível. Itens que dependem de decisões abertas devem apontar para o ID da decisão e não iniciar implementação prematura.

## Definition of Done

Uma entrega está pronta quando o comportamento atende aos critérios de aceite, testes relevantes passam, revisão ocorre, documentação e contrato mudam juntos e não há defeito crítico aberto. Para integração, a evidência precisa demonstrar o fluxo em execução, não apenas telas ou chamadas isoladas.

## Qualidade por frente

API: validar autorização, erro padronizado, migração e transições de estado. Web: validar permissões, formulários e estados de interface. Mobile: validar persistência após reinício, permissões nativas, outbox e reenvio. Infraestrutura: validar segredos, saúde, logs, backup e acesso privado a evidências.

## Gestão do backlog

Use os IDs do JSON em issues e pull requests. O cenário vertical é a prioridade de demonstração. Se o prazo exigir corte, preservar autenticação, planejamento, checklist, foto, offline, sincronização e revisão; adiar dashboard avançado, notificações, PDF, assinatura e conflitos complexos.

Revise riscos a cada marco: divergência do modelo de dados, contrato de sync aberto, perda de dados offline, prazo e ausência de política para dados pessoais. Registre impacto, mitigação, responsável e prazo no acompanhamento do time.
