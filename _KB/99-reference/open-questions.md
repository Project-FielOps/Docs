# Questões abertas

> Documento histórico de diagnóstico. As questões de sincronização, sessão, storage e QR/GPS foram substituídas ou resolvidas por decisões posteriores. Consulte `_KB/12-decisions/` e `Docs/decisions/pending.md` para o estado vigente.

## CRITICAL

### Pergunta 1 — Como a autorização por objeto será validada de forma consistente?

#### Contexto
A base documental exige autorização por perfil e por objeto, mas a implementação atual pode permitir acesso mais amplo em alguns endpoints.

#### Por que precisamos dessa informação
Sem essa regra clara, a API pode permitir acesso indevido ou gerar comportamento inconsistente em produção.

#### Impacto caso permaneça indefinido
Alta chance de falhas de segurança e de regras do MVP não cumpridas.

#### Possíveis interpretações
- o backend deve validar o vínculo do recurso antes de qualquer leitura ou escrita;
- o painel deve ocultar ações e consultas não permitidas;
- a validação por objeto é obrigatória em toda operação protegida.

#### Recomendação
Manter a regra documental como obrigatória e registrar qualquer divergência do código como correção pendente.

### Pergunta 2 — Qual é o contrato final de sincronização mobile?

#### Contexto
A documentação original exige sincronização, mas usam-se diferentes modelos e o código atual não oferece contrato definitivo.

#### Por que precisamos dessa informação
Sem contrato formal, a sincronização pode produzir duplicidade, perda de dados ou conflito de estado.

#### Impacto caso permaneça indefinido
Impacto crítico para o MVP funcional e para a execução em campo.

#### Possíveis interpretações
- push e pull separados;
- idempotência por operationId;
- cursor de sincronização e reprocessamento;
- upload separado para evidências.

#### Recomendação
Documentar o contrato e manter a implementação atual como AS-IS incompleto.

### Pergunta 3 — Como a sessão JWT será invalidada?

#### Contexto
A base documental exige política de invalidação e logout, com tokens e credenciais protegidos.

#### Por que precisamos dessa informação
A segurança e a sessão de usuário dependem desta política.

#### Impacto caso permaneça indefinido
Risco de sessão prolongada ou revogação insuficiente.

#### Possíveis interpretações
- blacklist de tokens;
- refresh token e rotação;
- invalidation por usuário e por senha;
- política de desconexão no backend.

#### Recomendação
Definir formalmente antes de promover a regra como ativa.

## HIGH

### Pergunta 4 — Como o storage de evidências será implementado em AWS?

#### Contexto
A arquitetura do projeto exige AWS e storage em objeto.

#### Por que precisamos dessa informação
Arquivos e fotos precisam de armazenamento durável e seguro, com rastreabilidade e下载/consulta corretos.

#### Impacto caso permaneça indefinido
Alta dificuldade de trabalho prático e integração com produção.

#### Possíveis interpretações
- S3 para objetos;
- metadata em PostgreSQL;
- download com autorização por objeto;
- chave pública e chave privada com uso seguro.

#### Recomendação
Registrar como requisito obrigatório e distinguir da implementação atual local.

### Pergunta 5 — Como o QR/GPS será tratado em cenários de indisponibilidade?

#### Contexto
A base documental exige regras de QR e GPS, mas o comportamento exato em caso de indisponibilidade ainda precisa ser validado.

#### Impacto caso permaneça indefinido
Operaçãο em campo pode ser bloqueada indevidamente.

#### Recomendação
Manter a regra documental e registrar a implementação atual como ponto de correção.

## MEDIUM

### Pergunta 6 — Qual será o modelo real de auditoria e histórico de resposta?

#### Contexto
A documentação exige histórico, mas o código pode não estar completamente alinhado.

#### Recomendação
Registrar como requisito e diferenciação entre histórico obrigatório e UI de histórico avançado.

### Pergunta 7 — Qual parte do cronograma acadêmico deve continuar como referência?

#### Contexto
O cronograma e o TAP são marcados como legado, mas ainda precisam ser atualizados para condizer com a documentação.

#### Recomendação
Usar como contexto histórico e manter um documento de atualização de legado em paralelo.

## LOW

### Pergunta 8 — Quais documentos legados precisam ser revisados para refletir o status atual?

#### Contexto
Muitos artefatos históricos coexistem com a documentação atual.

#### Recomendação
Manter a versão original como referência e ferramenta de comparação, sem tratá-la como regra ativa.
