# FieldOps Web — Design System Industrial v1

**Status:** aprovado para o MVP da interface administrativa

**Escopo:** painel web usado por administradores e supervisores

**Tema:** Light Mode; tema escuro permanece P1

**Base visual:** [FieldOps Mobile — Design System v1](DESIGN_SYSTEM.md)

## Objetivo

O painel FieldOps é uma ferramenta operacional para configurar a plataforma, planejar inspeções, acompanhar a execução e revisar resultados. A interface deve permitir leitura rápida, comparação segura de dados e execução eficiente de tarefas repetitivas em notebooks e desktops.

O design compartilha a identidade do mobile, sem copiar sua composição. No campo, o mobile prioriza uma ação por vez e alvos grandes; na web, o painel prioriza visão geral, filtros, tabelas, formulários e comparação de informações, mantendo hierarquia e espaço suficientes para não se tornar um dashboard denso.

Este documento define aparência e interação. Regras de negócio, permissões, estados oficiais e contratos continuam definidos na documentação de produto, domínio e API.

## Princípios de experiência

1. **Situação antes da ação:** deixar claro onde o usuário está, qual o estado operacional e quais ações são válidas.
2. **Eficiência com segurança:** reduzir passos em tarefas frequentes, mas confirmar ações críticas e irreversíveis.
3. **Densidade controlada:** exibir dados suficientes para comparar e decidir, sem comprimir texto, controles ou estados.
4. **Hierarquia previsível:** título, contexto, filtros, conteúdo e ações ocupam posições consistentes entre telas.
5. **Verdade operacional:** status e permissões vêm do domínio e da API; cor, menu ou guard não substituem autorização.
6. **Estado sempre visível:** loading, vazio, erro, sucesso, permissão insuficiente e ação em processamento têm tratamento explícito.
7. **Acessível por padrão:** contraste, teclado, foco, labels e mensagens fazem parte do componente, não de uma correção posterior.
8. **Continuidade entre canais:** nomes, cores semânticas e linguagem permanecem coerentes com o aplicativo mobile.

## Direção visual

A linguagem visual combina verde FieldOps, azul-petróleo, grafite e superfícies off-white. A interface é industrial, precisa e contemporânea: estrutura clara, bordas discretas, cantos moderados e pouca decoração.

Usar:

- superfícies claras e hierarquia por espaçamento, borda e contraste;
- verde para ação principal e identidade;
- grafite e azul-petróleo para navegação e informação estrutural;
- cor semântica apenas quando houver significado operacional;
- ícone acompanhado de texto em ações ou estados que possam gerar dúvida.

Evitar:

- glassmorphism, neon, gradientes chamativos ou estética cyberpunk;
- sombras fortes, excesso de cards ou cantos excessivamente arredondados;
- dashboard corporativo denso, gráficos decorativos ou métricas sem ação associada;
- aparência genérica de template SaaS ou produto de IA;
- usar o Scout como decoração recorrente no painel;
- depender apenas de hover, ícone ou cor para transmitir informação.

## Design tokens

Os tokens abaixo são a referência para o tema do Angular Material e para estilos SCSS. Não usar valores hexadecimais avulsos em componentes quando existir token equivalente.

### Cores

```scss
$fo-color-primary: #16a66a;
$fo-color-primary-hover: #128f5c;
$fo-color-primary-pressed: #0e8755;
$fo-color-primary-dark: #075e4a;
$fo-color-primary-soft: #e7f7f0;
$fo-color-primary-subtle: #f2faf6;

$fo-color-navigation: #102a2e;
$fo-color-navigation-hover: #17383d;
$fo-color-navigation-active: #21464b;

$fo-color-background: #f7f8f6;
$fo-color-surface: #ffffff;
$fo-color-surface-secondary: #f1f3f1;
$fo-color-surface-selected: #edf7f2;

$fo-color-text-primary: #102a2e;
$fo-color-text-secondary: #647274;
$fo-color-text-tertiary: #8e999a;
$fo-color-text-inverse: #ffffff;

$fo-color-border: #e2e7e4;
$fo-color-border-strong: #cdd5d1;
$fo-color-disabled: #b8c0bd;
$fo-color-focus: #397c9a;

$fo-color-success: #16865a;
$fo-color-success-soft: #e7f7f0;
$fo-color-warning: #b7791f;
$fo-color-warning-soft: #fff5e3;
$fo-color-error: #c94141;
$fo-color-error-soft: #fdecec;
$fo-color-info: #397c9a;
$fo-color-info-soft: #eaf4f8;
$fo-color-conflict: #7d4bb3;
$fo-color-conflict-soft: #f5ecff;
```

