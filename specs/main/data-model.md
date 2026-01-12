# Data Model: StelaCRM

**Date**: 2026-01-08  
**Feature**: StelaCRM - Plataforma SaaS Single-Tenant  
**Storage**: SQLite (embedded via PocketBase)

## Visão Geral

Modelo de dados para sistema CRM Single-Tenant. Todas as entidades são compartilhadas por todos os usuários da instalação (single-tenant), com controle de acesso via permissões de usuário.

## Entidades Principais

### Organization (Organização/Empresa)

Entidade única por instalação. Armazena configurações da empresa/organização.

**Campos**:
- `id` (string, PK): Identificador único
- `name` (string, 200 chars): Nome da empresa
- `settings` (JSON): Configurações gerais da organização (fuso horário: "America/Sao_Paulo", etc.)
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização

**Relacionamentos**:
- 1:1 com instalação (única organização por instalação)

**Validações**:
- `name`: obrigatório, máximo 200 caracteres (FR-068)
- `settings`: estrutura JSON válida (FR-069)

**Notas**:
- Criada durante onboarding inicial (User Story 1)
- Dados utilizados em relatórios, propostas, emails

---

### User (Usuário)

Usuários do sistema. Autenticação via email/senha. Autorização via perfis/permissões.

**Campos** (base PocketBase `users` collection + custom):
- `id` (string, PK): Identificador único
- `email` (string, único): Email do usuário (usado para login)
- `emailVisibility` (boolean): Visibilidade do email
- `username` (string, opcional)
- `password` (string, hashed): Senha (hash)
- `passwordHash` (string): Hash da senha (PocketBase)
- `name` (string, 200 chars): Nome completo do usuário
- `avatar` (file): Foto do usuário (arquivo)
- `profile` (relation): Referência ao Profile (perfil de permissões)
- `verified` (boolean): Email verificado
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização

**Relacionamentos**:
- N:1 com Profile (via `profile`)
- 1:N com Lead (via `assigned_user_id`)
- 1:N com Opportunity (via `assigned_user_id`)
- 1:N com Task (via `assigned_user_id`)
- 1:N com Note (via `author_id`)
- 1:N com ActivityLog (via `user_id`)

**Validações**:
- `email`: obrigatório, único, formato válido (FR-066): `^[^@]+@[^@]+\.[^@]+$`
- `name`: obrigatório, máximo 200 caracteres (FR-068)
- `email` não pode ser alterado após criação (FR-071)
- Usuário pode editar apenas: foto, nome, senha (FR-071)

**Notas**:
- Campo `preferences` NÃO existe (FR-070 removido)
- Todos os usuários acessam mesmos dados (com controle via permissões)

---

### Profile (Perfil de Permissões)

Conjunto de permissões atribuível a múltiplos usuários.

**Campos**:
- `id` (string, PK): Identificador único
- `name` (string, 200 chars): Nome do perfil (ex: "admin", "vendedor", "gerente")
- `description` (string, 1000 chars): Descrição do perfil
- `permissions` (JSON): Estrutura de permissões por recurso e ação
  ```json
  {
    "leads": {"read": true, "write": true, "delete": false},
    "opportunities": {"read": true, "write": true, "delete": false},
    "tasks": {"read": true, "write": true, "delete": true},
    "users": {"read": true, "write": false, "manage": false},
    // ... outros recursos
  }
  ```
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização

**Relacionamentos**:
- 1:N com User (via `profile` field em User)

**Validações**:
- `name`: obrigatório, único, máximo 200 caracteres (FR-068)
- `permissions`: estrutura JSON válida (FR-069, FR-033)
- Hierarquia implícita: write implica read, delete implica read (FR-033a)

**Recursos suportados** (FR-033):
- leads, oportunidades, tarefas, anotações, propostas, produtos, templates de email, workflows, relatórios, usuários, perfis, funis

**Ações básicas**: read, write, delete  
**Ações específicas**: manage (ex: users)

**Notas**:
- Perfis padrão têm escopos pré-definidos (admin vê todos, gerente vê equipe, vendedor vê próprios) (FR-033c)
- Filtros automáticos baseados em atribuição combinados com permissões (FR-033b)

