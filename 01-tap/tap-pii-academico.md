# Termo de Abertura do Projeto (TAP) — Projeto Integrador II

**Disciplina de origem:** Gestão de Projetos
**Projeto de referência:** FieldOps — Plataforma de Gestão de Inspeções Técnicas em Campo
**Versão:** 1.0
**Data:** 2026-08-21

> Este TAP formaliza a abertura do **Projeto Integrador II (PII) deste semestre**, cujo objeto é o desenvolvimento do **Projeto FieldOps**. Ele é redigido sob a ótica acadêmica do projeto (equipe, orientação, avaliação, prazos do semestre). O TAP complementar, focado exclusivamente no produto de software e em seus artefatos técnicos, está em `01-tap/tap-fieldops-produto.md`.

---

## 1. Identificação do projeto

| Campo | Descrição |
|---|---|
| Nome do projeto | FieldOps — Projeto Integrador II |
| Patrocinador (sponsor) | Coordenação/professor da disciplina, na qualidade de avaliador e representante do "cliente" simulado |
| Gerente do projeto | Aluno responsável pela condução técnica e de gestão do PII |
| Equipe | Aluno(s) responsáveis pelos componentes API, Interface Administrativa e (quando aplicável) App Mobile |
| Organização acadêmica | SENAI |
| Disciplina vinculada | Gestão de Projetos |

## 2. Justificativa e contexto

O Projeto Integrador II tem como finalidade consolidar, em um projeto prático e multidisciplinar, as competências de levantamento de requisitos, modelagem de dados, arquitetura de software, desenvolvimento full-stack e gestão de projetos trabalhadas ao longo do curso.

Para isso, a turma adota como estudo de caso o **Projeto FieldOps**: uma plataforma para planejamento, execução e revisão de inspeções técnicas em campo (manutenção, conformidade, auditoria de ativos), composta por três aplicações complementares — API REST, Interface Administrativa Web e Aplicativo Mobile — documentadas previamente em especificação funcional própria (ver `../02-eap/eap-fieldops.md` e o TAP de produto).

O PII usa esse contexto para que o aluno pratique, de ponta a ponta, o ciclo de um projeto real: abertura, planejamento (EAP, cronograma), execução (implementação da API e da interface administrativa) e encerramento.

## 3. Objetivo do projeto

Entregar, até o final do semestre, uma versão funcional do MVP do FieldOps compreendendo:

1. **API REST** (Java/Spring Boot) com autenticação, cadastros, modelos de inspeção versionados, execução e revisão de inspeções, evidências, não conformidades, auditoria e indicadores;
2. **Interface Administrativa Web** (Angular) que consome essa API e cobre o escopo mínimo definido na especificação (gestão de usuários, cadastros, construtor de modelos, planejamento, acompanhamento e revisão de inspeções);
3. Documentação de gestão do projeto (este TAP, EAP e cronograma) e documentação técnica (especificação funcional, contrato de API).

O Aplicativo Mobile e a sincronização offline permanecem fora do escopo deste build específico (ver decisões de escopo na especificação), podendo ser tratados em etapa futura do curso.

## 4. Premissas

- A equipe tem acesso a Java 21, Node/Angular CLI, Docker e PostgreSQL para desenvolvimento local.
- A especificação funcional do FieldOps (modelo de dados, regras de negócio, contrato de API) está disponível e é a fonte de verdade para as decisões de implementação.
- O professor da disciplina atua como cliente/avaliador para fins de validação de entregas.
- O repositório GitHub da organização `Project-FielOps` é o meio oficial de entrega e histórico do projeto.

## 5. Restrições

- Prazo limitado ao calendário do semestre letivo.
- Equipe reduzida (dimensionamento de curso, não de mercado), o que limita o paralelismo de frentes de trabalho.
- Escopo mobile e sincronização offline explicitamente fora do MVP acadêmico atual.
- Ambiente de desenvolvimento local (sem infraestrutura de produção provisionada).

## 6. Principais entregas (alto nível)

| Entrega | Descrição |
|---|---|
| API FieldOps | Serviço REST completo cobrindo autenticação, cadastros, templates, inspeções, evidências, revisão, não conformidades, auditoria e dashboard |
| Interface Administrativa | Aplicação Angular cobrindo as telas do escopo mínimo (DoD da interface, seção 14.17 da especificação) |
| Documentação de gestão | TAP (PII e produto), EAP e cronograma do projeto |
| Documentação técnica | Especificação funcional, contrato OpenAPI/Swagger, READMEs de cada repositório |

## 7. Principais partes interessadas (stakeholders)

| Stakeholder | Papel |
|---|---|
| Professor da disciplina | Patrocinador / avaliador do PII |
| Aluno(s) responsáveis | Equipe de execução (gestão + desenvolvimento) |
| Usuário simulado ADMIN/SUPERVISOR | Persona representada pela especificação funcional, usada para validar a interface administrativa |
| Turma / colegas | Revisores de pares em apresentações e entregas |

## 8. Riscos iniciais identificados

| Risco | Impacto | Mitigação |
|---|---|---|
| Escopo do MVP maior que o tempo disponível no semestre | Atraso na entrega final | Priorizar rigorosamente o escopo mínimo (seção 14.17 da especificação) e tratar o restante como P1/P2 |
| Divergência entre contrato de API assumido pelo frontend e o realmente implementado no backend | Retrabalho de integração | Validar o frontend contra o Swagger real do backend antes da entrega, conforme já registrado no README do frontend |
| Ambiente de desenvolvimento (Docker/Java/Node) não disponível em todas as máquinas da equipe | Bloqueio de desenvolvimento individual | Documentar pré-requisitos e passos de setup nos READMEs de cada repositório |
| Concentração de commits em poucas sessões de trabalho | Baixa granularidade de acompanhamento de progresso | Adotar o cronograma e o board do GitHub Projects para checkpoints intermediários, não apenas a entrega final |

## 9. Visão inicial de prazo

Estimativa de alto nível (detalhada no cronograma, `../03-cronograma/cronograma-fieldops.md`):

- Planejamento e modelagem: já concluídos (especificação funcional consolidada).
- Desenvolvimento da API: concluído (marco registrado no histórico do repositório `API`).
- Desenvolvimento da Interface Administrativa: concluído (marco registrado no histórico do repositório `interface-administrativa`).
- Testes, ajustes finos, documentação de gestão e encerramento: em andamento a partir da data deste TAP.

## 10. Aprovação

| Papel | Nome | Data |
|---|---|---|
| Gerente do projeto (aluno) | _____________________ | 2026-08-21 |
| Patrocinador (professor) | _____________________ | ___/___/______ |