O verde de identidade pode representar sucesso somente quando o texto ou ícone esclarece o significado. Falha é vermelha; atenção e pendência usam âmbar; informação e processamento usam azul; conflito usa violeta. Todo status combina texto, ícone e cor.

### Tipografia

Usar **Nunito Sans** em toda a plataforma, nos pesos 400, 500, 600, 700 e 800. Fontes de tabela podem usar números tabulares (`font-variant-numeric: tabular-nums`) para facilitar comparação. Não usar Inter no produto.

| Token | Tamanho/linha | Peso | Uso |
| --- | --- | --- | --- |
| `display` | 32/40 px | 700 | número ou mensagem principal isolada |
| `h1` | 28/36 px | 700 | título da página |
| `h2` | 22/30 px | 700 | seção principal |
| `h3` | 18/26 px | 600 | card, painel ou subseção |
| `title` | 16/24 px | 600 | título de componente |
| `body` | 14/21 px | 400 | conteúdo padrão |
| `body-strong` | 14/21 px | 600 | conteúdo enfatizado |
| `label` | 13/18 px | 600 | campo e controle |
| `caption` | 12/17 px | 500 | metadado e apoio |

Texto operacional importante não deve ter menos de 14 px. Evitar caixa alta em frases; usar somente em códigos curtos ou labels auxiliares, com espaçamento entre letras adequado.

### Espaçamento, raios e elevação

```scss
$fo-space-1: 4px;
$fo-space-2: 8px;
$fo-space-3: 12px;
$fo-space-4: 16px;
$fo-space-5: 20px;
$fo-space-6: 24px;
$fo-space-8: 32px;
$fo-space-10: 40px;
$fo-space-12: 48px;

$fo-radius-xs: 4px;
$fo-radius-sm: 8px;
$fo-radius-md: 12px;
$fo-radius-lg: 16px;
$fo-radius-full: 999px;

$fo-motion-instant: 100ms;
$fo-motion-fast: 160ms;
$fo-motion-normal: 240ms;
$fo-motion-slow: 360ms;
```

Campos e botões usam raio de 8 px; cards e painéis, 12 px; diálogos, 16 px; badges e chips, raio total. Usar borda antes de sombra. A elevação padrão é `0 1px 2px rgba(16, 42, 46, 0.06)` e a de overlays é `0 12px 32px rgba(16, 42, 46, 0.14)`.

### Dimensões e breakpoints

- largura mínima suportada do painel: 1024 px;
- conteúdo central: máximo de 1440 px, salvo tabelas que precisem ocupar a área disponível;
- margem do conteúdo: 24 px entre 1024–1279 px e 32 px a partir de 1280 px;
- sidenav expandida: 248 px; recolhida: 72 px;
- topbar: 64 px;
- controles padrão: 40 px; controles de ação principal: 44 px;
- alvos interativos: mínimo de 40 × 40 px no desktop, preferencialmente 44 × 44 px para ações isoladas;
- `compact`: 1024–1279 px; `standard`: 1280–1599 px; `wide`: 1600 px ou mais.

Abaixo de 1024 px, preservar o fluxo essencial com sidenav em drawer, filtros recolhíveis e tabelas com rolagem horizontal identificável. O painel não precisa reproduzir o aplicativo mobile.

## Estrutura da aplicação

### Shell

O shell possui:

