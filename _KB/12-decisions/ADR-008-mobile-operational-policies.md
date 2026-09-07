# ADR-008 — Políticas operacionais do mobile no MVP

## Status

Accepted

## Contexto

O aplicativo mobile precisa operar em campo com conectividade intermitente, preservar alterações locais e comunicar claramente a diferença entre o estado oficial da inspeção e o estado de sincronização do dispositivo.

## Decisões

- A sincronização será automática ao recuperar a conexão e também poderá ser iniciada por `Sincronizar agora`.
- Alterações offline serão mantidas na fila local até confirmação do servidor, com retry controlado.
- Conflitos não serão resolvidos automaticamente no MVP. A alteração local será preservada e marcada como `syncStatus = 'conflict'`.
- O técnico receberá mensagem explícita quando houver conflito: `Esta inspeção foi alterada em outro local. Revise as alterações antes de continuar.`
- Câmera poderá ser substituída por seleção de imagem existente; QR Code poderá ser informado manualmente; localização poderá ser recusada quando não for exigida pela inspeção.
- A exigência de localização será configurável por inspeção/template com `requiresLocation`; os mocks usarão `false` até definição do backend.
- Notificações serão locais/mockadas no MVP. Push fica preparado para evolução futura.
- O histórico exibirá somente inspeções relacionadas ao técnico autenticado.
- Uma inspeção reprovada exigirá ação explícita de reabertura quando permitida. Sem essa permissão, será apresentada como somente leitura.
- Dados operacionais necessários ao offline serão persistidos localmente. Inspeções concluídas e sincronizadas poderão ser limpas após 30 dias; dados pendentes, com erro, em conflito ou evidências não confirmadas não serão removidos automaticamente.
- O mascote oficial será `Scout`, descrito como parceiro de campo do FieldOps.
- O suporte oficial mínimo será Android 10 / API 29 ou superior, salvo limitação documentada do Expo adotado.
- O estado oficial da inspeção permanece separado do estado local. Uma conclusão offline usa `status = 'IN_PROGRESS'`, `syncStatus = 'pending'` e `locallyCompletedAt`; somente a confirmação da API altera o status oficial para `SUBMITTED` ou outro estado retornado pelo servidor.

## Modelo de estado

```ts
type SyncStatus =
  | 'synced'
  | 'pending'
  | 'syncing'
  | 'error'
  | 'conflict';

interface Inspection {
  status: InspectionStatus;
  syncStatus: SyncStatus;
  locallyCompletedAt?: string;
  requiresLocation: boolean;
}
```

`InspectionStatus` representa o workflow oficial (`ASSIGNED`, `IN_PROGRESS`, `SUBMITTED`, `UNDER_REVIEW`, `APPROVED`, `REJECTED` e `CANCELED`). `SyncStatus` representa conectividade e sincronização local; um não deve ser usado para substituir o outro.

## Consequências

- O mobile precisará de SQLite, outbox persistente, monitor de conectividade e sincronizador com execução automática e manual.
- A limpeza local deverá ser centralizada e protegida por regras de retenção.
- O backend continuará sendo a fonte oficial após sincronização.
- A resolução de conflitos permanece uma evolução do backend/API; o MVP apenas preserva e sinaliza o conflito.

## Referências

- [Arquitetura mobile](../03-architecture/mobile.md)
- [Regras de negócio](../01-product/business-rules.md)
- [Contrato de sincronização](../05-api/specs/synchronization-contract.md)
- [ADR-006 — mobile e sincronização](ADR-006-mobile-and-sync.md)
