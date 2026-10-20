# Requisitos não funcionais

| ID | Requisito verificável | Evidência esperada |
|---|---|---|
| NFR-001 | Dados locais pendentes sobrevivem a fechamento/reinício | Teste mobile de SQLite/outbox |
| NFR-002 | API retorna erro estruturado sem segredo, stack trace ou token | Teste de contrato e inspeção de logs |
| NFR-003 | Operações críticas são autorizadas no servidor por perfil e vínculo | Testes 401/403 por objeto |
| NFR-004 | Listagens administrativas usam paginação | Resposta `PageResponse` e teste de integração |
| NFR-005 | Sync é idempotente e cursor só avança após persistência local | Testes de replay e falha parcial |
| NFR-006 | Tráfego publicado usa HTTPS; segredos vêm de configuração externa | Configuração AWS e revisão de release |
| NFR-007 | Evidências têm acesso autorizado, JPEG/JPG/PNG/PDF, máximo 10 MB/arquivo e 10/inspeção | Teste de validação, acesso e bucket |
| NFR-008 | Web trata loading, vazio, erro, permissão e ação em processamento | Testes de componentes e revisão visual |
| NFR-009 | Mobile Android trata permissões negadas sem perda de trabalho | Teste em dispositivo/emulador |
| NFR-010 | Migrações Flyway são versionadas e schema validado | `mvn test`/subida limpa |
| NFR-011 | OpenAPI, READMEs e modelos permanecem sincronizados com o contrato | Revisão cruzada |
| NFR-012 | A entrega AWS possui ambientes, logs, health check, backup e acesso privado documentados | Evidência de deploy e runbook |
| NFR-013 | Telas web principais buscam WCAG 2.1 AA | Auditoria de acessibilidade |
| NFR-014 | Operações comuns da API atingem p95 ≤ 500 ms, excluindo upload/download e dependências externas | Teste de carga controlado |
| NFR-015 | Logs são estruturados, possuem correlation ID, registram erros e API expõe health check | Inspeção de logs e `/actuator/health` |

Os limites e metas acima são definidos por DEC-004, DEC-005, DEC-010 e DEC-011. A exclusão física automática de arquivos não é exigida no MVP.

## Rastreabilidade e gate de verificação

Todos os requisitos abaixo são gates do MVP. O componente responsável prepara a evidência; QA registra o resultado e o ambiente. Código ou endpoint existente, sem execução e resultado registrados, não equivale a aceite.

| ID | Superfície responsável | Gate de verificação |
|---|---|---|
| NFR-001 | Mobile | Fechar e reabrir o app com operações pendentes; comparar respostas, evidências e fila antes/depois. |
| NFR-002 | API | Exercitar erros de validação e autorização; verificar payload e logs sem stack trace, segredo ou token. |
| NFR-003 | API | Matriz 401/403 por perfil, responsável e objeto; incluir acesso cruzado entre inspeções. |
| NFR-004 | API | Verificar metadados de paginação, limites e navegação em listagens administrativas. |
| NFR-005 | API + mobile | Repetir operações e simular falha parcial; comprovar ausência de duplicação e cursor só após persistência. |
| NFR-006 | Plataforma | Revisar configuração do ambiente publicado, redirecionamento HTTPS e origem dos segredos; guardar referência não sensível ao deploy. |
| NFR-007 | API + mobile | Cobrir formatos/tamanhos válidos e inválidos, limite por inspeção, acesso autorizado e persistência. |
| NFR-008 | Web | Testar loading, vazio, sem resultados, erro, permissão e mutação em processamento em telas críticas. |
| NFR-009 | Mobile | Negar cada permissão em dispositivo/emulador; comprovar continuidade do trabalho e ausência de perda de dados. |
| NFR-010 | API | Subir banco limpo com Flyway, validar schema e executar a suíte automatizada. |
| NFR-011 | Todas | Comparar OpenAPI, DTOs, READMEs e contratos consumidos; registrar divergências e correções. |
| NFR-012 | Plataforma | Conferir ambientes, logs, health check, backup e controles de acesso no runbook e no ambiente implantado. |
| NFR-013 | Web | Executar checklist WCAG 2.1 AA com método, ambiente, evidência e resultado por critério; ver checklist de [auditoria de acessibilidade](../11-planning/web-pendencies/WEB-ACCESSIBILITY-AUDIT.md). |
| NFR-014 | API | Medir p95 em carga controlada, identificando dataset, concorrência, duração e exclusões aplicadas. |
| NFR-015 | API + plataforma | Correlacionar requisição de ponta a ponta, inspecionar logs estruturados e consultar `/actuator/health`. |

### Registro mínimo de evidência

Para cada execução, registrar ID do requisito, resultado (`PASS`, `FAIL`, `BLOCKED` ou `NOT RUN`), data, versão/commit, ambiente, passos, resultado observado e link para artefato não sensível. Em caso de falha ou bloqueio, registrar responsável e próxima ação. Não anexar tokens, dados pessoais desnecessários, segredos ou evidências de cliente sem autorização.