- **sidenav:** marca, áreas autorizadas, item ativo e opção de recolher;
- **topbar:** título contextual quando necessário, ambiente, ajuda e menu do usuário;
- **breadcrumb:** somente quando a hierarquia não estiver evidente pelo título e navegação;
- **conteúdo:** cabeçalho de página, comandos, filtros, dados e feedback;
- **mensagens globais:** banners ou snackbars controlados, sem empilhar notificações.

O item ativo da sidenav usa superfície de contraste e marcador visual. Seções não autorizadas não aparecem. Recolher o menu mantém tooltip e nome acessível dos ícones.

### Cabeçalho de página

Ordem recomendada:

1. breadcrumb opcional;
2. título e descrição curta;
3. metadado ou status relevante;
4. ação primária à direita;
5. ações secundárias em menu quando houver mais de duas.

Cada página possui no máximo uma ação visualmente primária. A ação deve usar verbo e objeto, como `Criar inspeção`, `Publicar versão` ou `Aprovar inspeção`.

### Grid

Usar grid fluido de 12 colunas, gap de 24 px. Formulários comuns ocupam de 6 a 8 colunas em telas largas; páginas de detalhe podem combinar conteúdo de 8 colunas com painel lateral de 4. Não esticar campos curtos, como CEP, data ou prioridade, por toda a tela.

## Componentes

### Botões e ações

- **Primary:** fundo `primary`, texto branco; uma ação dominante por contexto.
- **Secondary:** fundo `primarySoft`, texto `primaryDark`.
- **Outline:** superfície transparente, borda `borderStrong`.
- **Ghost:** ação auxiliar em toolbar, tabela ou card.
- **Danger:** reservado a ação destrutiva ou bloqueio relevante; confirmação obrigatória quando houver impacto material.
- **IconButton:** exige nome acessível e tooltip; não usar para ações ambíguas.

Estados obrigatórios: default, hover, focus-visible, pressed, disabled e loading. Durante loading, preservar largura e label contextual; impedir envio duplicado.

### Campos e formulários

Labels ficam sempre visíveis acima ou no contorno do campo; placeholder não substitui label. Texto de ajuda vem antes do erro, e o erro aparece próximo ao campo. Campos obrigatórios devem ser identificados textualmente, não apenas por asterisco sem legenda.

Formulários usam agrupamento semântico, títulos de seção e ordem de tabulação coerente. Preservar os valores após falha de API. Em edição longa, separar ações fixas somente quando isso não ocultar conteúdo.

Validação ocorre ao sair do campo ou ao enviar, evitando erro prematuro durante digitação. Erros da API são mapeados aos campos; erro geral aparece em banner no início do formulário e recebe foco quando necessário.

### Tabelas de dados

Tabelas são o padrão para conjuntos administrativos comparáveis. Devem oferecer conforme a necessidade:

- título ou `aria-label`, cabeçalhos claros e ordenação anunciada;
- busca, filtros ativos e ação para limpar filtros;
- paginação e quantidade total;
- alinhamento à esquerda para texto, à direita para números e consistente para datas;
- status com badge textual; códigos e identificadores com fonte ou estilo estável;
- ações da linha no final, com até duas ações frequentes visíveis e demais em menu;
- seleção em lote somente quando houver operação real para o conjunto;
- estado vazio diferente de resultado vazio após filtro;
- skeleton ou indicador de carregamento sem apagar abruptamente o contexto;
- rolagem horizontal quando necessária, sem truncar a coluna de identificação principal.

Não usar zebra striping forte. Preferir divisores discretos e hover suave. Linhas usam 48–56 px; modo compacto de 44 px é permitido em auditoria e conjuntos extensos, desde que a acessibilidade seja preservada.

### Filtros e busca

Busca permanece próxima ao conjunto filtrado. Até três filtros frequentes podem ficar visíveis; filtros adicionais abrem painel lateral ou popover. Filtros aplicados aparecem como chips removíveis e sobrevivem à paginação. Informar claramente quando nenhum resultado decorre dos filtros.

### Cards e indicadores

Cards agrupam uma unidade coerente, não substituem toda a estrutura da página. Usar padding de 20–24 px, raio de 12 px e borda discreta. Cards de KPI contêm nome, valor, período/contexto e link ou ação quando aplicável.

