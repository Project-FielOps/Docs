# Especificação de autenticação e autorização

## Fluxo

Usuário ativo envia e-mail/senha a `POST /api/v1/auth/login`; API retorna `accessToken`, `refreshToken`, `tokenType`, `expiresInSeconds` e `mustChangePassword`. O cliente consulta `GET /api/v1/auth/me`, renova em `POST /api/v1/auth/refresh` e encerra em `POST /api/v1/auth/logout`. Credenciais inválidas e usuário inativo retornam erro indistinguível ao cliente.

## Regras e permissões

Aplicar RN-001..008 e DEC-002. A sessão não concede acesso por URL: cada endpoint deve verificar papel e vínculo de usuário com inspeção. Técnico só acessa inspeções atribuídas; supervisor só revisa as atribuídas como supervisor; ADMIN fica restrito a acesso e cadastros. A API atual ainda permite exceções e deve ser corrigida.

## Dados e erros

Senha é hash no servidor e nunca aparece em DTO/log. A recuperação usa `POST /api/v1/auth/forgot-password` e `POST /api/v1/auth/reset-password`, token opaco com hash SHA-256, uso único e expiração de 30 minutos, envio por Amazon SES e resposta genérica anti-enumeração. A redefinição é transacional e invalida todas as sessões. Senhas temporárias são marcadas com `mustChangePassword=true`, bloqueiam recursos normais até `change-password` e então são liberadas. Erros relevantes: `401 INVALID_CREDENTIALS`/`AUTHENTICATION_REQUIRED` e `403 ACCESS_DENIED`/`PASSWORD_CHANGE_REQUIRED`.

## Aceite

AC-AUTH e AC-SECURITY: usuário inativo não inicia sessão; recuperação não revela contas; token não é reutilizável; renovação repete a requisição no máximo uma vez; logout e reset revogam sessões; primeiro acesso com senha temporária exige troca.
