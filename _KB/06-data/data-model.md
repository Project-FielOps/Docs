---
source: base_fieldops/Projeto FieldOps/10 - Modelo de Dados a332594f77cf822ba17501051650b5cf.md
origin: ../references/base_fieldops/Projeto FieldOps/10 - Modelo de Dados a332594f77cf822ba17501051650b5cf.md
status: active
last_reviewed: 2026-09-05
migration_note: ../migration-inventory.csv
---

# Modelo de Dados (Canônico)

(Cópia integral de `base_fieldops/Projeto FieldOps/10 - Modelo de Dados`)

## 10.1 Objetivo do modelo

O modelo de dados do FieldOps deverá suportar todo o ciclo de vida das inspeções:

```
Cadastro do cliente
	↓
Cadastro dos locais e equipamentos
	↓
Criação do modelo de inspeção
	↓
Publicação de uma versão imutável
	↓
Agendamento da inspeção
	↓
Criação do snapshot do checklist
	↓
Execução em campo
	↓
Registro de respostas e evidências
	↓
Sincronização
	↓
Revisão e aprovação
	↓
Auditoria e preservação do histórico
```

O FieldOps utilizará dois contextos de persistência:

| Contexto | Tecnologia | Responsabilidade |
| --- | --- | --- |
| Persistência central | PostgreSQL | Fonte oficial dos dados sincronizados, regras de integridade, histórico e auditoria |
| Persistência local | SQLite | Operação offline, armazenamento das inspeções do técnico, respostas, evidências pendentes e fila de sincronização |

---

## 10.2 Princípios de modelagem

### Identificadores globais

As entidades deverão utilizar identificadores UUID.

Isso permitirá:

- criar registros no dispositivo sem depender do servidor;
- evitar identificadores locais temporários;
- reduzir remapeamentos durante a sincronização;
- reenviar operações sem criar registros duplicados;
- identificar entidades de forma consistente entre SQLite e PostgreSQL.

### Separação entre modelo e execução

O modelo reutilizável de inspeção não deverá ser confundido com uma inspeção realizada.

```
Modelo de inspeção
	↓
Versão publicada
	↓
Inspeção agendada
	↓
Snapshot dos itens
	↓
Respostas
```

O modelo define como futuras inspeções deverão funcionar.

A inspeção representa uma execução real em determinada data, cliente, local ou equipamento.

### Versionamento imutável

Uma versão publicada de um modelo não poderá ser alterada de forma destrutiva.

Quando houver mudanças no checklist, uma nova versão deverá ser criada.

Isso garante que inspeções antigas continuem associadas ao conteúdo que estava válido no momento do agendamento.

### Snapshot da inspeção

Cada inspeção possuirá uma cópia dos itens da versão utilizada.

Essa cópia será armazenada em `InspectionItemSnapshot`.

O snapshot preservará:

- título da seção;
- ordem da seção;
- texto do item;
- descrição;
- tipo de resposta;
- obrigatoriedade;
- regras de observação;
- regras de evidência;
- opções de seleção;
- ordem do item.

### Preservação histórica

Entidades utilizadas por inspeções não deverão ser excluídas fisicamente pelo fluxo comum.

Deverão ser utilizados:

- inativação;
- exclusão lógica;
- versionamento;
- snapshots;
- eventos de auditoria.

### Estado de negócio e sincronização

O estado da inspeção deverá permanecer separado do estado de sincronização do dispositivo.

Exemplo:

```
Estado de negócio: EM_ANDAMENTO
Estado local: PENDENTE
```

Isso significa que a inspeção está sendo executada, mas possui alterações locais ainda não enviadas.

### Arquivos fora do banco relacional

Fotografias e outros arquivos grandes não serão armazenados diretamente no PostgreSQL.

O banco guardará:

- identificador da evidência;
- tipo;
- tamanho;
- formato;
- chave do arquivo;
- vínculo com a inspeção;
- vínculo com a resposta ou não conformidade;
- datas de captura e envio.

O arquivo será mantido em armazenamento de objetos ou mecanismo equivalente.

### Controle de concorrência

Entidades sujeitas a alterações concorrentes deverão possuir controle de versão otimista.

Exemplo:

```
version = 3
```

