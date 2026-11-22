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
