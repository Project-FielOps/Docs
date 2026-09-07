# Design do FieldOps Mobile

## Objetivo

O FieldOps Mobile é uma ferramenta de trabalho para técnicos em campo. O design deve permitir que o técnico entenda rapidamente sua próxima ação, responda checklists com poucos toques e saiba com segurança o que foi salvo localmente e o que já foi confirmado pelo servidor.

A interface deve parecer um produto operacional profissional: clara, confiável, precisa e humana. A estética é clean e premium, mas nunca decorativa a ponto de competir com a tarefa.

## Princípios de experiência

1. **Ação primeiro:** a tela deve responder “o que preciso fazer agora?”.
2. **Baixa carga cognitiva:** uma decisão principal por bloco visual.
3. **Uso com uma mão:** ações primárias devem ficar próximas da parte inferior e ter áreas de toque confortáveis.
4. **Estado sempre visível:** loading, vazio, erro, offline, pendente, sincronizando e conflito não podem parecer uma tela quebrada.
5. **Verdade operacional:** o estado oficial da inspeção nunca é substituído pelo estado local de sincronização.
6. **Feedback proporcional:** toda ação confirmada deve produzir resposta visual ou textual curta.
7. **Campo real:** priorizar legibilidade em ambientes externos, contraste e tolerância a toques imprecisos.

## Direção visual

O sistema visual combina azul petróleo, teal, grafite e off-white. A composição usa superfícies claras, cards com cantos suaves, bordas discretas, whitespace e hierarquia tipográfica forte.

Evitar:

- neon, roxo ou estética cyberpunk;
- excesso de gradientes, glassmorphism ou sombras;
- dashboards densos e gráficos sem ação associada;
- aparência de template SaaS ou produto de IA;
- elementos infantis ou mascote permanente.

## Design tokens

### Cores

| Token | Valor | Uso |
| --- | --- | --- |
| `primary` | `#0B5960` | ações primárias, links e navegação ativa |
| `primaryDark` | `#073D43` | pressed, títulos de alto contraste |
| `accent` | `#D9A441` | destaque operacional e prioridade |
| `background` | `#F5F7F5` | fundo global |
| `surface` | `#FFFFFF` | cards, campos e superfícies |
| `textPrimary` | `#173235` | títulos e conteúdo principal |
| `textSecondary` | `#667879` | apoio, metadados e instruções |
| `border` | `#E1E9E7` | divisores e contornos |
| `success` | `#2F8F67` | aprovado, sincronizado e conforme |
| `warning` | `#B7791F` | atenção, pendência e prioridade alta |
| `error` | `#C05640` | falhas e bloqueios críticos |
| `muted` | `#EEF3F1` | controles secundários e áreas neutras |

Não comunicar estado somente por cor. Todo estado deve ter texto, ícone ou forma complementar.

### Tipografia

Usar fonte sem serifa, simples e legível, com pesos consistentes:

- `display`: 30/36, peso 700;
- `heading`: 23/29, peso 700;
- `title`: 17/23, peso 700;
- `body`: 15/21, peso 400;
- `caption`: 12/17, peso 500;
- `label`: 13/18, peso 700.

O corpo mínimo recomendado é 15 px. Textos de apoio podem usar 12 px apenas quando houver contraste suficiente.

### Espaçamento, raio e elevação

Escala de espaçamento: `4, 8, 12, 16, 24, 32, 48`.

Raios: `8` para controles pequenos, `12` para campos, `18` para cards, `24` para superfícies de destaque e `999` somente para badges/chips.

Priorizar bordas e contraste de superfície. Sombras devem ser suaves, curtas e usadas apenas para indicar elevação real.

## Componentes

Componentes visuais não devem conter regras de negócio. A lista inicial é:

- `Button` e `IconButton`;
- `Card`;
- `InspectionCard`;
- `ChecklistItem`;
- `StatusBadge` e `PriorityBadge`;
- `SyncIndicator`;
- `ProgressBar`;
- `Avatar`;
- `Chip` e `FilterChip`;
- `SearchField`;
- `SectionHeader`;
- `EmptyState`;
- `Skeleton`;
- `BottomSheet`;
- `Toast`;
- `Scout`.

Touch targets devem ter pelo menos 44 × 44 pontos. Botões críticos devem ser largos, ter label explícito e não depender somente de ícone.

## Navegação e composição

Usar bottom tabs para as áreas recorrentes:

```text
Início | Inspeções | Histórico | Perfil
```

O fluxo de execução usa Stack:

