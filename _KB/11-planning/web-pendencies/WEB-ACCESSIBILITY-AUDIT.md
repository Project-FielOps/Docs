# Checklist de auditoria de acessibilidade web

**Escopo:** painel administrativo web, telas principais do MVP.  
**Referência normativa:** WCAG 2.1 nível AA, DEC-011 e NFR-013.  
**Estado:** roteiro de execução; nenhuma tela foi auditada por este documento.

## Registro da execução

Preencher antes de iniciar:

| Campo | Valor |
|---|---|
| Data e responsável | |
| Commit/revisão do frontend | |
| Navegador e versão | |
| Sistema operacional | |
| Tela, papel e estado testados | |
| Ferramentas automáticas e versões | |

Por critério, registrar `PASS`, `FAIL`, `BLOCKED` ou `NOT RUN`, passos reproduzíveis e referência para evidência. Uma varredura automatizada isolada não comprova conformidade WCAG.

## Checklist manual e automatizado

| ID | Verificação | Procedimento e critério de aprovação | Resultado/evidência |
|---|---|---|---|
| A11Y-01 | Contraste de texto | Medir texto comum em pelo menos 4.5:1 e texto grande em pelo menos 3:1; registrar pares de cores e ferramenta. | |
| A11Y-02 | Contraste não textual | Medir controles, indicadores de foco e informação gráfica essencial em pelo menos 3:1 contra cores adjacentes. | |
| A11Y-03 | Teclado | Completar login, busca/filtro, formulário, diálogo e ação crítica sem mouse; ordem de foco segue a ordem visual e não há armadilha. | |
| A11Y-04 | Foco visível | Tabular por controles interativos; cada posição tem indicador de foco perceptível, sem ser cortado ou encoberto. | |
| A11Y-05 | Nome e papel acessíveis | Inspecionar árvore de acessibilidade; controles têm nome, papel e estado programáticos coerentes com o que é exibido. | |
| A11Y-06 | Formulários e erros | Confirmar label associado, instrução e erro relacionados ao campo; erro não é comunicado só por cor e permanece disponível após submissão inválida. | |
| A11Y-07 | Diálogos | Abrir, navegar, fechar e reabrir; foco entra no diálogo, permanece no seu contexto, fecha por teclado quando aplicável e retorna à origem. | |
| A11Y-08 | Status assíncrono | Acionar carregamento, sucesso e erro; atualização importante é anunciada sem deslocar o foco indevidamente. | |
| A11Y-09 | Cor e ícones | Verificar estados/status sem cor como único sinal; ícone funcional tem nome acessível e decorativo é ignorado. | |
| A11Y-10 | Zoom e reflow | A 200% de zoom e viewport compacta, conteúdo e ações do fluxo principal continuam disponíveis sem sobreposição ou rolagem bidimensional indevida. | |
| A11Y-11 | Movimento reduzido | Ativar `prefers-reduced-motion`; movimento não essencial é removido/reduzido e nenhuma animação bloqueia ação ou leitura. | |
| A11Y-12 | Tabelas | Confirmar cabeçalhos associados, ordenação identificável e operação do controle de ordenação por teclado. | |
| A11Y-13 | Ferramenta automática | Executar scanner configurado nas telas/estados cobertos; registrar violações, falsos positivos revisados e versão da ferramenta. | |

## Cobertura mínima do MVP

Executar a auditoria ao menos nestes fluxos e estados, usando perfil permitido:

- login: inicial, erro de credencial e processamento;
- shell/dashboard: carregando, dados, vazio e erro;
- formulário de planejamento: preenchido, inválido e submissão em andamento;
- detalhe/revisão: evidência, reprovação com motivo e confirmação de ação;
- listagem: carregando, sem resultados, erro e menu/filtro aberto.

Um resultado em uma tela não representa cobertura das demais. Registrar separadamente cada combinação de tela, estado e viewport relevante.

## Abertura e encerramento de achados

Para cada falha, registrar critério WCAG relacionado, tela/estado, passos, resultado esperado/observado, severidade acordada pela equipe, evidência e issue de correção. Reexecutar exatamente os mesmos passos após a correção. A auditoria só pode ser marcada como aprovada quando todos os critérios aplicáveis tiverem resultado e nenhuma falha bloqueadora permanecer.

## Referências

- [Design system web — acessibilidade e critérios de pronto](../../03-architecture/WEB_DESIGN_SYSTEM.md)
- [Requisitos não funcionais — NFR-013](../../01-product/requirements-non-functional.md)
- [Especificação funcional — verificação de usabilidade](../../01-product/application-scope-and-screen-specification.md)
- [Backlog web — WEB-T036](BACKLOG.md)
