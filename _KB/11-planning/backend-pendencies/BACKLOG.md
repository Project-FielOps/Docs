# Backlog de tasks do backend

Este backlog transforma as pendências técnicas em tarefas acompanháveis por humanos e agents. Ele complementa o [README de pendências](README.md): o README explica o contexto e este arquivo controla execução, dependências, evidências e handoffs.

## Estados e fluxo

- `BACKLOG`: tarefa identificada, ainda sem execução autorizada.
- `READY`: escopo e dependências suficientes para iniciar.
- `IN_PROGRESS`: execução em andamento.
- `VALIDATION`: implementação concluída pelo Backend Agent e entregue ao Validation & QA Agent.
- `BLOCKED`: impedida por decisão, acesso, recurso ou outra task; a dependência deve ser registrada.
- `DONE`: validada com evidência e documentação atualizada.

Fluxo padrão: `BACKLOG → READY → IN_PROGRESS → VALIDATION → DONE`. Uma falha de validação retorna a task para `IN_PROGRESS`; uma dependência ausente leva a `BLOCKED`.

O Backend Agent implementa e entrega a task com testes e evidências. O Validation & QA Agent valida de forma independente e pode retornar `APPROVED` ou `CHANGES_REQUIRED`. Compilação isolada não é critério suficiente para `DONE`.

## Visão do backlog

| ID | Task | Prioridade | Responsável | Dependência | Status |
| --- | --- | --- | --- | --- | --- |
| BE-001 | Definir conta, região, domínios e ambientes AWS | P0 | Humano/Arquitetura | — | `BACKLOG` |
| BE-002 | Definir e estruturar IaC | P0 | Backend/Infra | BE-001 | `BLOCKED` |
| BE-003 | Provisionar rede, IAM, RDS, S3, Secrets, ECR e CloudWatch | P0 | Backend/Infra | BE-002 | `BLOCKED` |
| BE-004 | Configurar ALB, ACM, Route 53 e ECS Fargate | P0 | Backend/Infra | BE-003 | `BLOCKED` |
| BE-005 | Validar a API implantada na AWS | P0 | Validation & QA | BE-004 | `BLOCKED` |
| BE-006 | Configurar identidade e envio pelo Amazon SES | P1 | Backend/Infra | conta AWS autorizada | `BLOCKED` |
| BE-007 | Implementar CI da API no GitHub Actions | P0 | Backend Agent | — | `READY` |
| BE-008 | Implementar CD ECR → ECS com OIDC | P0 | Backend/Infra | BE-002, BE-003 | `BLOCKED` |
| BE-009 | Executar testes de carga e medir p95 | P1 | Validation & QA | ambiente executável | `READY` |
| BE-010 | Configurar logs, métricas, alarmes e correlação | P1 | Backend/Infra | BE-004 | `BLOCKED` |
| BE-011 | Validar backup e restauração do RDS | P1 | Validation & QA | BE-003 | `BLOCKED` |
| BE-012 | Validar rollback da API | P1 | Validation & QA | BE-008 | `BLOCKED` |
| BE-013 | Publicar frontend em S3 + CloudFront | P1 | Frontend/Infra | decisão de hosting | `BLOCKED` |
| BE-014 | Executar jornada integrada frontend/API | P1 | Validation & QA | BE-005, BE-013 | `BLOCKED` |
| BE-015 | Integrar mobile com sync e upload | P2 | Mobile/Validation | mobile implementado | `BLOCKED` |

## Detalhamento e critérios de aceite

### BE-001 — contexto AWS

Registrar conta, região, domínios, ambientes, responsáveis, limites de custo e convenções de nomes. A task estará concluída quando houver decisão documentada em `_KB/12-decisions/`, com variáveis esperadas definidas e sem segredos versionados.

### BE-002 — IaC

Escolher a ferramenta, separar módulos e ambientes, definir variáveis, estado remoto e revisão das mudanças. Aceite: plano reproduzível, documentação de execução e aprovação da estratégia.

### BE-003 — recursos base

Provisionar VPC/subnets, grupos de segurança, RDS privado, S3 com bloqueio público e criptografia, Secrets Manager, ECR, logs e roles. Aceite: evidências de recursos, permissões mínimas e conectividade necessária sem exposição pública do banco ou bucket.

### BE-004 — entrada HTTPS e execução