---

### Funnel (Funil de Vendas)

Funil de vendas com etapas ordenadas.

**Campos**:
- `id` (string, PK): Identificador único
- `name` (string, 200 chars): Nome do funil
- `description` (string, 5000 chars): Descrição do funil
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização

**Relacionamentos**:
- 1:N com FunnelStage (via `funnel_id`)
- 1:N com Lead (via `funnel_id`)
- 1:N com Opportunity (via `funnel_id`)

**Validações**:
- `name`: obrigatório, máximo 200 caracteres (FR-068)
- `description`: máximo 5000 caracteres (FR-068)

**Notas**:
- Múltiplos funis podem existir (FR-004)
- Todos os funis são compartilhados (com controle via permissões)
- Funil padrão criado no onboarding (User Story 1)

---

### FunnelStage (Etapa do Funil)

Etapa dentro de um funil. Ordenadas por `order`.

**Campos**:
- `id` (string, PK): Identificador único
- `funnel_id` (string, FK): Referência ao Funnel
- `name` (string, 200 chars): Nome da etapa
- `order` (integer): Ordem da etapa no funil (para ordenação)
- `color` (string, 50 chars): Cor da etapa (hex, ex: "#FF5733")
- `icon` (string, 100 chars): Ícone da etapa (nome do ícone)
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização

**Relacionamentos**:
- N:1 com Funnel (via `funnel_id`)
- 1:N com Lead (via `stage_id`)
- 1:N com Opportunity (via `stage_id`)

**Validações**:
- `funnel_id`: obrigatório, referência válida
- `name`: obrigatório, máximo 200 caracteres (FR-068)
- `order`: obrigatório, único por funil

**Regras de Negócio**:
- Etapas são editáveis e reordenáveis (FR-005)
- Não pode deletar etapa se existirem oportunidades associadas (FR-006a)

**Notas**:
- Etapas padrão criadas no onboarding: Lead, Contato, Proposta, Negociação, Fechado

---

### Lead (Lead de Vendas)

Lead de vendas. Pode ser convertido em Opportunity.

**Campos**:
- `id` (string, PK): Identificador único
- `funnel_id` (string, FK): Referência ao Funnel
- `stage_id` (string, FK): Referência ao FunnelStage (etapa atual)
- `assigned_user_id` (string, FK, nullable): Referência ao User (responsável)
- `name` (string, 200 chars): Nome do lead
- `company` (string, 200 chars): Empresa
- `email` (string, 200 chars): Email
- `phone` (string, 20 chars): Telefone (normalizado: apenas dígitos)
- `source` (string, 200 chars): Origem do lead
- `qualification_score` (integer, nullable): Score de qualificação (0-100)
- `qualification_data` (JSON, nullable): Dados de qualificação (interesse, orçamento, timeline, etc.)
  ```json
  {
    "interest": "alto|medio|baixo",
    "budget": 50000.00,
    "timeline": "1-3 meses"
  }
  ```
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização
- `created_by` (string, FK): Referência ao User (criador)

**Relacionamentos**:
- N:1 com Funnel (via `funnel_id`)
- N:1 com FunnelStage (via `stage_id`)
- N:1 com User (via `assigned_user_id` e `created_by`)
- 1:1 com Opportunity (via conversão, quando convertido)
- 1:N com Task (via `lead_id`)
- 1:N com Note (via `lead_id`)

**Validações**:
- `funnel_id`: obrigatório, referência válida (FR-011)
- `stage_id`: obrigatório, referência válida (FR-011)
- `name`: obrigatório, máximo 200 caracteres (FR-068)
- `company`: máximo 200 caracteres (FR-068)
- `email`: formato válido (FR-066): `^[^@]+@[^@]+\.[^@]+$`, máximo 200 caracteres
- `phone`: formato válido (FR-065): após normalização (remover não-dígitos), `^\d{10,11}$` (10 ou 11 dígitos), máximo 20 caracteres (armazenado normalizado)
- `source`: máximo 200 caracteres (FR-068)
- `qualification_score`: 0-100, nullable

