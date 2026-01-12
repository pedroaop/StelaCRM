# StelaCRM

Sistema de CRM (Customer Relationship Management) SaaS Single-Tenant desenvolvido em Go para pequenas empresas que precisam de uma solução simples, flexível e eficiente para gerenciar vendas e relacionamento com clientes.

## 📋 Sobre o Projeto

O StelaCRM é uma plataforma completa de gestão de relacionamento com clientes e vendas, projetada especificamente para atender às necessidades de pequenas empresas. Com uma interface intuitiva e funcionalidades robustas, o sistema permite gerenciar todo o ciclo de vendas desde a captação de leads até o fechamento de negócios.

### Características Principais

- **Single-Tenant**: Uma instalação = uma empresa, garantindo controle total sobre os dados
- **Desenvolvido em Go**: Backend nativo em Go utilizando PocketBase como framework
- **Frontend Moderno**: Go Templates + HTMX para interface responsiva e interativa
- **Self-Service Onboarding**: Configuração inicial em menos de 10 minutos
- **Performance**: 95% das requisições de API < 200ms, dashboard < 2s

## 🚀 Tecnologias

### Backend
- **Go 1.21+**: Linguagem principal
- **PocketBase**: Framework backend com API REST nativa
- **SQLite**: Banco de dados embutido (via PocketBase)

### Frontend
- **Go Templates** (`html/template`): Renderização server-side
- **HTMX 1.x**: Interatividade incremental sem JavaScript complexo

### Testes
- **Go testing package**: Framework de testes padrão
- **testify**: Assertions e mocking

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

## 📦 Pré-requisitos

- **Go 1.21 ou superior**: [Instalar Go](https://go.dev/doc/install)
- **Git**: Para controle de versão

### Verificar Instalação

```bash
go version        # Deve exibir Go 1.21 ou superior
git --version     # Git instalado
```

## 🛠️ Instalação

### 1. Clonar o Repositório

```bash
git clone https://github.com/seu-usuario/stela-crm.git
cd stela-crm
```

### 2. Inicializar Módulo Go

```bash
go mod init github.com/stela-crm/stela-crm
go get github.com/pocketbase/pocketbase
```

### 3. Configurar Variáveis de Ambiente

Criar arquivo `.env` (opcional, pode usar configuração padrão):

```env
# PocketBase
PB_DATA_DIR=./pb_data
PB_ENCRYPTION_KEY=your-encryption-key-here

# Aplicação
PORT=8090
ENV=development
```

### 4. Executar Aplicação

```bash
# Desenvolvimento
go run cmd/stela-crm/main.go serve

# Ou build e executar
go build -o bin/stela-crm cmd/stela-crm/main.go
./bin/stela-crm serve
```

### 5. Acessar Sistema

- **Admin UI**: http://localhost:8090/_/
- **API**: http://localhost:8090/api/
- **Aplicação**: http://localhost:8090/

## 📁 Estrutura do Projeto

```
stela-crm/
├── cmd/
│   └── stela-crm/
│       └── main.go              # Entry point
├── internal/
│   ├── models/                  # Domain entities
│   ├── services/                # Business logic
│   ├── handlers/                # HTTP handlers
│   ├── hooks/                   # PocketBase hooks
│   ├── migrations/              # Database migrations
│   └── lib/                     # Reusable libraries
├── templates/                   # Go Templates
│   ├── layouts/
│   ├── pages/
│   └── components/
├── static/                      # Static assets
│   ├── css/
│   ├── js/
│   └── images/
├── tests/                       # Tests
│   ├── integration/
│   ├── unit/
│   └── fixtures/
├── specs/                       # Specifications
│   ├── 1-stela-crm/
│   │   ├── spec.md
│   │   └── checklists/
│   └── main/
│       ├── plan.md
│       ├── research.md
│       ├── data-model.md
│       ├── quickstart.md
│       └── contracts/
└── README.md
```

## 🚀 Começando

### Configuração Inicial

O sistema permite um onboarding rápido e intuitivo:

1. Configuração inicial da empresa
2. Criação do primeiro usuário administrador
3. Configuração do primeiro funil de vendas
4. Cadastro de produtos/serviços
5. Criação do primeiro lead

**Tempo estimado:** Menos de 10 minutos

Consulte o [Quickstart Guide](specs/main/quickstart.md) para instruções detalhadas.

## 📖 Documentação

### Documentação Principal

- **Especificação Técnica**: [`specs/1-stela-crm/spec.md`](specs/1-stela-crm/spec.md)
- **Plano de Implementação**: [`specs/main/plan.md`](specs/main/plan.md)
- **Modelo de Dados**: [`specs/main/data-model.md`](specs/main/data-model.md)
- **Quickstart Guide**: [`specs/main/quickstart.md`](specs/main/quickstart.md)
- **Decisões Técnicas**: [`specs/main/research.md`](specs/main/research.md)

### APIs

- **OpenAPI/Swagger**: [`specs/main/contracts/openapi.yaml`](specs/main/contracts/openapi.yaml)
- **API REST**: Documentação completa via PocketBase Admin UI

### Referências Externas

- **PocketBase Docs**: https://pocketbase.io/docs/
- **PocketBase Go SDK**: https://github.com/pocketbase/pocketbase
- **Go Templates**: https://pkg.go.dev/html/template
- **HTMX**: https://htmx.org/

## 🧪 Testes

### Executar Testes

```bash
# Todos os testes
go test ./...

# Testes unitários
go test ./tests/unit/...

# Testes de integração
go test ./tests/integration/...
```

### Estrutura de Testes

- **Unit Tests**: Lógica de negócio isolada (services, models, validators)
- **Integration Tests**: Integração com PocketBase (hooks, routes, handlers)

## 🏗️ Arquitetura

### Principais Entidades

- **Organization**: Empresa/organização (única por instalação)
- **User**: Usuários do sistema
- **Profile**: Perfis de permissões
- **Funnel**: Funis de vendas
- **FunnelStage**: Etapas de funis
- **Lead**: Leads de vendas
- **Opportunity**: Oportunidades de venda
- **Task**: Tarefas
- **Note**: Anotações
- **Product**: Produtos e serviços
- **Proposal**: Propostas comerciais
- **EmailTemplate**: Templates de email
- **WorkflowRule**: Regras de workflow
- **ActivityLog**: Log de atividades

Para detalhes completos, consulte o [Data Model](specs/main/data-model.md).

### Princípios Arquiteturais

O projeto segue os princípios definidos na [Constituição Stela CRM](.specify/memory/constitution.md):

- **API First**: APIs documentadas antes da UI
- **Library First**: Funcionalidades complexas como bibliotecas independentes
- **Cloud Native**: Compatível com 12-Factor App
- **Non-Blocking**: Operações longas assíncronas, APIs < 200ms p95
- **Self-Service Onboarding**: Configuração sem assistência
- **Sensible Defaults**: Funciona "out of the box"
- **TDD**: Testes escritos antes da implementação
- **Clean Code/Architecture**: Código limpo e arquitetura em camadas

## 🤝 Contribuindo

Contribuições são bem-vindas! Por favor:

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📝 Licença

[Adicionar licença quando definida]

## 📧 Contato

[Adicionar informações de contato]

---

**Status do Projeto**: 🚧 Em Desenvolvimento

**Versão**: 0.1.0 (Planejamento)

**Última Atualização**: Janeiro 2026
