# Notação do Banco de Dados - Lógica de Negócio

## 📋 Índice
- [Modelos Base](#modelos-base)
- [Fluxo de Solicitação de Crédito](#fluxo-de-solicitação-de-crédito)
- [Fluxo de Proposta de Crédito](#fluxo-de-proposta-de-crédito)
- [Gestão de Linhas de Crédito](#gestão-de-linhas-de-crédito)
- [Análise de Risco](#análise-de-risco)
- [Comunicação](#comunicação)
- [Auditoria e Histórico](#auditoria-e-histórico)
- [Regras de Negócio e Fluxos de Trabalho](#regras-de-negócio-e-fluxos-de-trabalho)

## Modelos Base

### BaseModel
Modelo base abstrato que fornece campos comuns para todos os modelos

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `uuid` | UUID | Identificador único para cada registro |
| `created_at` | DateTime | Data e hora de criação |
| `created_by` | ForeignKey | Usuário que criou o registro |
| `updated_at` | DateTime | Data e hora da última atualização |
| `updated_by` | ForeignKey | Usuário que atualizou o registro pela última vez |

## Fluxo de Solicitação de Crédito

### CreditSolicitation
Modelo principal para solicitações de crédito

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `protocol` | CharField | Identificador único da solicitação |
| `status` | CharField | Status atual da solicitação |
| `public_user` | ForeignKey | Link para o usuário público que fez a solicitação |
| `credit_line` | ForeignKey | Linha de crédito associada |
| `proposal` | ForeignKey | Link para a proposta de crédito (se existir) |
| `agent` | ForeignKey | Agente designado |
| `backoffice` | ForeignKey | Usuário de backoffice designado |

### SolicitationData
Armazena detalhes da solicitação de crédito

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `line_of_credit` | CharField | Tipo de crédito solicitado |
| `credit_purpose` | CharField | Finalidade do crédito |
| `claimed_amount` | DecimalField | Valor solicitado |
| `duration` | IntegerField | Duração do empréstimo |
| `interest_rate` | DecimalField | Taxa de juros |
| `grace_period` | CharField | Período de carência |
| `amortization_term` | CharField | Prazo de amortização |
| `payment_term` | CharField | Condições de pagamento |
| `shopping_frequency` | CharField | Frequência de compras |

### SolicitationProperty
Armazena informações sobre propriedades para a solicitação de crédito

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `property_type` | CharField | Tipo de propriedade |
| `property_conditions` | CharField | Condições da propriedade |
| `has_movable_property` | BooleanField | Indica se existem bens móveis |
| `is_own_property` | BooleanField | Indica se é propriedade própria |
| `physical_structure_of_the_business` | TextField | Detalhes da estrutura do negócio |

## Fluxo de Proposta de Crédito

### CreditProposal
Modelo principal para propostas de crédito

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `protocol` | CharField | Identificador único da proposta |
| `status` | CharField | Status atual da proposta |
| `agent` | ForeignKey | Agente designado |
| `backoffice` | ForeignKey | Usuário de backoffice designado |

**Regras de Negócio:**
- Pode agregar de 1 a 5 solicitações de crédito
- Possui tarefas padrão para gestão do fluxo de trabalho

### ProposalGuarantor
Armazena informações do fiador

| Categoria | Campos |
|----------|---------|
| Informações Pessoais | nome, CPF, etc. |
| Detalhes de Contato | telefone, email, endereço |
| Informações Financeiras | renda, bens |
| Informações Profissionais | ocupação, empregador |
| Informações de Documentos | RG, números de registro |

### ProposalOpinion
Armazena pareceres de análise de crédito

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `department` | CharField | Departamento que emitiu o parecer |
| `evaluator` | ForeignKey | Usuário responsável pela avaliação |
| `status` | CharField | Status do parecer |
| `opinion_text` | TextField | Parecer detalhado |
| `given_at` | DateTimeField | Data do parecer |
| `updated_at` | DateTimeField | Data da última atualização |

## Gestão de Linhas de Crédito

### CreditLine
Define as linhas de crédito disponíveis

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `name` | CharField | Nome da linha de crédito |
| `description` | TextField | Descrição detalhada |
| `min_financing_months` | PositiveIntegerField | Período mínimo de financiamento |
| `max_financing_months` | PositiveIntegerField | Período máximo de financiamento |
| `max_grace_months` | PositiveIntegerField | Período máximo de carência |
| `interest_rate` | DecimalField | Taxa de juros base |
| `tac_percentage` | DecimalField | Taxa de abertura de crédito |
| `max_financing_value` | DecimalField | Valor máximo de financiamento |
| `min_financing_value` | DecimalField | Valor mínimo de financiamento |
| `additional_rates` | ManyToManyField | Taxas adicionais aplicáveis |

## Análise de Risco

### SPCConsultation
Armazena resultados de consultas ao SPC (Serviço de Proteção ao Crédito)

| Categoria | Campos |
|----------|---------|
| Informações do Cliente | nome, CPF, data de nascimento |
| Detalhes do Endereço | rua, cidade, estado, CEP |
| Informações de Restrição | tem_restricoes, detalhes_restricoes |
| Cheques Devolvidos | tem_cheques_devolvidos, detalhes_cheques |
| Dados da Consulta | data_consulta, dados_originais |

## Comunicação

### SMSLog
Registra comunicações por SMS

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `msisdn` | CharField | Número de telefone |
| `message` | TextField | Conteúdo do SMS |
| `reference` | CharField | Identificador de referência |
| `status` | CharField | Status da mensagem |
| `created_at` | DateTimeField | Data e hora de envio |

## Auditoria e Histórico

### Auditor
Registra operações do sistema

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `total_time` | FloatField | Tempo total da operação |
| `python_time` | FloatField | Tempo de processamento Python |
| `db_time` | FloatField | Tempo de operação no banco de dados |
| `total_queries` | IntegerField | Número de consultas |
| `path` | TextField | Endpoint da API |
| `method` | CharField | Método HTTP |
| `response_status_code` | IntegerField | Status HTTP |
| `ip` | GenericIPAddressField | IP do cliente |
| `proxy_verified` | BooleanField | Status de verificação do proxy |

### SolicitationHistory/ProposalHistory
Registra alterações em solicitações e propostas

| Campo | Tipo | Descrição |
|-------|------|-------------|
| `action` | CharField | Descrição da ação |
| `timestamp` | DateTimeField | Momento em que a ação ocorreu |

## Regras de Negócio e Fluxos de Trabalho

### 1. Fluxo de Solicitação de Crédito
- Inicia com solicitação do usuário público
- Passa por estágios de validação
- Pode ser vinculada a uma proposta de crédito
- Possui tarefas e documentos associados

### 2. Fluxo de Proposta de Crédito
- Pode agregar de 1 a 5 solicitações de crédito
- Possui estágios definidos:
  - Registro
  - Validação
  - Análise
  - Finalização
- Requer pareceres de diferentes departamentos
- Registra todas as alterações no histórico

### 3. Análise de Risco
- Inclui consultas ao SPC
- Monitora restrições de crédito
- Acompanha cheques devolvidos
- Armazena histórico de consultas

### 4. Segurança
- Todos os modelos herdam do BaseModel para rastreamento
- Armazenamento seguro de dados com criptografia
- Controle de permissões de usuários
- Registro de operações

---

> **Nota:** Esta estrutura de banco de dados suporta um sistema abrangente de gestão de crédito com rastreamento adequado, análise e medidas de segurança implementadas. 
