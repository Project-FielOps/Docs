# Requisitos não funcionais

| ID | Requisito verificável | Evidência esperada |
|---|---|---|
| NFR-001 | Dados locais pendentes sobrevivem a fechamento/reinício | Teste mobile de SQLite/outbox |
| NFR-002 | API retorna erro estruturado sem segredo, stack trace ou token | Teste de contrato e inspeção de logs |
| NFR-003 | Operações críticas são autorizadas no servidor por perfil e vínculo | Testes 401/403 por objeto |
| NFR-004 | Listagens administrativas usam paginação | Resposta `PageResponse` e teste de integração |
| NFR-005 | Sync é idempotente e cursor só avança após persistência local | Testes de replay e falha parcial |
| NFR-006 | Tráfego publicado usa HTTPS; segredos vêm de configuração externa | Configuração AWS e revisão de release |
| NFR-007 | Evidências têm acesso autorizado e armazenamento privado | Teste de acesso e configuração de bucket |
| NFR-008 | Web trata loading, vazio, erro, permissão e ação em processamento | Testes de componentes e revisão visual |
| NFR-009 | Mobile Android trata permissões negadas sem perda de trabalho | Teste em dispositivo/emulador |
| NFR-010 | Migrações Flyway são versionadas e schema validado | `mvn test`/subida limpa |
| NFR-011 | OpenAPI, READMEs e modelos permanecem sincronizados com o contrato | Revisão cruzada |
| NFR-012 | A entrega AWS possui ambientes, logs, health check, backup e acesso privado documentados | Evidência de deploy e runbook |

Limites de arquivo, TTL, retenção, metas de latência e nível WCAG permanecem DEC-004, DEC-005, DEC-010 e DEC-011. Não foram inventados números.
