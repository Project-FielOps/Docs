# Registro de validação integrada do MVP

Este formulário acompanha a execução do [critério ponta a ponta](mvp-integration-plan.md#criterio-final-de-aceite-reproduzivel). Ele organiza evidências por etapa; não substitui testes unitários/contratuais e não declara o MVP aceito antes da execução.

## Identificação da rodada

| Campo | Valor |
|---|---|
| Responsável pela execução | |
| Data e janela | |
| Ambiente/URL (sem credenciais) | |
| Commit API | |
| Commit web | |
| Commit mobile | |
| Versão do banco/migrações | |
| Dispositivo/OS mobile | |
| Navegador/versão | |
| Dados de teste e papéis usados | |

## Pré-condições

- [ ] ambiente e dados de teste identificados; sem dados pessoais reais desnecessários;
- [ ] usuário ADMIN, SUPERVISOR atribuído, técnico atribuído e usuários negativos disponíveis;
- [ ] builds/testes locais registrados em seus planos por aplicação;
- [ ] contratos e bloqueios (`ARC-027`, `IVH-001`, lacunas API) revisados com responsáveis;
- [ ] método seguro de guardar logs e arquivos de evidência definido, removendo tokens e segredos.

## Execução por etapa

Use `PASS`, `FAIL`, `BLOCKED` ou `NOT RUN`. Informar IDs de evidência sem anexar credenciais, tokens, fotos de cliente ou payloads sensíveis. Em caso de `FAIL`/`BLOCKED`, registrar issue e responsável.

| Etapa do aceite | Resultado esperado | Status | Evidência/issue |
|---|---|---|---|
| 1. ADMIN prepara cadastros | Usuários ativos e relações cliente/local/equipamento disponíveis. | `NOT RUN` | |
| 2. SUPERVISOR planeja inspeção | Modelo publicado e atribuição válida; campos inválidos são recusados. | `NOT RUN` | |
| 3. Técnico recebe atribuição | Somente a atribuição do técnico autenticado aparece no mobile. | `NOT RUN` | |
| 4. Técnico inicia | Transição e vínculo são persistidos e visíveis. | `NOT RUN` | |
| 5. Perda de rede | Estado offline fica claro; nenhuma ação pendente desaparece. | `NOT RUN` | |
| 6. Checklist dinâmico | Tipos de resposta e obrigatoriedades do snapshot são respeitados. | `NOT RUN` | |
| 7. Não conformidade | NC é registrada e vinculada ao item correto; retry não duplica. | `NOT RUN` | |
| 8. Evidência | Arquivo permitido é anexado ao item/inspeção; limites e autorização são aplicados. | `NOT RUN` | |
| 9. Reinício do app | Respostas, anexos pendentes e fila são recuperados sem perda. | `NOT RUN` | |
| 10. Conclusão offline | Estado local pendente não antecipa estado oficial do servidor. | `NOT RUN` | |
| 11. Retorno da rede | Conectividade é detectada sem exigir reinstalação ou limpeza local. | `NOT RUN` | |
| 12. Sincronização | Reenvio é idempotente; falha parcial conserva pendências e cursor válido. | `NOT RUN` | |
| 13. Revisão web | Supervisor atribuído consulta respostas e evidências autorizadas. | `NOT RUN` | |
| 14. Decisão | Aprovação/reprovação válida é persistida; motivo é exigido quando aplicável. | `NOT RUN` | |
| 15. Decisão chega ao mobile | Técnico recebe a alteração pelo mecanismo incremental contratado; cursor não perde eventos. | `NOT RUN` | |
| 16. Correção após rejeição | Técnico atribuído reabre explicitamente, corrige e reenvia; histórico anterior permanece. | `NOT RUN` | |
| 17. Falha/conflito | Pendências, erro e conflito são visíveis e preservados; acesso fora de escopo falha com segurança. | `NOT RUN` | |

## Resultado da rodada

| Campo | Valor |
|---|---|
| Resultado geral (`PASS`/`FAIL`/`BLOCKED`/`NOT RUN`) | |
| Etapas com falha ou bloqueio | |
| Issues abertas/relacionadas | |
| Responsável e próxima ação | |
| Revisor e data | |

A rodada só pode resultar em `PASS` quando todas as etapas aplicáveis forem aprovadas, cada uma tiver evidência rastreável e não houver bloqueio aberto de P0. Repetir este registro para cada ambiente ou rodada de correções. O aceite técnico não substitui aprovação de produto/Scrum Master.

## Documentos complementares

- [Plano integrado e sequência de aceite](mvp-integration-plan.md)
- [Plano de validação web/API](web-pendencies/WEB-API-VALIDATION-PLAN.md)
- [Checklist de auditoria de acessibilidade](web-pendencies/WEB-ACCESSIBILITY-AUDIT.md)
- [Requisitos não funcionais e evidências](../01-product/requirements-non-functional.md)
