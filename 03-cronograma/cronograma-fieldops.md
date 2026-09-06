# Cronograma do Projeto FieldOps

**Versão:** 1.0
**Data:** 2026-08-21
**Estado:** cronograma legado preservado para rastreabilidade. A data vigente de entrega é o fim de novembro de 2026; consulte [plano atualizado](../_KB/11-planning/mvp.md).
**Referência de EAP:** `../02-eap/eap-fieldops.md`

## Como ler este cronograma

- **Status "Concluído"**: verificado diretamente no histórico de commits dos repositórios `API` e `interface-administrativa` (ambos convergem para 2026-08-17 como data de conclusão do build inicial) e nos artefatos de documentação já existentes.
- **Status "Planejado"**: atividades ainda não realizadas, projetadas a partir de hoje (2026-08-21) até o encerramento do PII.
- **Predecessora**: refere-se ao ID da atividade antecessora nesta mesma tabela.
- **Responsável**: papel dentro da equipe do PII (preencher com o(s) nome(s) reais da equipe/aluno responsável).
- Datas de início/planejamento retroativas (antes de 2026-08-21) refletem o planejamento reconstituído do trabalho já realizado; não são um registro de apontamento de horas.

Total de itens: **65** (mínimo exigido: 50).

---

## Fase 1 — Iniciação (2026-07-06 a 2026-07-10)

| ID | EAP | Atividade | Predecessora | Duração | Início | Fim | Responsável | Marco | Status |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 1.1.1 | Reunião de abertura do PII e definição do estudo de caso (FieldOps) | — | 1 dia | 2026-07-06 | 2026-07-06 | Gerente do Projeto | Não | Concluído |
| 2 | 1.1.1 | Identificação de patrocinador e stakeholders | 1 | 1 dia | 2026-07-07 | 2026-07-07 | Gerente do Projeto | Não | Concluído |
| 3 | 1.1.1 | Elaboração do TAP — PII acadêmico | 2 | 1 dia | 2026-07-08 | 2026-07-08 | Gerente do Projeto | Não | Concluído |
| 4 | 1.1.1 | Elaboração do TAP — Produto FieldOps | 3 | 1 dia | 2026-07-09 | 2026-07-09 | Gerente do Projeto | Não | Concluído |
| 5 | 1.1.1 | Aprovação do TAP pelo patrocinador | 4 | 1 dia | 2026-07-10 | 2026-07-10 | Patrocinador | **Sim** | Concluído |

## Fase 2 — Planejamento e Modelagem (2026-07-13 a 2026-07-24)

| ID | EAP | Atividade | Predecessora | Duração | Início | Fim | Responsável | Marco | Status |
|---|---|---|---|---|---|---|---|---|---|
| 6 | 1.2.1 | Levantamento de requisitos funcionais (Notion) | 5 | 3 dias | 2026-07-13 | 2026-07-15 | Gerente do Projeto | Não | Concluído |
| 7 | 1.2.2 | Modelagem de dados (entidades, relacionamentos, diagrama conceitual) | 6 | 3 dias | 2026-07-16 | 2026-07-20 | Equipe | Não | Concluído |
| 8 | 1.2.3 | Definição das regras de negócio (RN-001 a RN-090) | 7 | 2 dias | 2026-07-21 | 2026-07-22 | Equipe | Não | Concluído |
| 9 | 1.2.4 | Definição da arquitetura (camadas, contextos funcionais) | 7 | 2 dias | 2026-07-21 | 2026-07-22 | Equipe | Não | Concluído |
| 10 | 1.8.1 | Consolidação da especificação funcional (`fieldops-spec.md`) | 8, 9 | 1 dia | 2026-07-23 | 2026-07-23 | Gerente do Projeto | Não | Concluído |
| 11 | 1.1.2 | Elaboração da EAP do projeto | 10 | 1 dia | 2026-07-24 | 2026-07-24 | Gerente do Projeto | Não | Concluído |
| 12 | 1.1.2 | Elaboração do cronograma do projeto | 11 | 1 dia | 2026-07-24 | 2026-07-24 | Gerente do Projeto | **Sim** | Concluído |

