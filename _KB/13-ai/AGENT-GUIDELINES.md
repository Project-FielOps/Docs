# AGENT-GUIDELINES

## Antes de alterar código

1. Ler [Docs/_KB/README.md](../README.md).
2. Ler [Docs/_KB/13-ai/AI-CONTEXT.md](AI-CONTEXT.md).
3. Consultar a documentação do domínio afetado.
4. Consultar ADRs relacionadas.
5. Comparar a implementação atual com a base documental.
6. Diferenciar requisito, decisão, implementação e hipótese.
7. Não assumir que código divergente seja novo padrão.
8. Não inventar regras que não constem na documentação original.
9. Atualizar testes e documentação quando a mudança alterar regra, arquitetura ou contrato.
10. Registrar desvios como pendências de correção.

## Regras para agentes

- requisito: descrição formal do comportamento esperado;
- decisão arquitetural: escolha explícita que orienta o projeto;
- implementação: estado real do código;
- hipótese: ideia ainda não validada;
- sugestão: possibilidade sem decisão formal.

## Restrições

- não misturar AS-IS e TO-BE;
- não remover documentação original antes de validar;
- não transformar proposta em fato;
- não esconder divergências entre regra documental e implementação.

## Atualização necessária

Toda mudança de comportamento, arquitetura ou contrato exige atualização documentária e validação relacionada.
