# 19 - Critérios de Avaliação

Owner: Luciano Júnior

# 19. Critérios de Avaliação

## 19.1 Princípios da avaliação

A avaliação deverá considerar tanto o produto entregue quanto a aprendizagem individual. A existência de uma funcionalidade pronta não será suficiente quando o estudante não conseguir explicar as decisões, o fluxo de dados e sua contribuição.

Princípios:

- avaliação incremental, não concentrada apenas na última semana;
- equilíbrio entre funcionamento, qualidade e compreensão;
- evidências por commits, pull requests, testes e demonstrações;
- integração real entre as aplicações;
- distinção entre nota da equipe e desempenho individual;
- valorização da confiabilidade, não apenas da quantidade de telas.

## 19.2 Rubrica integrada do produto — 100 pontos

| Dimensão | Pontos | Evidências esperadas |
| --- | --- | --- |
| Visão, requisitos e aderência ao escopo | 5 | Backlog coerente, histórias e regras atendidas. |
| Arquitetura e organização | 10 | Separação de responsabilidades, estrutura e decisões documentadas. |
| Aplicativo mobile | 20 | Navegação, checklist, estados, persistência e experiência de campo. |
| Recursos nativos | 10 | Câmera, QR Code, localização e permissões. |
| Offline e sincronização | 15 | SQLite, outbox, idempotência, falhas e estado visível. |
| Interface administrativa | 10 | Cadastros, modelos, planejamento, acompanhamento e revisão. |
| API, regras e modelo de dados | 15 | Segurança, validações, endpoints, persistência e documentação. |
| Testes, segurança e qualidade | 7 | Testes críticos, lint, tratamento de erros e ausência de segredos. |
| Documentação e processo | 4 | README, OpenAPI, backlog, PRs e evidências. |
| Demonstração e defesa técnica | 4 | Fluxo ponta a ponta e explicação individual. |
| **Total** | **100** |  |

## 19.3 Rubrica para a disciplina de Expo — 100 pontos

Considerando que a disciplina possui 2 aulas semanais de conteúdo e 3 de projeto:

| Dimensão | Pontos | Critérios |
| --- | --- | --- |
| Avaliações individuais de conteúdo | 25 | TypeScript, navegação, dados, formulários, recursos nativos, SQLite e sincronização. |
| Fundação e arquitetura mobile | 10 | Estrutura, rotas, componentes, tipos e organização. |
| Integração, sessão e dados remotos | 10 | API, autenticação, erros, cache e listas. |
| Checklist dinâmico | 15 | Renderização, validação, progresso e persistência. |
| Recursos nativos | 15 | Câmera, QR Code, localização e permissões. |
| Offline e sincronização | 15 | SQLite, outbox, idempotência no cliente e recuperação de falhas. |
| Testes, acessibilidade e performance | 5 | Casos críticos, qualidade de uso e otimização. |
| Build, documentação e defesa | 5 | Build Android, README e explicação da contribuição. |
| **Total** | **100** |  |

## 19.4 Rubrica para Java/Spring Boot — 100 pontos

| Dimensão | Pontos |
| --- | --- |
| Modelagem do domínio e banco | 15 |
| API REST e DTOs | 15 |
| Autenticação e autorização | 15 |
| Regras de negócio e transições | 15 |
| Modelos versionados e snapshots | 10 |
| Sincronização e idempotência | 10 |
| Upload, evidências e auditoria | 5 |
| Testes automatizados | 8 |
| OpenAPI, execução e documentação | 5 |
| Defesa individual | 2 |
| **Total** | **100** |

## 19.5 Rubrica para a interface administrativa — 100 pontos

| Dimensão | Pontos |
| --- | --- |
| Arquitetura Angular e organização | 10 |
| Autenticação, guards e autorização visual | 10 |
| Cadastros e formulários | 15 |
| Construtor de modelos | 20 |
| Planejamento e acompanhamento | 15 |
| Revisão, aprovação e reprovação | 15 |
| Estados de interface, acessibilidade e responsividade | 5 |
| Testes e qualidade | 5 |
| Build, documentação e defesa | 5 |
| **Total** | **100** |

## 19.6 Avaliação por marcos

| Marco | Peso sugerido no projeto | Critério principal |
| --- | --- | --- |
| M1 — Fundação | 5% | Projetos executáveis e organizados. |
| M2 — Autenticação | 10% | Sessão integrada e autorização. |
| M3 — Planejamento | 10% | Modelo e inspeção criados pela web. |
| M4 — Execução online | 15% | Checklist dinâmico e respostas. |
| M5 — Recursos nativos | 15% | Foto, QR Code e localização. |
| M6 — Offline | 20% | Persistência e sincronização confiável. |
| M7 — Revisão | 10% | Aprovação, reprovação e correção. |
| M8 — Release | 15% | Integração final, build, documentação e defesa. |
| **Total** | **100%** |  |

## 19.7 Componente individual

A nota individual poderá considerar:

- avaliação prática ou teórica;
- explicação oral de uma parte do código;
- histórico de commits;
- pull requests abertos e revisados;
- capacidade de reproduzir e corrigir um defeito;
- compreensão da integração com a API;
- participação nas demonstrações;
- cumprimento das responsabilidades assumidas.

A quantidade de commits não deverá ser utilizada isoladamente. Serão considerados relevância, qualidade, autoria real e capacidade de explicar a contribuição.

## 19.8 Evidências obrigatórias por equipe

- URL dos repositórios;
- backlog atualizado;
- pull requests principais;
- README de cada aplicação;
- instruções de execução;
- OpenAPI;
- esquema ou diagrama de dados;
- build Android;
- build ou URL da interface administrativa;
- ambiente ou contêiner da API;
- usuários de demonstração;
- roteiro da demonstração;
- registro dos testes principais;
- lista de limitações conhecidas.

## 19.9 Penalidades técnicas sugeridas

Poderão reduzir a avaliação:

- credenciais ou segredos no repositório;
- perda de dados não sincronizados;
- ausência de autorização no backend;
- duplicidade causada por reenvio previsível;
- uso do Swagger ou banco como substituto da interface administrativa no fluxo final;
- telas estáticas apresentadas como integração concluída;
- alteração manual de banco durante a demonstração;
- funcionalidades copiadas sem compreensão;
- ausência de histórico ou evidências de participação;
- impossibilidade de executar o projeto a partir da documentação.

## 19.10 Bônus ou diferenciais

Após a conclusão estável do MVP, poderão ser valorizados:

- notificações push;
- relatório PDF;
- assinatura;
- biometria;
- dashboard avançado;
- acessibilidade ampliada;
- testes ponta a ponta;
- monitoramento;
- deploy automatizado;
- resolução assistida de conflitos;
- portal do cliente;
- solução criativa validada com usuários.

> Funcionalidades adicionais não compensam falhas graves no fluxo principal, na segurança ou na preservação dos dados offline.
> 

---