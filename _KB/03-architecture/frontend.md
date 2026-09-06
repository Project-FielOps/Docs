# Arquitetura da interface administrativa

## Estado atual

O painel está em `interface-administrativa/` e usa Angular 22, TypeScript estrito, componentes standalone, Angular Material, SCSS e Vitest. Ele atende administração, planejamento e revisão; não substitui o app de campo.

## Organização

`src/app/core/` concentra autenticação, guards, interceptors, HTTP, modelos e layout. `shared/` contém componentes reutilizáveis. `features/` organiza as áreas de negócio: autenticação, dashboard, usuários, clientes, locais, equipamentos, modelos, inspeções, evidências, não conformidades e auditoria.

Serviços por feature consomem a API. O interceptor de autenticação envia o token; o interceptor de erro centraliza respostas de sessão, rede, permissão e servidor. A URL da API é configurada em `src/environments/` e deve apontar para `/api/v1`.

## Regras de experiência

Cada tela de dados trata carregamento, vazio, erro e conteúdo. Formulários reativos exibem erros de campo retornados pela API. Controles de ação devem respeitar o papel do usuário e o estado da inspeção, mas a API continua sendo a autoridade. Acessibilidade mínima inclui contraste, erro próximo ao campo e status que não dependem só de cor.