```text
Detalhes → Checklist → Evidências/observações → Revisão → Confirmação
```

Notificações são acessadas pelo header da Home. A navegação deve preservar contexto e permitir retorno sem perder alterações locais.

## Telas e hierarquia

### Login

Identidade FieldOps, campos grandes, loading no botão e erro acionável. O login atual é mockado e não representa autenticação real.

### Home

Mostrar saudação, próxima inspeção, resumo enxuto, progresso do dia, conectividade, sincronização e atualização relevante. Evitar gráficos corporativos.

### Lista

Busca e filtros ficam acima dos cards. O card mostra apenas título, código, local, horário, prioridade, status, progresso e pendência de sync quando aplicável.

### Detalhes

Separar metadados, local, progresso, observações, evidências e ações. Ações incompatíveis com o estado devem ser ocultadas ou apresentadas desabilitadas com explicação.

### Checklist

Cada item deve ser confortável para uma mão. Preferir opções grandes para conformidade e instrução próxima do campo de resposta. A progressão deve ser imediata e mostrar itens obrigatórios.

### Revisão e conclusão

Antes do envio, mostrar pendências, inconsistências, evidências e observações. Offline deve comunicar: **“Concluída no dispositivo — envio pendente”**. O estado oficial só muda após confirmação do backend.

## Estados de conectividade e sincronização

`InspectionStatus` representa o workflow oficial. `SyncStatus` representa o dispositivo:

```ts
type SyncStatus =
  | 'synced'
  | 'pending'
  | 'syncing'
  | 'error'
  | 'conflict';
```

Representação visual:

| Estado | Linguagem | Ação do usuário |
| --- | --- | --- |
| `synced` | “Tudo sincronizado” | nenhuma ação necessária |
| `pending` | “Alteração pendente” | sincronizar agora ou aguardar conexão |
| `syncing` | “Sincronizando alterações…” | aguardar sem bloquear a navegação |
| `error` | “Não foi possível sincronizar agora” | tentar novamente |
| `conflict` | “Conflito para revisar” | abrir revisão; não sobrescrever |

Ao recuperar conectividade, o sync inicia automaticamente. A ação manual `Sincronizar agora` permanece disponível.

## Offline-first

Persistir localmente antes da rede. O design deve mostrar que o trabalho foi preservado, sem afirmar que o servidor já recebeu a operação. Não remover silenciosamente dados pendentes, com erro, em conflito ou evidências não confirmadas.

Concluída e sincronizada pode ser mantida por 30 dias. A política deve ser centralizada e não pode apagar arquivos pendentes.

## Scout

`Scout` é o parceiro de campo do FieldOps: um cão inspetor profissional, confiável e equipado discretamente com elementos de EPI. Não é robô, não representa IA e não deve aparecer em todas as telas.

Usos aprovados:

- empty state: Scout descansando com a prancheta;
- início: Scout colocando o capacete;
- checklist completo: Scout marcando um check;
- evidência: Scout com câmera/prancheta;
- offline: Scout com rádio sem sinal;
- sincronização: Scout transmitindo pelo rádio;
- conclusão: celebração discreta;
- erro/conflito: Scout observando a prancheta com atenção.

Assets vetoriais atuais: `mobile/src/assets/mascot/scout-ready.svg`, `scout-offline.svg` e `scout-success.svg`. Os arquivos devem permanecer simples, com fundo transparente e poucas cores para facilitar substituição.

## Motion design

Animação existe para comunicar mudança de estado:

- entrada curta e escalonada dos cards;
- transição da barra de progresso ao responder item;
- aparecimento de thumbnail ao salvar evidência;
- indicador discreto durante sync;
- check rápido após confirmação;
- sucesso breve na conclusão.

Usar Reanimated ou animações nativas compatíveis com Expo. Evitar loops decorativos, confete excessivo, bloqueio de fluxo e animações que prejudiquem 60 FPS.

## Acessibilidade

- contraste compatível com WCAG 2.1 AA sempre que possível;
- labels para campos e ações;
- leitura sem depender de cor;
- suporte a Dynamic Type quando compatível;
- foco e ordem de leitura coerentes;
- feedback de erro associado ao campo ou ação;
- áreas de toque mínimas de 44 pontos;
- linguagem curta, profissional e humana.

## Referências

- [Arquitetura mobile](mobile.md)
- [Regras de negócio](../01-product/business-rules.md)
- [Escopo do MVP](../01-product/product-mvp.md)
- [ADR-008 — políticas operacionais](../12-decisions/ADR-008-mobile-operational-policies.md)
