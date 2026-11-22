# Validação Mobile — Design System e Expo SDK 57

**Data:** 2026-09-06  
**Executor:** FieldOps Mobile Validation & QA Agent  
**Escopo:** atualização visual, fontes, Scout, dependências Expo SDK 57, correção de rotas e compatibilidade iOS/Fabric

## Veredito

`APPROVED_WITH_WARNINGS`

Os gates automatizados e os bundles multiplataforma passaram. A validação visual em dispositivo físico iOS/Android não foi executada neste ambiente. O erro `topSvgLayout` foi tratado atualizando `react-native-svg` para a versão recomendada pelo SDK 57.

## Matriz requisito → implementação → evidência

| Requisito | Implementação | Evidência | Resultado |
| --- | --- | --- | --- |
| Design System v1 documentado | `_KB/03-architecture/DESIGN_SYSTEM.md` | Documento canônico criado e referenciado pela arquitetura mobile | Aprovado |
| Tokens visuais aplicados | `mobile/src/theme/index.ts` | Verde `#16A66A`, off-white, azul-petróleo, estados semânticos, spacing/radius/motion | Aprovado |
| Tipografia Nunito Sans | `mobile/app/_layout.tsx`, `@expo-google-fonts/nunito-sans` | Fontes 400/500/600/700/800 carregadas antes da navegação | Aprovado |
| Ícones consistentes | `mobile/app/(tabs)/_layout.tsx` | Lucide React Native nas quatro tabs | Aprovado |
| Bottom navigation flutuante | `mobile/app/(tabs)/_layout.tsx` | Barra escura, arredondada, com margens e item ativo contrastante | Aprovado estaticamente |
| Scout 2D | `mobile/src/assets/mascot/mascote.png`, `src/components/scout.tsx` | PNG oficial transparente e componente contextual para offline, revisão e sucesso | Aprovado estaticamente |
| Scout contextual na revisão | `mobile/app/inspections/[id]/review.tsx` | Estados de sucesso e pendência usam Scout com mensagem acionável | Aprovado estaticamente |
| Fonte padronizada | `mobile/app/_layout.tsx`, `src/theme/index.ts` | Nunito Sans 400/500/600/700/800 carregada globalmente; tabs e logo também usam Nunito Sans | Aprovado |
| Ícones padronizados | `mobile/app/(tabs)/_layout.tsx` | Lucide React Native em todas as tabs | Aprovado |
| Rota de detalhes sem duplicidade | `mobile/app/inspections/[id]/index.tsx` | Bundle Web sem erro de Screen duplicada | Aprovado |
| Expo SDK 57 | `mobile/package.json` | Expo 57.0.20, Router 57.0.19, RN 0.86.0, React 19.2.3 | Aprovado |
| Web | dependências `react-native-web`, `expo-linking`, `buffer` | `expo export --platform web` concluído | Aprovado |
| Android | configuração e bundle Expo | `expo export --platform android` concluído | Aprovado |
| iOS/Fabric | `react-native-svg@15.15.4` | `expo export --platform ios` concluído sem erro `topSvgLayout` | Aprovado em bundle |

## Comandos executados

```text
npm run typecheck                         PASS
EXPO_NO_TELEMETRY=1 npm run lint         PASS
EXPO_NO_TELEMETRY=1 npx expo export --platform web --output-dir /tmp/fieldops-mobile-web      PASS
EXPO_NO_TELEMETRY=1 npx expo export --platform android --output-dir /tmp/fieldops-mobile-android  PASS
EXPO_NO_TELEMETRY=1 npx expo export --platform ios --output-dir /tmp/fieldops-mobile-ios      PASS
CI=1 EXPO_NO_TELEMETRY=1 npm start       Metro iniciou e permaneceu executando
```

## Limitações e próximos testes

- validar no iPhone com Expo Go compatível com SDK 57 e iOS 16.4+; o bundle iOS foi validado, mas o dispositivo físico não está disponível neste ambiente;
- validar visualmente a barra flutuante, fontes, safe areas e assets Scout em telas reais;
- validar no Android 10/API 29 ou superior;
- executar os cenários funcionais do mobile após SQLite/outbox e integração com a API;
- os avisos restantes do npm são vulnerabilidades transitivas e não bloquearam os bundles.
