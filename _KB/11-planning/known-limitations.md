# Limitações conhecidas (2026-11-26)

Lista exigida nas evidências obrigatórias da avaliação. Cada item é uma limitação aceita para o MVP, não um defeito oculto.

| Área | Limitação | Impacto | Caminho de evolução |
|---|---|---|---|
| Offline | Conflitos são preservados e sinalizados, sem resolução assistida (ADR-008). | O técnico decide "Tentar de novo" ou "Descartar" na aba Sincronização. | Tela de comparação local × servidor. |
| Offline | O aparelho guarda a carteira do técnico logado; trocar de técnico no mesmo aparelho mantém a fila do anterior guardada até ele voltar. | Nada é perdido, mas só sobe quando o dono entra de novo. | Aviso de pendências de outro usuário no login. |
| Offline | Respostas usam "última gravação vence" (só o técnico atribuído edita). | Sem conflito real no MVP. | `baseVersion` por resposta. |
| Infra | API e painel em EC2 (AWS Academy) com PostgreSQL em contêiner e HTTP sem TLS. | O APK libera HTTP (`usesCleartextTraffic`). | RDS, HTTPS (ACM/ALB) e remoção do cleartext. |
| Infra | Evidências em volume local da EC2 por padrão (`FIELDOPS_STORAGE_MODE=local`). | Fotos dependem do disco da instância. | `FIELDOPS_STORAGE_MODE=s3` (já suportado pela API, ADR-005). |
| QR | Divergência entre ativo esperado e lido não é auditada no servidor. | A verificação fica registrada só no aparelho. | Evento de auditoria de leitura de QR (FO-009). |
| Mobile | Testes automatizados cobrem o motor de sync e o E2E contra a API; telas não têm teste de componente nem E2E em Android (Maestro/Detox). | Regressão visual depende de teste manual. | Maestro no fluxo do técnico. |
| Notificações | Sem push; o aparelho descobre mudanças pelo `pull` a cada sincronização. | Atualização depende de abrir o app/ter rede. | Expo Notifications. |
| Segurança | Valores de desenvolvimento (chave JWT e senha do banco de dev) existiram no histórico do Git até 2026-08-31. | Não valem para produção se a EC2 usa segredos próprios. | Confirmar segredos distintos em produção. |
