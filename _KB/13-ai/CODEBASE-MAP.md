# Mapa do código e responsabilidades

## Estrutura principal

- [API](../../../API): backend do sistema.
- [interface-administrativa](../../../interface-administrativa): frontend administrativo.
- [Docs](../..): documentação, requisitos e conhecimento do projeto.
- [base_fieldops](../../references/base_fieldops): documentação original do produto e do domínio.

## Mapeamento por componente

### API / auth
Responsabilidade: autenticação, sessão e segurança.
Tecnologias: Java, Spring Boot, Spring Security, JWT.
Dependências: user, review, audit.
Documentação: [../05-api/api-overview.md](../05-api/api-overview.md), [../07-security/security-overview.md](../07-security/security-overview.md)

### API / template
Responsabilidade: criação, versionamento e publicação de templates.
Tecnologias: Java, JPA, Flyway.
Dependências: inspection, domain model.
Documentação: [../01-product/product-business-rules.md](../01-product/product-business-rules.md)

### API / inspection
Responsabilidade: criação, execução e fluxo da inspeção.
Tecnologias: Java, Spring Boot, JPA.
Dependências: template, evidence, review, audit.
Documentação: [../02-domain/domain-overview.md](../02-domain/domain-overview.md)

### API / evidence
Responsabilidade: upload, vinculação e rastreabilidade de evidências.
Tecnologias: Java, storage abstração, arquivos e metadados.
Dependências: inspection, response, NC.
Documentação: [../06-data/data-model.md](../06-data/data-model.md)

### API / review
Responsabilidade: revisão, aprovação e reprovação.
Tecnologias: Java, Spring Boot.
Dependências: inspection, audit.
Documentação: [../01-product/user-flows.md](../01-product/user-flows.md)

### API / audit
Responsabilidade: registros históricos e eventos críticos.
Tecnologias: Java, JPA.
Dependências: all critical flows.
Documentação: [../06-data/data-model.md](../06-data/data-model.md)

### interface-administrativa
Responsabilidade: painel administrativo e visualização.
Tecnologias: Angular, TypeScript, Material.
Dependências: API REST.
Documentação: [../../../interface-administrativa/README.md](../../../interface-administrativa/README.md)

## Como usar este mapa

Para alterar uma funcionalidade, iniciar pela documentação do domínio e pela decisão relevante; depois localizar o componente no código e verificar se o comportamento atual está alinhado com a base documental.