**Regras de Negócio**:
- Criado na etapa inicial do funil selecionado (FR-011)
- Pode ser convertido em Opportunity atribuindo valor estimado (FR-013)
- Detecção de duplicados: email OU telefone (FR-019)

**Notas**:
- Quando convertido em Opportunity, Lead pode ser removido ou mantido com referência (decisão de implementação)

---

### Opportunity (Oportunidade de Venda)

Oportunidade de venda (lead convertido). Estende Lead com valor e status de negociação.

**Campos**:
- `id` (string, PK): Identificador único
- `lead_id` (string, FK, nullable): Referência ao Lead (se convertido de lead)
- `funnel_id` (string, FK): Referência ao Funnel
- `stage_id` (string, FK): Referência ao FunnelStage (etapa atual)
- `assigned_user_id` (string, FK, nullable): Referência ao User (responsável)
- `name` (string, 200 chars): Nome da oportunidade
- `company` (string, 200 chars): Empresa
- `email` (string, 200 chars): Email
- `phone` (string, 20 chars): Telefone (normalizado)
- `source` (string, 200 chars): Origem
- `estimated_value` (decimal): Valor estimado (R$), máximo 99.999.999,99 (FR-067)
- `negotiation_status` (enum): Status de negociação (FR-013a):
  - "em_andamento" (padrão ao criar) (FR-013c)
  - "pausado"
  - "vendido"
  - "perdido"
- `products` (JSON, nullable): Produtos/serviços associados
  ```json
  [
    {"product_id": "prod123", "quantity": 2, "unit_price": 1000.00}
  ]
  ```
- `qualification_score` (integer, nullable): Score de qualificação
- `qualification_data` (JSON, nullable): Dados de qualificação
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização
- `created_by` (string, FK): Referência ao User (criador)

**Relacionamentos**:
- N:1 com Lead (via `lead_id`, opcional)
- N:1 com Funnel (via `funnel_id`)
- N:1 com FunnelStage (via `stage_id`)
- N:1 com User (via `assigned_user_id` e `created_by`)
- 1:N com Proposal (via `opportunity_id`)
- 1:N com Task (via `opportunity_id`)
- 1:N com Note (via `opportunity_id`)
- 1:N com StageHistory (via `opportunity_id`)
- 1:N com StatusHistory (via `opportunity_id`)

**Validações**:
- `funnel_id`: obrigatório, referência válida
- `stage_id`: obrigatório, referência válida
- `name`: obrigatório, máximo 200 caracteres (FR-068)
- `estimated_value`: máximo 99.999.999,99 (FR-067)
- `negotiation_status`: obrigatório, valores válidos (FR-013d)
- Campos herdados de Lead: mesmo formato de validação

**Regras de Negócio**:
- Status padrão "Em andamento" ao criar (FR-013c)
- Status é independente da etapa do funil (FR-013b)
- Qualquer mudança de status é permitida (FR-013e)
- Alertar usuário em mudanças incomuns (Vendido → Em andamento, Perdido → Vendido) (FR-013f)
- Histórico de mudanças de status mantido (FR-014a)
- Histórico de mudanças de etapa mantido (FR-014)
- Valor total no kanban: apenas oportunidades com status "Em andamento" ou "Pausado" (FR-007e, FR-039a)

**Notas**:
- Status "Vendido" e "Perdido" excluídos de pipeline, mas incluídos em relatórios históricos (FR-039a, FR-051b)

---

### Task (Tarefa)

Tarefa vinculada a lead/oportunidade ou independente.

**Campos**:
- `id` (string, PK): Identificador único
- `lead_id` (string, FK, nullable): Referência ao Lead (opcional)
- `opportunity_id` (string, FK, nullable): Referência ao Opportunity (opcional)
- `assigned_user_id` (string, FK): Referência ao User (responsável)
- `type` (enum): Tipo de tarefa (FR-024):
  - "call" (ligar)
  - "email" (email)
  - "meeting" (reunião)
  - "other" (outro)
- `description` (string, 10000 chars): Descrição da tarefa
- `scheduled_at` (datetime): Data/hora agendada
- `status` (enum): Status (FR-027):
  - "pending" (pendente)
  - "completed" (concluída)
