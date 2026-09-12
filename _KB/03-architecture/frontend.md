# Arquitetura da interface administrativa

O conceito visual, os tokens e os padrões de interação estão definidos em [FieldOps Web — Design System Industrial v1](WEB_DESIGN_SYSTEM.md).

## Estado atual

O painel está em `interface-administrativa/` e usa Angular 22, TypeScript estrito, componentes standalone, Angular Material, SCSS e Vitest. Ele atende administração, planejamento e revisão; não substitui o app de campo.

## Organização

`src/app/core/` concentra autenticação, guards, interceptors, HTTP, modelos e layout. `shared/` contém componentes reutilizáveis. `features/` organiza as áreas de negócio: autenticação, dashboard, usuários, clientes, locais, equipamentos, modelos, inspeções, evidências, não conformidades e auditoria.

Serviços por feature consomem a API. O interceptor de autenticação envia o token; o interceptor de erro centraliza respostas de sessão, rede, permissão e servidor. A URL da API é configurada em `src/environments/` e deve apontar para `/api/v1`.

## Regras de experiência

Cada tela de dados trata carregamento, vazio, sem resultado, erro, sucesso, permissão e ação em processamento. Formulários reativos exibem erros de campo retornados pela API e preservam os dados em falhas recuperáveis. Controles de ação devem respeitar o papel do usuário e o estado da inspeção, mas a API continua sendo a autoridade. As telas principais devem buscar WCAG 2.1 AA, com contraste, foco visível, operação por teclado, erro próximo ao campo e status que não dependem só de cor.
