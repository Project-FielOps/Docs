# Pendências da interface administrativa web

**Atualizado em:** 2026-09-12

Este diretório é a fonte operacional das pendências de `interface-administrativa/`. Ele transforma os requisitos vigentes em especificações implementáveis, tasks rastreáveis e um catálogo dos endpoints que a aplicação deve consumir.

## Documentos

- [WEB-SPECS.md](WEB-SPECS.md): comportamento esperado das superfícies web, critérios de aceite e dependências.
- [ENDPOINTS.md](ENDPOINTS.md): catálogo de consumo `/api/v1`, disponibilidade atual e lacunas de contrato.
- [BACKLOG.md](BACKLOG.md): tasks, prioridade, dependências, status e ordem recomendada.

## Escopo

O painel atende administradores e supervisores para:

- autenticação e sessão;
- dashboard operacional;
- gestão de usuários, clientes, locais e equipamentos;
- construção, publicação e versionamento de modelos;
- planejamento e acompanhamento de inspeções;
- consulta de respostas, evidências e não conformidades;
- revisão por supervisor atribuído;
- auditoria.

O painel não executa o checklist do técnico e não substitui o mobile. `CLIENT_VIEWER`, dashboard avançado, relatórios, planos de ação completos e tema escuro permanecem fora do MVP.

## Fonte de verdade

Aplicar a precedência da [Knowledge Base](../../README.md). As especificações deste diretório detalham requisitos existentes; não ampliam autorização nem criam comportamento de domínio.

Referências obrigatórias:

- [Escopo do MVP](../../01-product/product-mvp.md)
- [Especificação de telas](../../01-product/application-scope-and-screen-specification.md)
- [Regras de negócio](../../01-product/product-business-rules.md)
- [Papéis e permissões](../../01-product/user-roles.md)
- [Rastreabilidade](../../01-product/requirements-traceability.md)
- [Arquitetura frontend](../../03-architecture/frontend.md)
- [Design System Industrial Web](../../03-architecture/WEB_DESIGN_SYSTEM.md)
- [Especificações da API](../../05-api/specs/README.md)
- [Decisões aprovadas](../../12-decisions/architecture-decisions.md)

## Interpretação do estado atual

Rotas e componentes encontrados no Angular são evidência de implementação parcial, não de aceite. Em 2026-09-12, o frontend possui as rotas principais e serviços HTTP, porém somente `src/app/app.spec.ts` foi encontrado como teste automatizado. Não há evidência versionada suficiente para declarar as superfícies web concluídas contra API real, design system e WCAG 2.1 AA.

Por isso, este backlog usa `IN_PROGRESS` para áreas parcialmente presentes e mantém o aceite pendente até o ciclo Frontend Agent → Web QA Agent.

## Regras de execução

1. Toda task deve apontar para uma spec `WEB-*` e para os endpoints aplicáveis.
2. O Frontend Agent segue integralmente o design system e entrega `READY_FOR_WEB_QA`.
3. O Web QA Agent valida separadamente comportamento, design, responsividade, acessibilidade e integração real.
4. Mock não comprova integração; build não comprova comportamento; screenshot não comprova interação.
5. Uma lacuna de API deve ser tratada como dependência `API_GAP`, nunca resolvida inventando endpoint no frontend.
6. Após aprovação, criar commit local apenas com o escopo validado; nunca executar `git push`.

