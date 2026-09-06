# Escopo do MVP

Referência: capítulos textuais 01, 08, 15, 17 e Anexo B da base preservada. A primeira entrega funcional deve demonstrar uma inspeção desde o planejamento na web até execução offline no Android, sincronização e decisão do supervisor.

## MVP obrigatório

- Fundação executável, autenticação, renovação e encerramento de sessão; autorização real na API.
- Gestão de usuários por administrador; clientes, locais e equipamentos por administrador/supervisor.
- Modelo com seções, itens, regras, prévia, versão publicada e snapshot na inspeção.
- Agendamento com cliente, local, técnico ativo, supervisor, prioridade, prazo e instruções; equipamento conforme tipo de inspeção; cancelamento justificado.
- Técnico consulta somente suas inspeções, inicia, responde checklist dinâmico, registra observações, fotos e NC, confirma equipamento por QR e coleta localização pontual mediante permissão. Evidências aceitas: JPEG/JPG, PNG e PDF, até 10 MB por arquivo e 10 por inspeção.
- Download prévio, SQLite operacional, retomada após reinício, outbox, idempotência, sincronização com resultado por operação e conflito explícito.
- Supervisor acompanha, vê respostas/fotos/NC/localização, inicia revisão, aprova ou reprova com motivo; técnico recebe correção. O dashboard mínimo é obrigatório: pendentes, enviadas, aprovadas/rejeitadas e aguardando revisão.
- Auditoria mínima, erros padronizados, migrações, OpenAPI, testes críticos, build Android e demonstração integrada.

Os 72 PBI P0 estão ligados a requisitos e aceites em [rastreabilidade](../requirements/traceability.md). As lacunas de implementação não retiram itens do MVP.

## SHOULD HAVE

Itens P1: notificações locais/push, assinatura desenhada, PDF básico, histórico detalhado de respostas, comentários por item, biometria, tema escuro e CSV. Reatribuição, comparação de inspeções e suporte assistido a conflitos também constam nas funcionalidades P1 da base.

Dashboard exige resolução de DEC-009: é P1 no backlog, mas há mínimo descrito como MVP na página 14. O código já contém resumo; isso não autoriza ampliar indicadores. A preservação do histórico após correção (RN-083) continua regra obrigatória, mesmo que a tela detalhada de histórico seja P1.

## FUTURE

Portal do cliente, multiempresa, planos de ação, ordens de serviço, manutenção preventiva, assinatura avançada, geofencing, roteirização, áudio/vídeo, OCR, IA, IoT, ERP, webhooks e relatórios regulatórios específicos. Campos e funções desses recursos na ficha ou diagrama são referências históricas.

## Controle de escopo

Manter um técnico responsável por inspeção, um fluxo de revisão, tipos essenciais e fotografia como evidência principal. A ordenação do construtor pode usar botões. Se necessário, aplicar a ordem de corte da base: dashboard avançado, tipos não essenciais, galeria, drag-and-drop, resolução complexa de conflitos, notificações/PDF/assinatura. Não cortar silenciosamente autenticação, planejamento, checklist, foto, offline, sync e revisão.
