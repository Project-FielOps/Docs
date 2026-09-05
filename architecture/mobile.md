# Arquitetura do aplicativo mobile

## Status e responsabilidade

O aplicativo Expo/React Native é planejado pelo backlog e ainda não existe neste workspace. Ele é a experiência do técnico: baixar inspeções atribuídas, executar checklist, capturar foto, ler QR Code, registrar localização e operar sem rede. Planejamento, revisão e administração permanecem no painel web. DEC-002 restringe a execução ao técnico atribuído; DEC-003 exige reabertura explícita após reprovação.

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

Use Secure Store para tokens. Dados operacionais ficam em SQLite, incluindo inspeções baixadas, respostas, evidências pendentes e outbox. Permissões de câmera e localização devem ser solicitadas no momento de uso, explicar a finalidade e oferecer estado de falha compreensível.

## Offline-first

Toda alteração persiste localmente antes de tentar rede. A outbox guarda ID idempotente, tipo de operação, payload, dependências, tentativas e estado. O envio respeita dependências; o pull usa cursor e só avança depois que o banco local persiste o resultado. Não descarte alterações locais silenciosamente: respostas tardias para inspeções aprovadas ou versões divergentes devem aparecer como conflito.

O contrato aprovado está em [synchronization.md](../specs/synchronization.md). Política de conflito operacional, tentativas e retenção da idempotência continuam pendentes.