- `completed_at` (datetime, nullable): Data de conclusão
- `priority` (enum, nullable): Prioridade (baixa, média, alta)
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização
- `created_by` (string, FK): Referência ao User (criador)

**Relacionamentos**:
- N:1 com Lead (via `lead_id`, opcional)
- N:1 com Opportunity (via `opportunity_id`, opcional)
- N:1 com User (via `assigned_user_id` e `created_by`)

**Validações**:
- `assigned_user_id`: obrigatório, referência válida
- `type`: obrigatório, valores válidos
- `description`: obrigatório, máximo 10000 caracteres (FR-068)
- `scheduled_at`: obrigatório
- `status`: obrigatório, valores válidos

**Regras de Negócio**:
- Pode ser vinculada a lead OU oportunidade (não ambos obrigatórios)
- Ordenação padrão por `scheduled_at` (FR-027a)
- Filtros: responsável, status, tipo, período (FR-027b)

---

### Note (Anotação)

Anotação vinculada a lead/oportunidade.

**Campos**:
- `id` (string, PK): Identificador único
- `lead_id` (string, FK, nullable): Referência ao Lead (opcional)
- `opportunity_id` (string, FK, nullable): Referência ao Opportunity (opcional)
- `author_id` (string, FK): Referência ao User (autor)
- `content` (string, 10000 chars): Conteúdo da anotação (texto livre)
- `created` (datetime): Data/hora de criação
- `updated` (datetime): Data de última atualização

**Relacionamentos**:
- N:1 com Lead (via `lead_id`, opcional)
- N:1 com Opportunity (via `opportunity_id`, opcional)
- N:1 com User (via `author_id`)

**Validações**:
- `author_id`: obrigatório, referência válida
- `content`: obrigatório, máximo 10000 caracteres (FR-068)

**Regras de Negócio**:
- Deve ser vinculada a lead OU oportunidade (pelo menos um)
- Histórico cronológico mantido (FR-026)

---

### Product (Produto/Serviço)

Produto ou serviço vendável.

**Campos**:
- `id` (string, PK): Identificador único
- `name` (string, 200 chars): Nome do produto/serviço
- `description` (string, 5000 chars): Descrição
- `unit_price` (decimal): Preço unitário (R$), máximo 99.999.999,99 (FR-067)
- `category` (string, 200 chars): Categoria
- `status` (enum): Status (FR-028):
  - "active" (ativo)
  - "inactive" (inativo)
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização
- `created_by` (string, FK): Referência ao User (criador)

**Relacionamentos**:
- 1:N com ProposalItem (via `product_id`)

**Validações**:
- `name`: obrigatório, máximo 200 caracteres (FR-068)
- `description`: máximo 5000 caracteres (FR-068)
- `unit_price`: obrigatório, máximo 99.999.999,99 (FR-067)
- `category`: máximo 200 caracteres (FR-068)
- `status`: obrigatório, valores válidos

---

### Proposal (Proposta/Cotação)

Proposta comercial vinculada a oportunidade.

**Campos**:
- `id` (string, PK): Identificador único
- `opportunity_id` (string, FK): Referência ao Opportunity
- `items` (JSON): Lista de produtos com quantidades e descontos
  ```json
  [
    {
      "product_id": "prod123",
      "name": "Produto X",
      "quantity": 2,
      "unit_price": 1000.00,
      "discount": 100.00,
      "total": 1900.00
    }
  ]
  ```
- `subtotal` (decimal): Subtotal (R$)
- `discount_total` (decimal): Total de descontos (R$)
- `total` (decimal): Total (R$)
- `status` (enum): Status (FR-029):
  - "draft" (rascunho)
  - "sent" (enviada)
  - "accepted" (aceita)
  - "rejected" (recusada)
- `sent_at` (datetime, nullable): Data de envio
- `pdf_file` (file, nullable): Arquivo PDF gerado
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização
- `created_by` (string, FK): Referência ao User (criador)

**Relacionamentos**:
- N:1 com Opportunity (via `opportunity_id`)
- N:1 com User (via `created_by`)

