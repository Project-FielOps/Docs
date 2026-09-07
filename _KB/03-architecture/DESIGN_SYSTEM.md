# FieldOps Mobile — Design System v1

**Status:** aprovado para o MVP mobile  
**Escopo:** aplicativo do técnico de campo  
**Tema:** Light Mode

## Identidade

O FieldOps Mobile combina campo, segurança, eficiência e confiança. A interface deve ser operacional, acolhedora e moderna, com composição limpa, fundos claros levemente quentes, cards amplos, whitespace, navegação inferior flutuante e poucas cores por tela.

O verde FieldOps é a cor mais reconhecível da marca. Azul-petróleo e grafite fornecem contraste. O Scout 2D faz parte da identidade, mas complementa a interface e nunca substitui informação ou ação.

Não usar glassmorphism, gradientes chamativos, neon, estética cyberpunk, dashboard corporativo denso ou linguagem visual de produto de IA.

## Tokens

### Cores de marca

```ts
export const colors = {
  primary: '#16A66A',
  primaryPressed: '#0E8755',
  primaryDark: '#075E4A',
  primarySoft: '#E7F7F0',
  primarySubtle: '#F2FAF6',

  background: '#F7F8F6',
  surface: '#FFFFFF',
  surfaceSecondary: '#F1F3F1',

  textPrimary: '#102A2E',
  textSecondary: '#647274',
  textTertiary: '#8E999A',

  border: '#E2E7E4',
  borderStrong: '#CDD5D1',
  disabled: '#B8C0BD',

  success: '#16A66A',
  successSoft: '#E7F7F0',
  warning: '#E6A23C',
  warningSoft: '#FFF5E3',
  error: '#D94C4C',
  errorSoft: '#FDECEC',
  info: '#397C9A',
  infoSoft: '#EAF4F8',
  offline: '#647274',
  offlineSoft: '#EEF1F0',
  white: '#FFFFFF',
} as const;
```

Offline não usa vermelho: ausência de conexão não é necessariamente erro.

### Sincronização

```ts
export const syncColors = {
  synced: { foreground: '#16A66A', background: '#E7F7F0' },
  pending: { foreground: '#B7791F', background: '#FFF5E3' },
  syncing: { foreground: '#397C9A', background: '#EAF4F8' },
  error: { foreground: '#D94C4C', background: '#FDECEC' },
  conflict: { foreground: '#A855F7', background: '#F5ECFF' },
} as const;
```

Todo status usa ícone + texto + cor: `✓ Sincronizado`, `↻ Sincronizando`, `◷ Envio pendente`, `! Falha ao sincronizar` e `! Conflito`.

### Tipografia

Usar Inter, com `@expo-google-fonts/inter`, nos pesos 400, 500, 600 e 700.

```ts
export const typography = {
  display: { fontFamily: 'Inter_700Bold', fontSize: 36, lineHeight: 43 },
  h1: { fontFamily: 'Inter_700Bold', fontSize: 30, lineHeight: 36 },
  h2: { fontFamily: 'Inter_600SemiBold', fontSize: 24, lineHeight: 30 },
  h3: { fontFamily: 'Inter_600SemiBold', fontSize: 20, lineHeight: 26 },
  title: { fontFamily: 'Inter_600SemiBold', fontSize: 18, lineHeight: 24 },
  bodyLarge: { fontFamily: 'Inter_400Regular', fontSize: 16, lineHeight: 24 },
  body: { fontFamily: 'Inter_400Regular', fontSize: 14, lineHeight: 21 },
  label: { fontFamily: 'Inter_500Medium', fontSize: 14, lineHeight: 20 },
  caption: { fontFamily: 'Inter_400Regular', fontSize: 12, lineHeight: 17 },
} as const;
```

Informações importantes não devem usar menos de 14 px. Evitar usar `700` em todos os textos.

### Espaçamento, raios e motion

