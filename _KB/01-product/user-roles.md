# Papéis e permissões

## Papéis canônicos

- ADMIN
- SUPERVISOR
- TECHNICIAN
- CLIENT_VIEWER

## Regras de autorização

- somente usuários ativos podem iniciar sessão;
- a API valida perfil em todas as operações protegidas;
- o técnico atribuído executa somente inspeções atribuídas a ele;
- o supervisor atribuído revisa somente as inspeções do seu escopo;
- o ADMIN cuida de cadastros e permissões, sem assumir execução operacional por padrão;
- a autorização por perfil e por objeto é regra oficial do MVP;
- o código atual que foge a esse padrão deve ser tratado como desvio a corrigir.

## Observação

A documentação original define a regra, e a implementação atual deve ser comparada a ela para apontar lacunas e correções necessárias.
