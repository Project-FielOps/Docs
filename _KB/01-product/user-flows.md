# Fluxos principais

## Fluxo de criação e publicação de template

1. ADMIN ou SUPERVISOR cria um modelo de inspeção.
2. O modelo recebe seções e itens.
3. O modelo é validado antes da publicação.
4. A publicação gera uma versão imutável.
5. A inspeção usa uma versão publicada e preserva snapshot.

## Fluxo de agendamento e atribuição

1. SUPERVISOR agenda inspeção.
2. Define cliente, local, equipamento e técnico.
3. O técnico recebe a inspeção atribuída.
4. A inspeção fica disponível para execução.

## Fluxo de execução

1. Técnico inicia inspeção atribuída.
2. Sistema carrega snapshot da versão selecionada.
3. Técnico responde itens e registra evidências.
4. Quando concluído, a inspeção é enviada para revisão.
5. A revisão exige supervisor atribuído.

## Fluxo de revisão

1. Supervisor inicia revisão.
2. Revisa respostas, observações e evidências.
3. Aprova ou reprova com motivo.
4. Em reprovada, o técnico corrige e reenvia.
5. A correção segue regra formal de reabertura da inspeção.

## Fluxo de auditoria

- ações críticas são registradas;
- histórico permanece disponível;
- alterações importantes são rastreáveis por usuário, data e motivo.
