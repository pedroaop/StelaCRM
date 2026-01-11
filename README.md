# StelaCRM

Sistema de CRM (Customer Relationship Management) SaaS Single-Tenant desenvolvido para pequenas empresas que precisam de uma solução simples, flexível e eficiente para gerenciar vendas e relacionamento com clientes.

## 📋 Sobre o Projeto

O StelaCRM é uma plataforma completa de gestão de relacionamento com clientes e vendas, projetada especificamente para atender às necessidades de pequenas empresas. Com uma interface intuitiva e funcionalidades robustas, o sistema permite gerenciar todo o ciclo de vendas desde a captação de leads até o fechamento de negócios.

## ✨ Funcionalidades Principais

### 📊 Funis e Pipelines
- Múltiplos funis de vendas personalizáveis
- Etapas editáveis e reordenáveis
- Visualizações flexíveis: Kanban, Lista e Tabela
- Cálculo automático de valores por etapa
- Drag-and-drop para movimentação entre etapas

### 👥 Gerenciamento de Leads e Oportunidades
- Cadastro e qualificação de leads
- Sistema de score para priorização
- Conversão de leads em oportunidades
- Status de negociação independente (Em andamento, Pausado, Vendido, Perdido)
- Histórico completo de mudanças

### 📥 Importação e Exportação
- Importação de leads via CSV/Excel
- Exportação de dados para análise
- Validação e tratamento de duplicados
- Mapeamento de colunas personalizado

### ✅ Tarefas e Anotações
- Criação de tarefas vinculadas a leads/oportunidades
- Anotações com histórico cronológico
- Visualização em lista e calendário
- Filtros e busca avançada

### 💼 Produtos, Serviços e Propostas
- Cadastro de produtos e serviços
- Criação de cotações e propostas
- Geração automática de PDF
- Envio de propostas por e-mail

### 🔐 Perfis e Permissões
- Sistema de perfis com permissões granulares
- Controle de acesso por recurso e ação
- Distribuição automática de leads
- Dashboard personalizado por perfil

### 📈 Dashboard e Relatórios
- Métricas personalizadas por usuário
- Visualização de pipeline e conversão
- Relatórios de performance
- Exportação em PDF e Excel

### 🔗 Integrações
- Integração com formulários web (webhook)
- Envio de e-mails automatizados
- Templates de e-mail personalizáveis
- Workflows automatizados

### ⚙️ Workflows Automatizados
- Regras baseadas em eventos
- Ações automáticas (tarefas, e-mails)
- Execução condicional
- Histórico de execuções

## 🎯 Público-Alvo

Pequenas empresas que buscam:
- Solução simples e intuitiva
- Flexibilidade para adaptar processos
- Controle completo sobre seus dados (single-tenant)
- Gestão eficiente do ciclo de vendas
- Relatórios e análises de performance

## 🚀 Começando

### Pré-requisitos

[Documentar tecnologias e versões necessárias quando disponível]

### Instalação

[Instruções de instalação serão adicionadas durante o desenvolvimento]

### Configuração Inicial

O sistema permite um onboarding rápido e intuitivo:
1. Configuração inicial da empresa
2. Criação do primeiro usuário administrador
3. Configuração do primeiro funil de vendas
4. Cadastro de produtos/serviços
5. Criação do primeiro lead

**Tempo estimado:** Menos de 10 minutos

## 📖 Documentação

A documentação completa do projeto está disponível na pasta `specs/`:
- **Especificação técnica**: `specs/1-stela-crm/spec.md`
- **Plano de desenvolvimento**: `specs/main/plan.md`
- **Checklist de requisitos**: `specs/1-stela-crm/checklists/requirements.md`

## 🏗️ Arquitetura

[Detalhes de arquitetura serão adicionados durante o desenvolvimento]

### Principais Entidades

- **Organization**: Empresa/organização
- **User**: Usuários do sistema
- **Profile**: Perfis de permissões
- **Funnel**: Funis de vendas
- **Lead**: Leads de vendas
- **Opportunity**: Oportunidades de venda
- **Task**: Tarefas
- **Note**: Anotações
- **Product/Service**: Produtos e serviços
- **Proposal**: Propostas comerciais
- **WorkflowRule**: Regras de workflow
- **ActivityLog**: Log de atividades

## 🔒 Segurança

- Autenticação baseada em email/senha
- Sistema de permissões granular
- Princípio do menor privilégio
- Log de auditoria para operações críticas
- Validação de dados em todas as entradas

## 🌍 Localização

- Idioma: Português Brasileiro (PT-BR)
- Moeda: Real Brasileiro (R$)
- Formato de data: DD/MM/YYYY
- Formato de hora: HH:mm
- Fuso horário configurável por organização

## 📊 Métricas de Performance

O sistema foi projetado para atender aos seguintes critérios de performance:
- 95% das requisições de API respondem em menos de 200ms
- Dashboard carrega em menos de 2 segundos
- Importação de até 1000 leads em menos de 30 segundos
- Visualização kanban suporta até 100 oportunidades por etapa

## 🤝 Contribuindo

[Diretrizes de contribuição serão adicionadas]

## 📝 Licença

[Informações de licença serão adicionadas]

## 📧 Contato

[Informações de contato serão adicionadas]

---

**Status do Projeto:** Em desenvolvimento

Para mais informações, consulte a [especificação completa](./specs/1-stela-crm/spec.md).