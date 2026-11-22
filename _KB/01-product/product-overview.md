# Projeto e problema

O FieldOps é uma plataforma acadêmica que será desenvolvida para planejar, executar e revisar inspeções técnicas em campo. Substitui formulários impressos, planilhas, fotografias soltas e mensagens por um fluxo em que cada resposta mantém vínculo com inspeção, item, usuário e evidência.

A operação inicial é de uma organização prestadora que atende vários clientes. O técnico usa um aplicativo Android; administrador e supervisor usam o painel web; a API centraliza autorização, regras e persistência oficial. O aplicativo funciona sem rede após baixar os dados autorizados.

## Objetivos rastreáveis

| ID | Objetivo | Domínios que o realizam |
|---|---|---|
| OBJ-001 | Dar autonomia administrativa e controlar acesso | Fundação, autenticação, cadastros |
| OBJ-002 | Padronizar inspeções e preservar o checklist utilizado | Modelos e planejamento |
| OBJ-003 | Registrar execução, fotos, QR e localização sem perder trabalho | Execução, recursos nativos, offline e sync |
| OBJ-004 | Revisar resultados com decisão e histórico rastreáveis | Revisão e auditoria |
| OBJ-005 | Demonstrar integração e aprendizagem com evidências reproduzíveis | Qualidade, entrega e todos os domínios |

Esses IDs organizam os objetivos existentes; não introduzem metas quantitativas de produtividade ou promessa de conformidade regulatória. O sucesso do MVP é demonstrado pelo [roteiro de aceitação](../04-development/demo.md), sem editar banco durante o fluxo.

## Usuários e stakeholders

As personas originais são fictícias: Carlos representa o técnico que precisa salvar respostas e fotos em rede instável; Marina representa a supervisora que agenda e revisa; Ana representa a administradora de cadastros e acessos; Roberto representa o cliente interessado nos resultados, cujo portal está fora do MVP.

O TAP identifica como gerentes Victor Toledo, José Eduardo, Giovani Miomoto, Luciano Menezes e Tobias Perassi. Esses nomes são registros do documento de 15/08/2026, não atribuições novas de tarefas. O patrocinador continua “professor responsável / coordenação acadêmica — a validar”; assinatura e data de aprovação estão em branco.

Também são interessados as equipes de desenvolvimento e testes, avaliadores, operação prestadora e clientes atendidos. Não há atribuição nominal confirmada por módulo. Ver [restrições acadêmicas](../11-planning/academic.md).

## Limites do problema

O sistema registra inspeções; não substitui um sistema corporativo completo de manutenção. Não executa diagnóstico automático, rastreamento contínuo de técnicos, faturamento, ERP, IoT ou tratamento completo de planos de ação no MVP. A ficha ilustrativa não estabelece normas técnicas, bloqueio operacional automático ou aprovação com restrições.