O dashboard mínimo mostra pendentes, enviadas, aprovadas/rejeitadas e aguardando revisão. Gráficos só entram quando ajudam uma decisão e possuem alternativa textual ou tabular.

### Status, prioridade e progresso

`StatusBadge` combina ícone opcional, texto e fundo suave. Não traduzir estados de domínio de formas diferentes entre telas. Prioridade é separada de status. Progresso apresenta valor textual ou percentual além da barra.

Estados sugeridos de sincronização e operação mantêm a semântica mobile:

| Estado | Cor | Exemplo de texto |
| --- | --- | --- |
| sucesso | verde | `Aprovada`, `Sincronizado` |
| atenção | âmbar | `Aguardando revisão`, `Prazo próximo` |
| erro | vermelho | `Falha ao carregar`, `Bloqueada` |
| informação | azul | `Em processamento`, `Em andamento` |
| conflito | violeta | `Conflito para revisar` |
| neutro | grafite | `Rascunho`, `Cancelada` |

### Dialog, drawer, menu e snackbar

- **Dialog:** decisão curta ou confirmação; não acomodar fluxo longo ou formulário complexo.
- **Drawer:** filtros avançados, contexto complementar ou detalhe rápido sem perder a lista.
- **Menu:** ações secundárias relacionadas; nunca esconder a única ação principal.
- **Snackbar:** confirmação transitória e reversível; erros que exigem decisão permanecem visíveis em banner ou na página.

Diálogos destrutivos nomeiam o objeto e o impacto. Não usar confirmação genérica como `Tem certeza?` sem contexto.

### Estados de página

Toda tela de dados implementa:

- `loading`: skeleton compatível com a estrutura ou progresso identificado;
- `loaded`: conteúdo atualizado e ação disponível conforme permissão;
- `empty`: explicação e ação para primeiro uso, quando autorizada;
- `no-results`: filtros mantidos, resumo da busca e opção de limpar;
- `error`: problema em linguagem clara, dados preservados e ação para tentar novamente;
- `forbidden`: explicar falta de acesso sem revelar dados protegidos;
- `processing`: bloquear somente a ação em curso e prevenir duplicação;
- `success`: confirmação proporcional, sem interromper desnecessariamente o fluxo.

## Padrões por fluxo

### Dashboard

Começar por pendências acionáveis e indicadores do MVP. Priorizar inspeções aguardando revisão, atrasadas e não conformidades críticas. Usar no máximo uma faixa principal de KPIs antes das listas de trabalho. Cada indicador deve abrir o recorte correspondente quando houver permissão.

### Cadastros

Listagem é a entrada principal; criação e edição usam página dedicada ou drawer apenas para formulários curtos. Relações seguem `cliente → local → equipamento`. Ao trocar um pai, limpar ou revalidar dependências e explicar o efeito.

### Construtor de modelos

Separar estrutura, propriedades do item e prévia. Mostrar rascunho, versão publicada e alterações não publicadas de forma inequívoca. Reordenamento deve possuir alternativa por teclado ou botões; drag-and-drop não pode ser o único mecanismo.

### Planejamento de inspeção

Agrupar contexto, responsáveis, prazo/prioridade e requisitos de execução. Filtrar locais pelo cliente e equipamentos pelo local. Antes de confirmar, exibir resumo e erros de vínculo no grupo correspondente.

### Acompanhamento e revisão

Manter identificação, responsáveis e status visíveis. Organizar respostas por seção e associar evidência e não conformidade ao item correto. Ações de aprovar ou reprovar ficam no contexto de revisão; reprovação exige motivo. Comparações não podem depender apenas de cor.

### Auditoria

Usar tabela compacta, filtros avançados e detalhe somente leitura. Destacar ator, ação, recurso, data e correlação sem expor tokens, senhas ou conteúdo sensível. Datas mostram timezone ou seguem padrão de ambiente documentado.

## Conteúdo e linguagem