**Validações**:
- `opportunity_id`: obrigatório, referência válida
- `items`: estrutura JSON válida (FR-069)
- `subtotal`, `discount_total`, `total`: calculados automaticamente (FR-030)
- `status`: obrigatório, valores válidos

**Regras de Negócio**:
- Totais calculados automaticamente (FR-030)
- PDF gerado quando solicitado (FR-031)
- Envio por email com PDF anexado (FR-032)
- Se envio falhar, status permanece "draft" ou "sent", alerta ao usuário (FR-076)

---

### EmailTemplate (Template de Email)

Template de email com variáveis dinâmicas.

**Campos**:
- `id` (string, PK): Identificador único
- `name` (string, 200 chars): Nome do template
- `subject` (string, 500 chars): Assunto do email
- `body` (string, 10000 chars): Corpo do email (HTML/texto com variáveis)
- `type` (enum): Tipo de template:
  - "proposal" (proposta)
  - "followup" (follow-up)
  - "other" (outro)
- `variables` (JSON, nullable): Variáveis disponíveis no template
  ```json
  {
    "customer_name": "Nome do cliente",
    "proposal_value": "Valor da proposta",
    "proposal_link": "Link da proposta"
  }
  ```
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização
- `created_by` (string, FK): Referência ao User (criador)

**Validações**:
- `name`: obrigatório, máximo 200 caracteres (FR-068)
- `subject`: obrigatório, máximo 500 caracteres (FR-068)
- `body`: obrigatório, máximo 10000 caracteres (FR-068)

**Notas**:
- Variáveis dinâmicas substituídas ao usar template (FR-043)

---

### WorkflowRule (Regra de Workflow)

Regra de workflow automatizado.

**Campos**:
- `id` (string, PK): Identificador único
- `name` (string, 200 chars): Nome da regra
- `trigger` (enum): Evento trigger (FR-047):
  - "lead_created"
  - "stage_changed"
  - "status_changed"
  - "opportunity_created"
- `conditions` (JSON, nullable): Condições opcionais
  ```json
  {
    "stage_id": "stage123",
    "source": "website"
  }
  ```
- `actions` (JSON): Ações a executar
  ```json
  [
    {
      "type": "create_task",
      "task_type": "call",
      "assigned_user_id": "user123",
      "scheduled_at": "+1 day"
    },
    {
      "type": "send_email",
      "template_id": "template123",
      "to": "lead_email"
    }
  ]
  ```
- `status` (enum): Status (FR-050):
  - "active" (ativo)
  - "inactive" (inativo)
- `created` (datetime): Data de criação
- `updated` (datetime): Data de última atualização
- `created_by` (string, FK): Referência ao User (criador)

**Validações**:
- `name`: obrigatório, máximo 200 caracteres (FR-068)
- `trigger`: obrigatório, valores válidos
- `actions`: estrutura JSON válida (FR-069)
- `status`: obrigatório, valores válidos

**Regras de Negócio**:
- Valida usuário alvo antes de executar (FR-048a)
- Pula ação se usuário inválido, registra em ActivityLog (FR-048b)
- Execução registrada no histórico (FR-049)

---

### ActivityLog (Log de Atividades)

Log de atividades do sistema (auditoria).

**Campos**:
- `id` (string, PK): Identificador único
- `user_id` (string, FK, nullable): Referência ao User (quem executou, nullable se sistema)
- `action` (string, 200 chars): Tipo de ação (ex: "lead_created", "stage_changed", "workflow_executed")
- `entity_type` (string, 100 chars): Tipo de entidade (ex: "lead", "opportunity")
- `entity_id` (string, FK, nullable): ID da entidade afetada
- `details` (JSON, nullable): Detalhes da ação
  ```json
  {
    "old_stage": "stage1",
    "new_stage": "stage2",
    "workflow_id": "workflow123"
  }
  ```
- `created` (datetime): Timestamp da ação

**Relacionamentos**:
- N:1 com User (via `user_id`, opcional)

**Validações**:
- `action`: obrigatório, máximo 200 caracteres (FR-068)
- `entity_type`: máximo 100 caracteres (FR-068)