## Fase 3 — Desenvolvimento da API / Backend (2026-07-27 a 2026-08-17)

| ID | EAP | Atividade | Predecessora | Duração | Início | Fim | Responsável | Marco | Status |
|---|---|---|---|---|---|---|---|---|---|
| 13 | 1.3.1 | Scaffold do projeto Spring Boot (build, config) | 12 | 2 dias | 2026-07-27 | 2026-07-28 | Dev Backend | Não | Concluído |
| 14 | 1.6.1 | Configuração do ambiente (Docker Compose + PostgreSQL) | 13 | 1 dia | 2026-07-29 | 2026-07-29 | Dev Backend | Não | Concluído |
| 15 | 1.3.1 | Migração inicial do schema (Flyway V1, UUID, controle otimista) | 14 | 2 dias | 2026-07-30 | 2026-07-31 | Dev Backend | Não | Concluído |
| 16 | 1.3.2 | Autenticação JWT e configuração do Spring Security | 15 | 2 dias | 2026-08-03 | 2026-08-04 | Dev Backend | Não | Concluído |
| 17 | 1.3.2 | Autorização por papéis (ADMIN/SUPERVISOR/TECHNICIAN/CLIENT_VIEWER) | 16 | 1 dia | 2026-08-05 | 2026-08-05 | Dev Backend | Não | Concluído |
| 18 | 1.3.3 | CRUD de usuários | 17 | 1 dia | 2026-08-06 | 2026-08-06 | Dev Backend | Não | Concluído |
| 19 | 1.3.3 | CRUD de clientes | 17 | 1 dia | 2026-08-06 | 2026-08-06 | Dev Backend | Não | Concluído |
| 20 | 1.3.3 | CRUD de locais (InspectionSite) | 19 | 1 dia | 2026-08-07 | 2026-08-07 | Dev Backend | Não | Concluído |
| 21 | 1.3.3 | CRUD de equipamentos | 20 | 1 dia | 2026-08-07 | 2026-08-07 | Dev Backend | Não | Concluído |
| 22 | 1.3.4 | Modelo de inspeção: template, seções e itens | 21 | 2 dias | 2026-08-10 | 2026-08-11 | Dev Backend | Não | Concluído |
| 23 | 1.3.4 | Versionamento imutável e publicação de templates | 22 | 1 dia | 2026-08-11 | 2026-08-11 | Dev Backend | Não | Concluído |
| 24 | 1.3.5 | Ciclo de vida da inspeção (agendamento → execução → submissão) | 23 | 2 dias | 2026-08-12 | 2026-08-13 | Dev Backend | Não | Concluído |
| 25 | 1.3.5 | Snapshot de itens da inspeção (`InspectionItemSnapshot`) | 24 | 1 dia | 2026-08-13 | 2026-08-13 | Dev Backend | Não | Concluído |
| 26 | 1.3.5 | Respostas de inspeção (individuais e em lote) | 25 | 1 dia | 2026-08-14 | 2026-08-14 | Dev Backend | Não | Concluído |
| 27 | 1.3.6 | Metadados de evidências (upload real fora de escopo) | 24 | 1 dia | 2026-08-14 | 2026-08-14 | Dev Backend | Não | Concluído |
| 28 | 1.3.7 | Não conformidades | 24 | 1 dia | 2026-08-14 | 2026-08-14 | Dev Backend | Não | Concluído |
| 29 | 1.3.7 | Revisão em ciclos (begin-review / approve / reject) | 26, 28 | 1 dia | 2026-08-14 | 2026-08-14 | Dev Backend | Não | Concluído |
| 30 | 1.3.8 | Auditoria de eventos do sistema | 29 | 1 dia | 2026-08-14 | 2026-08-14 | Dev Backend | Não | Concluído |
| 31 | 1.3.8 | Dashboard e indicadores | 30 | 1 dia | 2026-08-14 | 2026-08-14 | Dev Backend | Não | Concluído |
| 32 | 1.3.1 | Padronização de erros e seed de dados de desenvolvimento | 31 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Backend | Não | Concluído |
| 33 | 1.3.9 | Testes de integração (auth, versionamento, fluxo completo) | 32 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Backend | Não | Concluído |
| 34 | 1.3.10 | Documentação Swagger/OpenAPI e README da API | 33 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Backend | **Sim** | Concluído |