- usar português direto, profissional e humano;
- preferir verbos de ação: `Criar`, `Salvar`, `Publicar`, `Aprovar`, `Reprovar`;
- evitar jargão técnico da API e enum bruto na interface;
- mensagens de erro explicam o problema, o que foi preservado e a próxima ação;
- botões não usam `OK` ou `Sim` quando uma ação específica puder ser nomeada;
- datas, números e estados seguem formato consistente em toda a aplicação;
- não afirmar sucesso antes da confirmação da API.

## Iconografia e identidade

Usar **Material Symbols Rounded** ou o conjunto já fornecido pelo Angular Material, mantendo um único sistema de ícones na web. Tamanhos padrão: 20 px em controles, 24 px na navegação e 16–18 px em badges. Ícones são apoio, não substitutos automáticos de labels.

O Scout pode aparecer apenas em onboarding, primeiro acesso ou empty states relevantes. Deve manter o estilo e a paleta definidos no design system mobile, sem ocupar páginas de trabalho recorrente ou substituir instruções.

## Motion

Motion comunica mudança de estado: abertura de drawer, atualização de filtro, expansão de seção e confirmação de ação. Usar durações de 100–240 ms na maior parte da interface e easing Material padrão. Evitar animação de entrada em todas as linhas, loops decorativos e transições que atrasem uma tarefa.

Respeitar `prefers-reduced-motion`; nesse modo, remover deslocamentos e animações não essenciais.

## Acessibilidade

As telas principais devem buscar **WCAG 2.1 AA**, conforme DEC-011 e NFR-013.

- contraste mínimo de 4.5:1 para texto comum e 3:1 para texto grande e elementos gráficos essenciais;
- foco visível com contorno de 2 px e offset suficiente;
- operação completa por teclado, sem armadilha de foco;
- landmarks, headings e ordem de leitura coerentes;
- labels, descrições e erros associados programaticamente aos campos;
- tabelas com cabeçalhos e ordenação acessíveis;
- diálogos com foco inicial, contenção e retorno ao elemento de origem;
- status, prioridade e validação nunca comunicados somente por cor;
- tooltip não contém informação indispensável;
- zoom de 200% não pode impedir o fluxo principal;
- atualizações assíncronas importantes são anunciadas sem interromper a leitura.

## Tema Angular Material

O tema deve mapear os tokens FieldOps para as APIs de theming do Angular Material. Componentes Material podem ser customizados por tema e variáveis suportadas; evitar seletores frágeis sobre a estrutura interna ou `::ng-deep`.

Centralizar em `src/styles/` ou `src/app/shared/design-system/`:

```text
design-system/
├── tokens/          cores, tipografia, spacing, radius e motion
├── theme/           tema Angular Material e estilos globais
├── components/      componentes visuais compartilhados
├── directives/      comportamentos acessíveis reutilizáveis
└── patterns/        estados de página, tabelas, filtros e formulários
```

Componentes visuais compartilhados não contêm autorização ou regra de domínio. A feature decide quais ações existem; o componente apresenta seus estados de forma consistente.

## Critérios de pronto visual

Uma tela web está pronta quando:

1. usa tokens e componentes compartilhados em vez de valores locais equivalentes;
2. trata loading, vazio, sem resultado, erro, sucesso, permissão e processamento;
3. preserva dados de formulário em falha recuperável;
4. funciona nos breakpoints `compact`, `standard` e `wide`;
5. permite o fluxo principal por teclado e possui foco visível;
6. não depende apenas de cor, hover ou ícone;
7. respeita papel, vínculo e estado exibidos pela API;
8. possui teste de componente para estados e ações críticas;
9. passa por revisão visual e auditoria básica WCAG 2.1 AA;
10. não introduz regra de negócio dentro do design system.

## Referências

- [FieldOps Mobile — Design System v1](DESIGN_SYSTEM.md)
- [Design do FieldOps Mobile](mobile-design.md)
- [Arquitetura da interface administrativa](frontend.md)
- [Escopo e especificação de telas](../01-product/application-scope-and-screen-specification.md)
- [Requisitos não funcionais](../01-product/requirements-non-functional.md)
- [Decisões de arquitetura](../12-decisions/architecture-decisions.md)
- [Regras de negócio](../01-product/business-rules.md)
