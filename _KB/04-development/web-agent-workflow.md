# Workflow de desenvolvimento e QA da interface web

## Objetivo

Este workflow coordena o `fieldops-web-frontend-agent` e o `fieldops-web-qa-agent` na evolução de `interface-administrativa/`. Autor e validador têm responsabilidades separadas: o agente frontend entrega `READY_FOR_WEB_QA`; somente o agente QA emite veredito.

Execute desenvolvimento e QA em execuções separadas. O QA recebe a demanda original, as fontes, o diff e o handoff factual, mas não deve receber uma conclusão esperada, uma lista de problemas suspeitos ou instrução para confirmar a avaliação do autor.

O [Design System Industrial Web](../03-architecture/WEB_DESIGN_SYSTEM.md) é contrato obrigatório nas duas etapas.

As tasks, specs e endpoints da interface estão em [Pendências da interface web](../11-planning/web-pendencies/README.md). Toda implementação deve indicar os IDs `WEB-T*` e `WEB-*` afetados e conferir o catálogo de endpoints antes de alterar serviços HTTP.

## Fluxo

```text
Demanda e requisito
        ↓
Frontend: análise documental + design + contrato da API
        ↓
Plano técnico para mudança relevante
        ↓
Implementação incremental + testes + inspeção visual
        ↓
READY_FOR_WEB_QA
        ↓
QA independente: código + runtime + design + a11y + API real
        ↓
APPROVED / APPROVED_WITH_WARNINGS / CHANGES_REQUIRED
        ↓                                      │
Commit local                               correção
        ↓                                      │
Entrega sem push                              │
                                               │
                                               └── novo READY_FOR_WEB_QA
```

`BLOCKED_BY_DOCUMENTATION` interrompe apenas a decisão afetada até definição humana. O restante do escopo pode continuar quando independente.

## 1. Intake

Registrar objetivo, RN/FR/AC, usuário/papel, fluxo, estado de domínio, telas, contrato da API e critério de aceite. Mudança sem requisito formal deve ao menos possuir comportamento observável e fonte acordada.

## 2. Análise do Frontend Agent

O agente lê a política documental e o design system completos, inspeciona frontend e backend relevante e apresenta plano para mudanças que afetem fluxo, contrato, componente compartilhado, tema, formulário complexo, tabela ou múltiplas features.

O plano contém superfície afetada, matriz de estados, endpoints, responsividade, acessibilidade, riscos e testes. Lacunas são registradas antes da implementação.

## 3. Implementação

Desenvolver em incrementos verificáveis:

1. contrato tipado e serviço HTTP;
2. estado e comportamento da feature;
3. composição com componentes/tokens compartilhados;
4. estados alternativos e erros;
5. responsividade e acessibilidade;
6. testes e integração.

A ordem pode mudar quando a tarefa exigir, mas nenhuma camada pode ficar simulada no caminho de produção. Mudança necessária no backend é registrada e só é implementada quando estiver dentro do escopo autorizado.

## 4. Gate do desenvolvedor

Antes do handoff:

```bash
cd interface-administrativa
npm test -- --watch=false
npm run build
npx prettier --check src
```

O desenvolvedor registra resultados, inspeciona o runtime nos breakpoints aplicáveis e entrega `READY_FOR_WEB_QA` conforme seu skill. Ele não emite aprovação.

## 5. Validação independente

O QA refaz a matriz a partir das fontes, revisa além do diff, executa os gates e valida:

- comportamento permitido e proibido;
- conformidade com o design system;
- estados de página e de ação;
- teclado, foco, contraste, zoom e semântica;
- breakpoints compact, standard e wide;
- contrato HTTP e integração real com a API;
- regressão de componentes e fluxos relacionados.

Quando o ambiente não permitir runtime ou API real, o QA marca exatamente `NOT_EXECUTED`; não converte inspeção estática ou mock em evidência de integração.

Por padrão, o QA revisa, testa e reporta; não implementa a correção que ele próprio aprovaria. Se for explicitamente autorizado a acrescentar um teste adversarial ou uma evidência versionada, essa alteração deve retornar ao Frontend Agent e passar por revalidação independente antes da aprovação.

## 6. Correção e revalidação

Com `CHANGES_REQUIRED`, o Frontend Agent corrige somente os findings e regressões relacionadas, repete seus gates e emite novo `READY_FOR_WEB_QA`. O QA reexecuta o cenário do finding e a regressão proporcional ao risco; não aprova apenas por inspeção do patch.

## 7. Critério de saída

A mudança pode seguir para entrega quando:

- possui `APPROVED` ou `APPROVED_WITH_WARNINGS` justificado;
- gates aplicáveis passaram;
- requisitos críticos possuem evidência;
- integração não foi alegada sem execução;
- não há finding crítico, alto ou médio aberto;
- limitações e riscos residuais estão explícitos;
- documentação e contrato foram atualizados quando necessários.

## 8. Commit local e proibição de push

Depois de `APPROVED` ou `APPROVED_WITH_WARNINGS`, o Frontend Agent deve criar o commit local da entrega no repositório correspondente. O commit acontece somente após revisar `git status` e `git diff`, confirmar que os arquivos pertencem ao escopo validado e repetir qualquer gate afetado por correção posterior à validação.

- adicionar ao stage apenas arquivos da entrega; nunca incluir alterações locais preexistentes ou não relacionadas;
- usar o prefixo convencional apropriado (`feat:`, `fix:`, `test:`, `docs:` ou `chore:`) e uma mensagem que descreva o comportamento entregue;
- quando a entrega alterar mais de um dos repositórios independentes, criar commits separados em cada repositório, limitados ao respectivo escopo;
- registrar no handoff final o hash e a mensagem de cada commit criado;
- se o working tree não permitir separar com segurança a entrega de alterações alheias, não criar o commit e relatar o impedimento;
- **não executar `git push` em nenhuma circunstância**. Publicação remota depende de solicitação explícita do responsável e fica fora deste workflow.

O QA não cria o commit de aprovação e não executa push. Se o QA tiver sido autorizado a adicionar testes ou evidências versionadas, essas alterações retornam ao Frontend Agent para gates, revalidação proporcional e inclusão no commit local aprovado.
