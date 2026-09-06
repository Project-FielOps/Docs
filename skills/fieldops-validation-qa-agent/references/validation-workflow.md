# Impact analysis e testes independentes

## Expansão incremental

1. Identifique o repositório, a base do diff e alterações locais preexistentes.
2. Extraia arquivos e símbolos alterados.
3. Classifique-os como controller, service, entity, repository, DTO, mapper, migration, security, configuration ou test.
4. Localize imports, injeções, chamadas, mappings, repositories, testes e consumidores do contrato.
5. Expanda um nível por padrão. Expanda o fluxo inteiro para segurança, transação, migration, DTO público, componente compartilhado ou estado de inspeção.
6. Mapeie o fluxo a RN/FR/AC e abra apenas a documentação dos domínios alcançados.

Exemplo: mudança em `InspectionService` requer examinar controllers de inspeção/respostas, repositories e entidades usados, revisão/auditoria relacionada, consumidores do contrato e testes do workflow.

## Matriz mínima conforme aplicabilidade

- autenticação: anônimo, token inválido/expirado, usuário inativo;
- autorização: papel incorreto e papel correto com objeto alheio;
- contrato: payload válido/inválido, status, erro estruturado e compatibilidade;
- domínio: transição válida/inválida, imutabilidade e vínculos cruzados;
- dados: constraints, paginação, query, transação, rollback, concorrência;
- evidência: tipo/conteúdo, tamanho, vínculo, autorização, falha parcial e privacidade;
- sync: replay, ordem/dependência, falha parcial, conflito, cancelamento e aprovação tardia;
- auditoria: ator, ação, objeto e valores permitidos sem dados sensíveis.

### Gate de compilação limpa

Antes de interpretar qualquer resultado de teste, execute no repositório `API/`:

```text
mvn clean test
mvn clean package
```

Use Java 21 e Docker/Testcontainers quando exigidos pelo projeto. Uma execução incremental pode reutilizar bytecode antigo e não detectar erro de compilação em arquivo alterado; portanto, seu resultado não pode ser usado como gate final. Se o comando limpo não puder ser executado, registre a limitação e não trate o build como aprovado.

Depois, confira `git diff --check` e o estado dos artefatos apenas como verificações auxiliares. Não use a ausência de erro em `mvn package -DskipTests` incremental para afirmar que o fonte compila.

Execute testes direcionados antes da suíte ampla. Para cada método novo ou alterado em adapter, client, storage, gateway, mapper ou controller, identifique e execute um teste que percorra diretamente esse método. Testar somente outro método da mesma classe não valida o caminho novo. Registre exatamente o que não pôde ser executado e por quê.

## Cenários de avaliação da própria skill

Use estes casos em revisão de comportamento, não como testes de correspondência textual:

- ADMIN consegue iniciar ou aprovar uma inspeção;
- técnico lê ou altera inspeção atribuída a outro técnico;
- supervisor não atribuído inicia revisão;
- item de outra seção é alterado por URL cruzada;
- resposta aceita campo incompatível com `responseType`;
- entidade dependente de inspeção aprovada continua editável;
- replay de sync duplica resposta, evidência ou transição;
- uma migration aplicada é modificada;
- falha no armazenamento deixa arquivo ou metadado inconsistente.

O resultado esperado é rastrear o requisito, demonstrar o problema e atribuir severidade coerente; não exigir uma implementação predeterminada.
