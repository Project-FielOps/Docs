# Termo de Abertura do Projeto (TAP) — Projeto FieldOps

**Versão:** 1.0
**Data:** 2026-08-21
**Fonte de artefatos considerados:** especificação funcional (`fieldops-spec.md`, extraída do Notion), repositórios `API` e `interface-administrativa` da organização GitHub `Project-FielOps`, board do projeto (GitHub Projects #4).

> Este é o segundo TAP solicitado na atividade da Aula 2, elaborado "considerando o Projeto FieldOps" e toda a documentação/artefatos disponíveis. Ele descreve o FieldOps sob a ótica de **produto de software**, independente do enquadramento acadêmico (que está tratado em `01-tap/tap-pii-academico.md`).

---

## 1. Identificação do projeto

| Campo | Descrição |
|---|---|
| Nome do projeto | FieldOps |
| Tipo | Produto de software — plataforma de gestão de inspeções técnicas em campo |
| Organização | Project-FielOps |
| Repositórios | `API` (Java/Spring Boot), `interface-administrativa` (Angular), `aplicativo-mobile` (Expo/React Native — não iniciado), `gestao-fieldops` / `Docs` (documentação) |

## 2. Justificativa

Empresas que realizam inspeções técnicas periódicas (manutenção, conformidade, segurança) em ativos e locais de clientes hoje dependem, tipicamente, de processos manuais ou planilhas para planejar inspeções, atribuir técnicos, coletar respostas em campo e revisar resultados. Isso gera:

- perda de rastreabilidade de quem respondeu o quê, quando e com qual evidência;
- dificuldade de auditar decisões (aprovações/reprovações) e de preservar o histórico quando os checklists mudam ao longo do tempo;
- ausência de visão consolidada (indicadores) sobre inspeções atrasadas, pendentes de revisão ou não conformidades críticas.

O FieldOps propõe resolver isso com um modelo de dados que trata **modelo de inspeção** e **execução da inspeção** como conceitos distintos e versionados (checklist publicado de forma imutável; cada inspeção preserva um snapshot congelado dos itens usados), preservação histórica (nunca excluir fisicamente registros usados em inspeções) e um fluxo formal de revisão/aprovação por supervisores.

## 3. Objetivo do projeto

Entregar uma plataforma composta por três aplicações que cobrem o ciclo completo de uma inspeção — **cadastro do cliente → cadastro de locais/equipamentos → criação e publicação de modelo de inspeção → agendamento → execução em campo → sincronização → revisão e aprovação → auditoria**:

| Componente | Stack | Responsabilidade |
|---|---|---|
| API REST | Java + Spring Boot + PostgreSQL | Regras de negócio, autenticação, persistência, auditoria |
| Interface Administrativa | Angular + TypeScript | Cadastros, planejamento, acompanhamento, revisão |
| Aplicativo Mobile | Expo + React Native + TypeScript | Execução de inspeções em campo com suporte offline (SQLite) |

## 4. Escopo

### 4.1 Dentro do escopo (MVP)
- Autenticação e perfis: `ADMIN`, `SUPERVISOR`, `TECHNICIAN`, `CLIENT_VIEWER`.
- Cadastros: clientes, locais (`InspectionSite`), equipamentos.
- Modelos de inspeção versionados (`InspectionTemplate` → `InspectionTemplateVersion` → `TemplateSection` → `TemplateItem`), com publicação imutável.
- Planejamento e ciclo de vida da inspeção (`DRAFT` → `ASSIGNED` → `IN_PROGRESS` → `SUBMITTED` → `UNDER_REVIEW` → `APPROVED`/`REJECTED`/`CANCELED`), com snapshot dos itens (`InspectionItemSnapshot`) e respostas (`InspectionResponse`).
- Evidências fotográficas vinculadas a respostas/não conformidades.
- Não conformidades com criticidade (`LOW`/`MEDIUM`/`HIGH`/`CRITICAL`).
- Revisão em ciclos (`InspectionReview`), com aprovação/reprovação motivada.
- Auditoria (`AuditEvent`) de ações relevantes do sistema.
- Indicadores/dashboard administrativo.
- Interface Administrativa Web cobrindo o escopo mínimo (seção 14.17 da especificação).

### 4.2 Fora do escopo deste ciclo de build
- Aplicativo Mobile (execução em campo) e persistência local (SQLite) — repositório `aplicativo-mobile` criado, porém sem código.
- Sincronização offline (`SyncOperation`/`SyncMetadata`) e endpoints `/api/v1/mobile/**`.
- QR Code e captura de localização em campo.
- Portal do cliente (`CLIENT_VIEWER` com self-service).
- Plano de ação completo de não conformidade (tratado como P2).
- Upload de evidências pela interface administrativa (evidências são capturadas no mobile; a web hoje apenas exibe).

## 5. Premissas

- PostgreSQL como persistência central; SQLite reservado à operação offline do app mobile (fora deste build).
- IDs globais em UUID em todas as entidades, para permitir sincronização futura sem remapeamento.
- Nunca excluir fisicamente entidades usadas em registros históricos — inativação/versionamento no lugar de exclusão.
- Controle de concorrência otimista (campo `version`) em entidades sujeitas a updates concorrentes.
- Armazenamento de evidências fora do banco relacional (banco guarda apenas metadados).

## 6. Restrições

- Um único técnico responsável por inspeção no MVP.
- Todo equipamento pertence a um local (não há equipamento "solto").
- Tipos de resposta do MVP limitados a: `TEXT_SHORT`, `TEXT_LONG`, `NUMBER`, `BOOLEAN`, `CONFORMITY`, `SINGLE_CHOICE`, `DATE`.
- Reordenação de seções/itens no construtor de modelos por botões mover-para-cima/baixo — drag-and-drop é desejável, não obrigatório.
- Segredos/configurações sensíveis fora do repositório (variáveis de ambiente).

## 7. Principais entregas

1. API REST documentada via OpenAPI/Swagger, com testes de integração cobrindo autenticação, versionamento de templates e o fluxo completo de inspeção (agendar → executar → submeter → revisar → aprovar/reprovar).
2. Interface Administrativa Web com as telas de: login, dashboard, usuários, clientes, locais, equipamentos, construtor de modelos de inspeção, planejamento/acompanhamento/detalhe/revisão de inspeções, não conformidades e auditoria.
3. Banco de dados versionado via Flyway, com seed de dados de desenvolvimento (usuários de cada perfil, clientes, locais e equipamentos de exemplo).
4. Documentação de gestão do projeto (este TAP, EAP, cronograma) e documentação técnica (especificação funcional).

## 8. Stakeholders

| Stakeholder | Interesse |
|---|---|
| Administrador (perfil `ADMIN`) | Gerenciar usuários e cadastros mestres |
| Supervisor (perfil `SUPERVISOR`) | Planejar inspeções, revisar e aprovar/reprovar resultados |
| Técnico (perfil `TECHNICIAN`) | Executar inspeções atribuídas (via app mobile, fora deste build) |
| Cliente final (perfil `CLIENT_VIEWER`) | Consulta futura de resultados (fora do MVP) |
| Equipe de desenvolvimento | Entregar e manter os três componentes da plataforma |

## 9. Riscos iniciais

| Risco | Impacto | Mitigação |
|---|---|---|
| App mobile inexistente inviabiliza a demonstração ponta a ponta do ciclo de inspeção (execução em campo) | Alto — parte central da proposta de valor não é demonstrável | Tratar como próxima fase explícita do roadmap; usar dados de seed/API diretamente para simular execução nas demos |
| Divergência entre o contrato de API assumido pelo frontend e o real | Retrabalho de integração | Validar contra Swagger antes de cada entrega (já aplicado uma vez, conforme commit `fix: align frontend with the real backend API contract`) |
| Ausência de repositório de documentação centralizado até esta atividade | Perda de rastreabilidade de decisões de gestão | Criação do repositório `Docs` nesta entrega, como fonte única de TAP/EAP/cronograma |
| Board do GitHub Projects não auditado neste TAP (acesso pendente de escopo de leitura) | Falta de visibilidade sobre itens pendentes fora do código | Revisar o board manualmente e conciliar com o cronograma na próxima atualização |

## 10. Aprovação

| Papel | Nome | Data |
|---|---|---|
| Responsável pelo projeto | _____________________ | 2026-08-21 |
| Patrocinador / cliente simulado | _____________________ | ___/___/______ |
