# Especificação de modelos, execução e revisão

## Fluxo principal

Supervisor autorizado monta rascunho, publica versão, agenda inspeção e atribui técnico ativo. A API cria `InspectionItemSnapshot`. O técnico atribuído inicia, responde cada item, salva localmente, registra evidências/NC e envia. O supervisor atribuído inicia revisão e aprova ou reprova com motivo. Uma reprovação permite ação explícita de reabertura para `IN_PROGRESS`; o técnico corrige o checklist inteiro e reenvia, preservando o histórico anterior.

## Estados

`ASSIGNED → IN_PROGRESS → SUBMITTED → UNDER_REVIEW → APPROVED`; `UNDER_REVIEW → REJECTED → IN_PROGRESS`; `ASSIGNED|IN_PROGRESS|REJECTED → CANCELED` com motivo. `APPROVED` é somente leitura. Regras RN-024..044 e RN-079..085, DEC-002/003.

## Validação e erros

Obrigatórios, observação e evidência em não conformidade seguem o snapshot. Respostas devem ser compatíveis com `responseType`; tipos desconhecidos bloqueiam publicação/execução conforme RN-023. Erros esperados incluem `INVALID_TRANSITION`, `REQUIRED_ITEM_MISSING`, `OBSERVATION_REQUIRED`, `EVIDENCE_REQUIRED`, `INSPECTION_LOCKED` e `ACCESS_DENIED`.

## Recursos nativos

QR divergente não confirma o vínculo; o técnico identifica manualmente o equipamento previsto. GPS é pontual, opcional e sua ausência é registrada. Foto é a evidência principal no MVP; vídeo, documento, assinatura e plano de ação da ficha são históricos/futuros.

## Dependências

Cadastros, versão publicada, API autorizada, SQLite/outbox e armazenamento de evidências. O código atual implementa parte online e deve ser alinhado às decisões antes do mobile.