Uma atualização baseada na versão 2 deverá ser rejeitada ou tratada como conflito caso o servidor já esteja na versão 3.

---

## 10.3 Diagrama conceitual textual

```
USER
 ├── cria ou agenda ── N INSPECTION
 ├── executa ───────── N INSPECTION
 ├── revisa ────────── N INSPECTION_REVIEW
 └── realiza ações ─── N AUDIT_EVENT

CLIENT 1 ── N INSPECTION_SITE
INSPECTION_SITE 1 ── 0..N EQUIPMENT

INSPECTION_TEMPLATE 1 ── N INSPECTION_TEMPLATE_VERSION
INSPECTION_TEMPLATE_VERSION 1 ── N TEMPLATE_SECTION
TEMPLATE_SECTION 1 ── N TEMPLATE_ITEM

INSPECTION_TEMPLATE_VERSION 1 ── N INSPECTION

CLIENT 1 ── N INSPECTION
INSPECTION_SITE 1 ── N INSPECTION
EQUIPMENT 1 ── 0..N INSPECTION
USER 1 ── N INSPECTION

INSPECTION 1 ── N INSPECTION_ITEM_SNAPSHOT
INSPECTION_ITEM_SNAPSHOT 1 ── 0..1 INSPECTION_RESPONSE
INSPECTION_RESPONSE 1 ── 0..N EVIDENCE

INSPECTION 1 ── 0..N NON_CONFORMITY
INSPECTION_RESPONSE 1 ── 0..N NON_CONFORMITY
NON_CONFORMITY 1 ── 0..N EVIDENCE

INSPECTION 1 ── 0..N INSPECTION_REVIEW
INSPECTION 1 ── N AUDIT_EVENT
```

---

## 10.4 Como interpretar as cardinalidades

### Cardinalidade `1 ── N`

Representa um relacionamento de um para muitos.

Exemplo:

```
CLIENT 1 ── N INSPECTION_SITE
```

Um cliente pode possuir vários locais de inspeção.

Cada local pertence a um cliente.

---

## 10.5 Estrutura organizacional

## 10.5.1 Client

A entidade `Client` representa a empresa, instituição ou organização atendida pela operação de inspeções.

Um cliente poderá possuir vários locais de inspeção.

```
CLIENT
    ├── INSPECTION_SITE
    ├── INSPECTION_SITE
    └── INSPECTION_SITE
```

### Campos

| Campo | Tipo sugerido | Regra |
| --- | --- | --- |
| id | UUID | Chave primária |
| name | string | Nome de exibição obrigatório |
| legal_name | string | Razão social opcional no MVP |
| document | string | Documento opcional e validado |
| email | string | Opcional |
| phone | string | Opcional |
| status | enum | ACTIVE ou INACTIVE |
| created_at | datetime | Gerado pelo servidor |
| updated_at | datetime | Gerado pelo servidor |
| version | integer | Controle otimista |

### Relacionamentos

```
CLIENT 1 ── N INSPECTION_SITE
CLIENT 1 ── N INSPECTION
```

Um cliente inativo continuará relacionado às inspeções históricas, mas não poderá ser selecionado em novos agendamentos.

---

## 10.5.2 InspectionSite

A entidade `InspectionSite` representa uma unidade, fábrica, loja, obra, depósito, prédio, setor ou outra localização pertencente ao cliente.

### Exemplo

```
Cliente: Indústria Alfa
    ├── Unidade Sorocaba
    ├── Unidade Itu
    └── Unidade Campinas
```

### Campos

| Campo | Tipo sugerido | Regra |
| --- | --- | --- |
| id | UUID | Chave primária |
| client_id | UUID | Cliente proprietário |
| name | string | Obrigatório |
| description | string | Opcional |
| address_line | string | Endereço |
| city | string | Cidade |
| state | string | Estado |
| postal_code | string | CEP |
| latitude | decimal | Coordenada opcional |
| longitude | decimal | Coordenada opcional |
| contact_name | string | Responsável local opcional |
| contact_phone | string | Opcional |
| status | enum | ACTIVE ou INACTIVE |
| created_at | datetime | Auditoria |
| updated_at | datetime | Auditoria |
| version | integer | Controle otimista |
*** End Patch