Configurar task definition, ECS Fargate, ALB, ACM e Route 53. Aceite: health check saudável, HTTPS funcional, redirecionamento HTTP quando aplicável e nenhuma credencial embutida na imagem ou task definition.

### BE-005 — validação operacional

Validar health, login, refresh/logout, autorização adversarial, migrations, evidências S3 e recuperação de senha quando o SES estiver disponível. Aceite: matriz de cenários com resultado, evidência e ambiente identificados.

### BE-006 — Amazon SES

Verificar identidade ou domínio, confirmar região, role com `ses:SendEmail` e situação de sandbox. No AWS Academy, a role `voclabs` negou `ses:GetAccount`, `ses:TagResource` e `ses:CreateEmailIdentity`; usar conta autorizada, identidade previamente provisionada ou implementação alternativa de desenvolvimento.

### BE-007 — CI da API

Criar workflow com checkout, Java 21, `mvn clean test`, empacotamento, build e verificação da imagem Docker. Aceite: PR bloqueada quando testes falham, artefato identificado pelo SHA e nenhum segredo no workflow.

### BE-008 — CD AWS

Configurar OIDC do GitHub, push no ECR, atualização da task definition, deploy no ECS, espera de estabilidade, smoke test e rollback. Aceite: permissões mínimas, execução repetível e rollback demonstrado.

### BE-009 — carga

Definir cenários, volume, duração e ambiente; medir p95, erros e saturação. Aceite: relatório reproduzível, com alvo inicial de p95 ≤ 500 ms para operações internas, excluindo transferência de arquivo e dependências externas quando explicitado.

### BE-010 — observabilidade

Configurar logs estruturados, correlation ID, métricas de latência/erro e alarmes, com retenção definida. Aceite: consulta operacional disponível e confirmação de que tokens, senhas, hashes, headers de autorização e credenciais não aparecem nos logs.

### BE-011 — continuidade do RDS

Documentar backup, retenção, restauração e responsáveis. Aceite: restauração testada em ambiente controlado, com tempo observado e evidência armazenada.

### BE-012 — rollback

Testar retorno para uma task definition anterior sem migração destrutiva. Aceite: versão anterior fica saudável, o smoke test passa e o procedimento de retorno está documentado.

### BE-013 — hosting do frontend

Publicar o Angular em S3 privado, CloudFront com OAC e ACM, incluindo cache, fallback de rotas e configuração da URL da API. Aceite: HTTPS, domínio, refresh de rota e CORS funcionando.

### BE-014 — jornada integrada

Executar login, navegação autorizada, operação de inspeção, evidência e logout pelo frontend contra a API implantada. Aceite: cenários críticos passam sem erro de domínio, CORS ou contrato.

### BE-015 — mobile

Implementar e testar SQLite/outbox, sync automático ao recuperar conexão, ação `Sincronizar agora`, retry, conflito com `syncStatus = 'conflict'`, histórico restrito ao técnico autenticado, permissões de câmera/QR/GPS e upload separado. Aceite: integração real ou teste de contrato reproduzível, preservação após reinício e nenhuma remoção silenciosa de dados pendentes, com erro ou em conflito.

## Registro de execução

| ID | Data | Executor | Ação/comando | Resultado/evidência | Próxima ação |
| --- | --- | --- | --- | --- | --- |
| — | — | — | — | — | — |

## Regras para agents

1. Ler `_KB/README.md`, a documentação da área e as decisões relacionadas antes de alterar o projeto.
2. Não iniciar task `BLOCKED` sem registrar a dependência ou decisão que a libera.
3. Atualizar o status somente com evidência verificável.
4. Depois da implementação, executar testes proporcionais ao risco e entregar a task ao Validation & QA Agent.
5. Nunca registrar ou versionar segredos, tokens, senhas ou credenciais AWS.
6. Atualizar a documentação quando houver mudança de contrato, arquitetura, segurança ou operação.

## Referências

- [Pendências e contexto](README.md)
- [Arquitetura AWS](../../08-infrastructure/README.md)
- [Plano de implantação](../../08-infrastructure/deployment-plan.md)
- [Plano de CI/CD](../../09-devops/cicd-plan.md)
- [Decisões pendentes](../../../decisions/pending.md)
- [Diagnóstico do projeto](../../99-reference/diagnostics/discovery.md)
