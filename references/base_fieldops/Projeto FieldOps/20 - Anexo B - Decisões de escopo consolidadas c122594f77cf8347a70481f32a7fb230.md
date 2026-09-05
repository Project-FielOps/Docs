# 20 - Anexo B - Decisões de escopo consolidadas

Owner: Luciano Júnior

# Anexo B — Decisões de escopo consolidadas

1. O produto possui três aplicações: mobile, administrativa web e API REST.
2. A interface administrativa faz parte do MVP e não será substituída pelo Swagger.
3. O mobile é prioritariamente Android.
4. O aplicativo deverá operar offline após baixar uma inspeção.
5. O SQLite é persistência operacional, não cache descartável.
6. A sincronização utilizará operações idempotentes.
7. O modelo de inspeção será versionado.
8. Cada inspeção preservará um snapshot dos itens.
9. O MVP terá um técnico responsável por inspeção.
10. O portal do cliente ficará fora do MVP.
11. Fotografias serão a evidência principal.
12. QR Code e localização serão recursos obrigatórios do projeto mobile.
13. A coleta de localização será pontual.
14. A revisão será realizada pela interface administrativa.
15. As 16 semanas efetivas são o limite para planejamento do MVP.
16. As semanas institucionais restantes não serão consideradas reserva de desenvolvimento.
17. A avaliação combinará produto da equipe e domínio individual.