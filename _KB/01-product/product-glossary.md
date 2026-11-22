# Glossário de produto e contrato

| Termo | Significado |
|---|---|
| Cliente | Organização atendida pela prestadora; não é automaticamente usuário do sistema |
| Local | Unidade, área ou instalação pertencente a um cliente |
| Equipamento | Ativo pertencente a um local, identificado por QR único |
| Modelo | Identidade reutilizável do checklist |
| Versão publicada | Conteúdo publicado e preservado para execução |
| Snapshot | Cópia dos itens da versão, criada ao agendar a inspeção |
| Resposta atual | Valor do técnico para um item do snapshot; no máximo uma por item |
| Revisão | Decisão de supervisor, registrada por ciclo; não substitui histórico de respostas |
| Evidência | Arquivo e metadados vinculados à inspeção e opcionalmente à resposta/NC |
| NC | Não conformidade registrada na inspeção; não equivale a um plano de ação |
| Outbox | Fila persistente local de operações não confirmadas pela API |
| Cursor | Marcador de alterações recebidas; só avança após persistência local |
| Idempotência | Repetir uma operação não duplica seus efeitos |
| Versão otimista | Versão de concorrência da entidade, distinta do número publicado do modelo |

## Estados em português e valores do contrato observado

| Apresentação | API e enum Java |
|---|---|
| Rascunho | DRAFT |
| Atribuída | ASSIGNED |
| Em andamento | IN_PROGRESS |
| Enviada | SUBMITTED |
| Em revisão | UNDER_REVIEW |
| Aprovada | APPROVED |
| Reprovada | REJECTED |
| Cancelada | CANCELED |

Os valores ingleses já são usados no código; os portugueses descrevem o negócio. DRAFT consta no enum, mas o endpoint atual de criação já produz ASSIGNED. “Aguardando sincronização” é apresentação do estado local, não um novo estado de negócio da API. Reabertura após reprovação depende de DEC-003.

A base diferencia estado de negócio e sincronização (SINCRONIZADO, PENDENTE, SINCRONIZANDO, ERRO, CONFLITO). O formato de transporte desses estados locais ainda não é contrato publicado.
