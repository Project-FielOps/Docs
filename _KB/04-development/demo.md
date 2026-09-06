# Roteiro de demonstração do MVP

Executar sem editar banco nem usar Swagger como substituto do painel:

1. ADMIN autentica e cadastra/seleciona cliente, local, equipamento e usuários.
2. SUPERVISOR cria e publica modelo, agenda e atribui inspeção.
3. TECHNICIAN autentica, baixa a inspeção, perde a rede, inicia, responde, registra foto, NC e localização (ou ausência de GPS), confirma QR e conclui.
4. App reconecta, envia push e upload, repete uma operação para demonstrar idempotência e executa pull por cursor.
5. SUPERVISOR consulta resultado, inicia revisão, reprova com motivo; técnico reabre explicitamente, corrige e reenvia; supervisor aprova.
6. Consultar auditoria, evidenciar proteção após aprovação e mostrar estado de sync.

Pré-condições são dados de demonstração reproduzíveis, builds e ambiente AWS documentados. Até o mobile e AWS existirem, este roteiro é requisito pendente, não resultado alcançado.