## Fase 4 — Desenvolvimento da Interface Administrativa / Frontend (2026-08-10 a 2026-08-17)

| ID | EAP | Atividade | Predecessora | Duração | Início | Fim | Responsável | Marco | Status |
|---|---|---|---|---|---|---|---|---|---|
| 35 | 1.4.1 | Scaffold Angular standalone e layout base (shell, sidenav) | 22 | 2 dias | 2026-08-10 | 2026-08-11 | Dev Frontend | Não | Concluído |
| 36 | 1.4.2 | Tela de login, interceptors HTTP e storage de token | 35 | 1 dia | 2026-08-12 | 2026-08-12 | Dev Frontend | Não | Concluído |
| 37 | 1.4.2 | Guards de rota e de perfil | 36 | 1 dia | 2026-08-12 | 2026-08-12 | Dev Frontend | Não | Concluído |
| 38 | 1.4.3 | Módulo de usuários (ADMIN) | 37 | 1 dia | 2026-08-13 | 2026-08-13 | Dev Frontend | Não | Concluído |
| 39 | 1.4.4 | Módulo de clientes + drill-down de locais | 37 | 1 dia | 2026-08-13 | 2026-08-13 | Dev Frontend | Não | Concluído |
| 40 | 1.4.4 | Módulo de locais + drill-down de equipamentos | 39 | 1 dia | 2026-08-14 | 2026-08-14 | Dev Frontend | Não | Concluído |
| 41 | 1.4.4 | Módulo de equipamentos (com busca por QR) | 40 | 1 dia | 2026-08-14 | 2026-08-14 | Dev Frontend | Não | Concluído |
| 42 | 1.4.5 | Construtor de modelos: seções e itens | 41 | 2 dias | 2026-08-14 | 2026-08-17 | Dev Frontend | Não | Concluído |
| 43 | 1.4.5 | Preview de checklist e histórico de versões | 42 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Frontend | Não | Concluído |
| 44 | 1.4.6 | Planejamento e listagem de inspeções (filtros, cascata cliente→local→equip.) | 42 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Frontend | Não | Concluído |
| 45 | 1.4.7 | Detalhe e tela de revisão de inspeções (aprovar/reprovar) | 44 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Frontend | Não | Concluído |
| 46 | 1.4.8 | Telas de não conformidades e auditoria | 45 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Frontend | Não | Concluído |
| 47 | 1.4.9 | Dashboard administrativo | 46 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Frontend | Não | Concluído |
| 48 | 1.4.10 | Validação e ajuste de integração contra a API real | 34, 47 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Frontend | Não | Concluído |
| 49 | 1.4.10 | Atualização do README com nota de integração verificada | 48 | 1 dia | 2026-08-17 | 2026-08-17 | Dev Frontend | **Sim** — MVP web + API integrado | Concluído |

## Fase 5 — Qualidade e Ajustes (2026-08-24 a 2026-09-04) — planejado

| ID | EAP | Atividade | Predecessora | Duração | Início | Fim | Responsável | Marco | Status |
|---|---|---|---|---|---|---|---|---|---|
| 50 | 1.7.4 | Testes manuais ponta a ponta (login → inspeção → revisão) | 49 | 2 dias | 2026-08-24 | 2026-08-25 | Equipe | Não | Planejado |
| 51 | 1.7.2 | Escrita de testes automatizados do frontend | 49 | 3 dias | 2026-08-26 | 2026-08-28 | Dev Frontend | Não | Planejado |
| 52 | 1.7.3 | Revisão de UI/UX da interface administrativa | 50 | 2 dias | 2026-08-31 | 2026-09-01 | Equipe | Não | Planejado |
| 53 | 1.7.4 | Correção dos bugs e pontos identificados na revisão | 51, 52 | 3 dias | 2026-09-02 | 2026-09-04 | Equipe | **Sim** — Homologação interna aprovada | Planejado |

## Fase 6 — Documentação e Governança (2026-09-07 a 2026-09-11) — planejado

