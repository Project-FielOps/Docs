# Estratégia de testes

API: JUnit/Spring Security/Testcontainers para autenticação, autorização por objeto, invariantes de modelo, transições, snapshot, respostas, evidências, revisão e replay de sync. O código atual cobre autenticação, publicação/snapshot e fluxo online; os cenários de acesso de ADMIN, GET por vínculo, tipos, upload e sync ainda são lacunas.

Web: testes de guards, formulários, filtros encadeados, estados loading/vazio/erro, revisão e erros 401/403/409/422. O workspace contém apenas smoke test de criação da aplicação; ampliar antes do aceite.

Mobile: em Android/emulador, testar permissões, cada tipo, persistência após reinício, QR divergente, GPS ausente, outbox, falha parcial, replay, conflito e correção pós-reprovação. O app não existe neste workspace.

Cada teste deve apontar para RN/FR/AC e provar comportamento observável. Testes não autorizam afirmar deploy ou aceite sem execução registrada.
