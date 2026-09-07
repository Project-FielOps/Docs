# Arquitetura do aplicativo mobile

O conceito visual e os padrões de interação estão definidos em [FieldOps Mobile — Design System v1](DESIGN_SYSTEM.md) e detalhados em [mobile-design.md](mobile-design.md).

## Status e responsabilidade

O aplicativo Expo SDK 57 / React Native é a experiência do técnico: baixar inspeções atribuídas, executar checklist, capturar foto, ler QR Code, registrar localização e operar sem rede. Planejamento, revisão e administração permanecem no painel web. DEC-002 restringe a execução ao técnico atribuído; DEC-003 exige reabertura explícita após reprovação. O suporte nativo do produto começa em Android 10/API 29; no iOS, o SDK 57 exige iOS 16.4 ou superior.

## Estrutura proposta

```text
app/                 rotas Expo Router
features/            casos de uso por domínio
application/         orquestração de casos de uso
domain/              entidades e regras sem dependência de UI
infrastructure/      api, database, repositories, storage e sync
components/          componentes reutilizáveis
design-system/       tokens e componentes visuais
schemas/, hooks/, utils/, config/
```

Use Secure Store para tokens. Dados operacionais ficam em SQLite, incluindo inspeções baixadas, respostas, evidências pendentes e outbox. Permissões de câmera e localização devem ser solicitadas no momento de uso, explicar a finalidade e oferecer estado de falha compreensível. O suporte mínimo previsto é Android 10 / API 29 ou superior. O mascote do produto é Scout, usado somente em estados contextuais.

## Offline-first

Toda alteração persiste localmente antes de tentar rede. A outbox guarda ID idempotente, tipo de operação, payload, dependências, tentativas e estado. O envio respeita dependências; o pull usa cursor e só avança depois que o banco local persiste o resultado. A sincronização inicia automaticamente ao recuperar a conexão e também pode ser acionada por `Sincronizar agora`. Não descarte alterações locais silenciosamente: respostas tardias para inspeções aprovadas ou versões divergentes devem aparecer como conflito, preservando o valor local e usando `syncStatus = 'conflict'`.

O status oficial da inspeção é independente do status local de sincronização. Uma conclusão offline deve ser representada como `status = 'IN_PROGRESS'`, `syncStatus = 'pending'` e `locallyCompletedAt`; somente a confirmação do servidor altera o estado oficial. A exigência de localização é configurável por `requiresLocation` e, nos mocks, permanece `false`.

O histórico é filtrado pelo técnico autenticado. Inspeções concluídas e sincronizadas podem ser mantidas por 30 dias antes da limpeza; dados pendentes, com erro, em conflito e evidências não confirmadas não podem ser removidos automaticamente.

O contrato aprovado está em [synchronization-contract.md](../05-api/specs/synchronization-contract.md). A resolução automática de conflitos não faz parte do MVP: o cliente apenas preserva e sinaliza o conflito para evolução posterior do backend/API.
