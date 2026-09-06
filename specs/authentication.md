# Especificação de autenticação e autorização

## Fluxo

Usuário ativo envia e-mail/senha a `POST /api/v1/auth/login`; API retorna `accessToken`, `refreshToken`, `tokenType` e `expiresInSeconds`. O cliente consulta `GET /api/v1/auth/me`, renova em `POST /api/v1/auth/refresh` e encerra localmente em `POST /api/v1/auth/logout`. Credenciais inválidas e usuário inativo retornam erro indistinguível ao cliente.

## Regras e permissões

Aplicar RN-001..008 e DEC-002. A sessão não concede acesso por URL: cada endpoint deve verificar papel e vínculo de usuário com inspeção. Técnico só acessa inspeções atribuídas; supervisor só revisa as atribuídas como supervisor; ADMIN fica restrito a acesso e cadastros. A API atual ainda permite exceções e deve ser corrigida.

## Dados e erros

Senha é hash no servidor e nunca aparece em DTO/log. DEC-005 exige senha temporária com troca obrigatória no primeiro acesso, recuperação por token único enviado por e-mail com expiração de 30 minutos, JWT curto, refresh rotativo, revogação no logout e invalidação de todos os refresh tokens na troca/redefinição. O código atual ainda usa TTL configurado e logout stateless, devendo ser alinhado. Erros relevantes: `401 INVALID_CREDENTIALS`/`AUTHENTICATION_REQUIRED` e `403 ACCESS_DENIED`.

## Aceite

AC-AUTH e AC-SECURITY: usuário inativo não inicia sessão; replay de URL não cruza escopo; renovação repete a requisição no máximo uma vez; logout remove tokens locais. Testar também status inativo após emissão, revogação e primeiro acesso quando DEC-005 for respondida.