| ID | EAP | Atividade | Predecessora | Duração | Início | Fim | Responsável | Marco | Status |
|---|---|---|---|---|---|---|---|---|---|
| 54 | 1.8.3 | Consolidação da documentação de gestão (este repositório `Docs`) | 12 | 1 dia | 2026-08-21 | 2026-08-21 | Gerente do Projeto | Não | Concluído |
| 55 | 1.8.4 | Levantamento e conciliação do board GitHub Projects #4 | 53 | 2 dias | 2026-09-07 | 2026-09-08 | Gerente do Projeto | Não | Planejado |
| 56 | 1.8.2 | Revisão final dos READMEs técnicos (API e frontend) | 53 | 1 dia | 2026-09-09 | 2026-09-09 | Equipe | Não | Planejado |
| 57 | 1.1.3 | Checkpoint de monitoramento e controle com o patrocinador | 55, 56 | 1 dia | 2026-09-11 | 2026-09-11 | Gerente do Projeto | **Sim** | Planejado |

## Fase 7 — Aplicativo Mobile (2026-09-14 a 2026-10-02) — planejado

| ID | EAP | Atividade | Predecessora | Duração | Início | Fim | Responsável | Marco | Status |
|---|---|---|---|---|---|---|---|---|---|
| 58 | 1.5.1 | Setup do projeto Expo/React Native | 57 | 2 dias | 2026-09-14 | 2026-09-15 | Dev Mobile | Não | Planejado |
| 59 | 1.5.2 | Persistência local (SQLite) e modelo offline | 58 | 4 dias | 2026-09-16 | 2026-09-21 | Dev Mobile | Não | Planejado |
| 60 | 1.5.3 | Telas de execução de inspeção offline | 59 | 4 dias | 2026-09-22 | 2026-09-25 | Dev Mobile | Não | Planejado |
| 61 | 1.5.4 | Captura de evidências e leitura de QR Code | 60 | 3 dias | 2026-09-28 | 2026-09-30 | Dev Mobile | Não | Planejado |
| 62 | 1.5.5 | Implementação da sincronização (push/pull idempotente) | 61 | 2 dias | 2026-10-01 | 2026-10-02 | Dev Mobile | **Sim** — MVP mobile concluído | Planejado |

## Fase 8 — Infraestrutura Complementar (2026-10-05 a 2026-10-07) — planejado

| ID | EAP | Atividade | Predecessora | Duração | Início | Fim | Responsável | Marco | Status |
|---|---|---|---|---|---|---|---|---|---|
| 63 | 1.6.3 | Definição de pipeline de CI/CD | 62 | 2 dias | 2026-10-05 | 2026-10-06 | Equipe | Não | Planejado |
| 64 | 1.6.4 | Configuração de armazenamento de evidências (objeto/S3-compatível) | 62 | 1 dia | 2026-10-07 | 2026-10-07 | Dev Backend | Não | Planejado |

## Fase 9 — Encerramento (2026-10-08 a 2026-10-09) — planejado

| ID | EAP | Atividade | Predecessora | Duração | Início | Fim | Responsável | Marco | Status |
|---|---|---|---|---|---|---|---|---|---|
| 65 | 1.9.1 / 1.9.2 / 1.9.3 | Homologação final com stakeholders, lições aprendidas e entrega/encerramento formal do PII | 63, 64 | 2 dias | 2026-10-08 | 2026-10-09 | Gerente do Projeto | **Sim** — Encerramento do PII | Planejado |

---

## Marcos do projeto (resumo)

| Marco | Data | Fase |
|---|---|---|
| TAP aprovado | 2026-07-10 | Iniciação |
| Planejamento concluído (EAP + Cronograma) | 2026-07-24 | Planejamento |
| Backend (API) concluído | 2026-08-17 | Desenvolvimento API |
| MVP Web (API + Interface Administrativa) integrado | 2026-08-17 | Desenvolvimento Frontend |
| Homologação interna aprovada | 2026-09-04 | Qualidade |
| Checkpoint de governança com o patrocinador | 2026-09-11 | Documentação |
| MVP mobile concluído | 2026-10-02 | Mobile |
| Encerramento do PII | 2026-10-09 | Encerramento |
