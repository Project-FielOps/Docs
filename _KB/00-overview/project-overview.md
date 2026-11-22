# Visão geral do projeto

## Projeto

O FieldOps é uma plataforma para planejar, executar e revisar inspeções técnicas em campo. O objetivo é substituir formulários impressos, planilhas, fotos dispersas e mensagens fragmentadas por um fluxo digital com rastreabilidade, evidência e revisão.

## Problema resolvido

A base documental original descreve um problema claro: a execução de inspeção técnica em campo envolve múltiplos arquivos, múltiplas etapas e baixa rastreabilidade de decisões e evidências. O sistema centraliza:

- planejamento da inspeção;
- execução remota ou em campo;
- coleta de fotos e observações;
- revisão e aprovação; 
- auditoria e histórico.

## Público e usuários

A base documental identifica os principais atores:

- ADMIN: cadastros e permissões;
- SUPERVISOR: planejamento, acompanhamento e revisão;
- TECHNICIAN: execução e respostas da inspeção;
- CLIENT_VIEWER: visualização futura de resultados;
- sistema: auditoria, sincronização e processamento automatizado.

## Contexto operacional

O cenário predominante do MVP é uma organização prestadora atendendo vários clientes. A estrutura de domínio é:

- cliente;
- local;
- equipamento;
- template/modelo de inspeção;
- inspeção vinculada a modelo publicado;
- execução por técnico;
- revisão por supervisor.

## Objetivos centrais

A base documental organiza os objetivos em torno de cinco eixos:

- autonomia administrativa e controle de acesso;
- padronização de inspeções e preservação do checklist;
- registro de execução, fotos, QR e localização;
- revisão com decisão e histórico rastreável;
- demonstração e entrega com evidências reproduzíveis.

## Escopo do MVP

O MVP inclui:

- autenticação e autorização;
- gestão de usuários, clientes, locais e equipamentos;
- criação e publicação de templates;
- agendamento e atribuição de inspeções;
- execução pelo técnico responsável;
- coleta de evidências e não conformidades;
- revisão pelo supervisor responsável;
- auditoria mínima e rastreabilidade.

## Fora do escopo do MVP

O material original descreve como fora do MVP elementos como:

- portal do cliente completo;
- multiempresa;
- manutenção preventiva e ERP;
- gestão de ordens de serviço;
- IA, OCR e automação avançada;
- integração massiva com sistemas externos.

## Status do código atual

A implementação presente no repositório confirma parte do MVP em backend e painel administrativo, mas não a totalidade do escopo previsto na base documental. Em especial:

- a API e o painel administrativo estão implementados;
- o mobile está planejado e faz parte do MVP funcional;
- a sincronização offline não está implementada como parte de um contrato concluído;
- AWS e storage de objetos são requisitos obrigatórios da arquitetura desejada, mas sua implementação real ainda exige definição técnica.

## Separação AS-IS vs TO-BE

- AS-IS: backend e administração implementados no repositório.
- TO-BE: mobile, sincronização offline, storage em objeto, infraestrutura AWS e dados de produção em cloud.

## Fontes

- [../../references/base_fieldops/Projeto FieldOps](../../references/base_fieldops/Projeto%20FieldOps)
- [../01-product/requirements.md](../01-product/requirements.md)
- [../03-architecture/system-overview.md](../03-architecture/system-overview.md)