```ts
export const spacing = { xs: 4, sm: 8, md: 12, base: 16, lg: 20, xl: 24, '2xl': 32, '3xl': 40, '4xl': 48 } as const;
export const radius = { xs: 6, sm: 10, md: 14, lg: 18, xl: 24, '2xl': 32, full: 999 } as const;
export const motion = { instant: 100, fast: 160, normal: 240, slow: 360 } as const;
```

Margem horizontal padrão: 20 px. Em telas pequenas: 16 px. Cards usam 16–20 px de padding e 18–24 px de raio.

## Componentes

- **Botões:** altura 52 px, raio 16 px, Inter SemiBold 16. Primary `#16A66A`, pressed `#0E8755`, secondary `#E7F7F0`, outline transparente com `borderStrong`, danger `#D94C4C`.
- **Inputs:** mínimo 52 px, padding horizontal 16 px, raio 14 px, focus em `primary` e erro em `error`. Labels devem permanecer visíveis.
- **Cards:** superfície branca, borda `#E2E7E4`, raio 20 px, separação por espaço/superfície/borda. Sombras só quando houver elevação real, com opacidade aproximada de 0.05.
- **Chips:** raio full; filtro normal em `surfaceSecondary`, selecionado em `textPrimary` com texto branco, ou verde suave para filtros operacionais.
- **Progress:** 8 px, track `#E7EBE9`, preenchimento `primary`, raio full e transição de 160–240 ms.
- **Checklist:** estado pendente neutro, concluído com fundo/borda verde suave e problema com atenção suave. Nunca preencher o card inteiro de vermelho ou verde.

## Navegação

Usar bottom tabs para `Home`, `Inspeções`, `Histórico` e `Perfil`. A barra deve ser flutuante, escura (`#102A2E`), com raio 28 px, altura 68–72 px e margens laterais de 16 px. O item ativo usa superfície branca e ícone escuro; os demais usam branco com 70–80% de opacidade.

Usar Lucide React Native com stroke 2: 24 px na navegação, 20 px em ações, 18 px em componentes e 16 px em badges. Não misturar bibliotecas de ícones.

## Scout

Scout é o parceiro de campo do FieldOps: cão inspetor 2D flat, profissional, confiável, com capacete/colete e elementos discretos de EPI. Não é infantil, robô ou símbolo de IA.

```ts
export const mascot = {
  furPrimary: '#B96D32', furDark: '#75401F', furLight: '#F4D3A5', muzzle: '#FFF3E1',
  helmet: '#F8F9F7', helmetShadow: '#DDE3DF', vest: '#16A66A', vestDark: '#075E4A',
  reflective: '#B9D94A', backpack: '#153E43', outline: '#102A2E', nose: '#102024', eyes: '#102024',
} as const;
```

Estilo: flat vector, formas arredondadas, contornos limpos, sombra mínima, fundo transparente e poucas cores. Usos: boas-vindas, empty state, offline, sincronização, conclusão, atenção e onboarding. Não usar em todos os cards, itens ou modais.

Assets versionados em `mobile/src/assets/mascot/`.

## Motion e estados

Animações comunicam feedback: cards entram com `opacity 0 → 1` e `translateY 8 → 0`; checklist anima check e progresso; thumbnail confirma evidência; sync gira discretamente e retorna ao neutro após confirmação; Scout pode piscar ou mover a pata, sem movimento permanente.

Todas as telas devem tratar `loading`, `loaded`, `empty`, `error`, `offline`, `pending`, `syncing` e `conflict`. A animação não pode bloquear o usuário.

## Acessibilidade

Touch targets mínimos de 44 × 44 px, preferencialmente 48 px; ações principais de 52–56 px; contraste compatível com WCAG 2.1 AA; labels para campos; ordem de foco coerente; Dynamic Type quando possível; mensagens que expliquem o problema, preservação dos dados e próxima ação. Nunca depender apenas de cor.

## Regra de aplicação

O design system define aparência e interação, não altera regras de negócio. O app deve seguir os estados oficiais e as permissões descritas em [business-rules.md](../01-product/business-rules.md), mantendo `InspectionStatus` separado de `SyncStatus`.