**Notas**:
- Registra operações críticas (FR-003)
- Registra execuções de workflow (sucessos e falhas) (FR-048b, FR-049)

---

### StageHistory (Histórico de Mudanças de Etapa)

Histórico de mudanças de etapa de oportunidades.

**Campos**:
- `id` (string, PK): Identificador único
- `opportunity_id` (string, FK): Referência ao Opportunity
- `user_id` (string, FK): Referência ao User (quem alterou)
- `old_stage_id` (string, FK, nullable): Etapa anterior
- `new_stage_id` (string, FK): Nova etapa
- `created` (datetime): Timestamp da mudança

**Relacionamentos**:
- N:1 com Opportunity (via `opportunity_id`)
- N:1 com User (via `user_id`)
- N:1 com FunnelStage (via `old_stage_id` e `new_stage_id`)

**Notas**:
- Mantém histórico completo (FR-014)

---

### StatusHistory (Histórico de Mudanças de Status)

Histórico de mudanças de status de negociação de oportunidades.

**Campos**:
- `id` (string, PK): Identificador único
- `opportunity_id` (string, FK): Referência ao Opportunity
- `user_id` (string, FK): Referência ao User (quem alterou)
- `old_status` (enum): Status anterior (em_andamento, pausado, vendido, perdido)
- `new_status` (enum): Novo status
- `created` (datetime): Timestamp da mudança

**Relacionamentos**:
- N:1 com Opportunity (via `opportunity_id`)
- N:1 com User (via `user_id`)

**Notas**:
- Mantém histórico completo (FR-014a)

---

## Validações Gerais

### Telefone (FR-065)
- Formatos aceitos: `(11) 98765-4321`, `11987654321`, `(11) 9876-5432`
- Validação: após remover caracteres não numéricos, `^\d{10,11}$`
- Armazenamento: apenas dígitos (normalizado)

### Email (FR-066)
- Validação simplificada: `^[^@]+@[^@]+\.[^@]+$`
- Não segue RFC completo

### Valores Monetários (FR-067)
- Limite máximo: R$ 99.999.999,99
- Valores acima são rejeitados

### Limites de Texto (FR-068)
- Campos curtos (name, company, source, category): 200 caracteres
- Campos de descrição: 5000 caracteres
- Campos de texto longo (notes content, email body): 10000 caracteres

### Campos JSON (FR-069)
- Estrutura básica documentada acima
- Validação completa de schema pode ser definida na implementação

---

## Índices Recomendados

- `User.email`: único
- `User.profile`: índice para joins
- `Lead.funnel_id`, `Lead.stage_id`, `Lead.assigned_user_id`: índices para filtros
- `Opportunity.funnel_id`, `Opportunity.stage_id`, `Opportunity.assigned_user_id`, `Opportunity.negotiation_status`: índices para filtros
- `Task.assigned_user_id`, `Task.scheduled_at`, `Task.status`: índices para listagem
- `ActivityLog.entity_type`, `ActivityLog.entity_id`, `ActivityLog.created`: índices para auditoria

---

## Notas de Implementação

### PocketBase Collections

As entidades acima serão implementadas como Collections no PocketBase. Algumas entidades podem aproveitar collections padrão do PocketBase:

- `User`: Usa collection `users` padrão do PocketBase + campos customizados
- `Organization`: Collection customizada `organizations`

### Relacionamentos

PocketBase suporta relacionamentos via campos de referência (relation fields). Relacionamentos N:1 são implementados via `relation` field. Relacionamentos 1:N são implícitos (não precisam campo adicional).

### Histórico

- `StageHistory` e `StatusHistory`: Collections separadas para histórico
- Alternativa: usar `ActivityLog` para histórico (decisão de implementação)

### Conversão Lead → Opportunity

Quando Lead é convertido em Opportunity:
- Opção 1: Criar Opportunity, manter Lead com referência
- Opção 2: Criar Opportunity, remover Lead
- Opção 3: Transformar Lead em Opportunity (adicionar campos)

**Recomendação**: Opção 1 (manter Lead com referência via `lead_id` em Opportunity) para manter histórico completo.
