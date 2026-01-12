# Quickstart: StelaCRM

**Date**: 2026-01-08  
**Feature**: StelaCRM - Plataforma SaaS Single-Tenant  
**Purpose**: Guia rápido para começar o desenvolvimento do StelaCRM

## Visão Geral

StelaCRM é uma plataforma SaaS Single-Tenant desenvolvida em Go utilizando PocketBase como framework backend e Go Templates + HTMX para frontend.

## Pré-requisitos

### Ferramentas Necessárias

- **Go 1.21+**: [Instalar Go](https://go.dev/doc/install)
- **PocketBase SDK**: Será usado como biblioteca Go
- **Git**: Para controle de versão

### Verificar Instalação

```bash
go version        # Deve exibir Go 1.21 ou superior
git --version     # Git instalado
```

## Estrutura do Projeto

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
├── static/                      # Static assets
├── tests/                       # Tests
└── specs/                       # Specifications
```

## Configuração Inicial

### 1. Inicializar Projeto Go

```bash
# Criar módulo Go
go mod init github.com/stela-crm/stela-crm

# Adicionar dependência PocketBase
go get github.com/pocketbase/pocketbase
```

### 2. Estrutura Básica

Criar estrutura de diretórios:

```bash
mkdir -p cmd/stela-crm
mkdir -p internal/{models,services,handlers,hooks,migrations,lib}
mkdir -p templates/{layouts,pages,components}
mkdir -p static/{css,js,images}
mkdir -p tests/{integration,unit,fixtures}
```

### 3. Arquivo Principal (cmd/stela-crm/main.go)

```go
package main

import (
    "log"
    "github.com/pocketbase/pocketbase"
    "github.com/pocketbase/pocketbase/core"
)

func main() {
    app := pocketbase.New()

    // Registrar hooks customizados
    app.OnBeforeServe().Add(func(e *core.ServeEvent) error {
        // Registrar rotas customizadas aqui
        return nil
    })

    // Iniciar servidor
    if err := app.Start(); err != nil {
        log.Fatal(err)
    }
}
```

### 4. Configuração de Ambiente

Criar arquivo `.env` (exemplo):

```env
# PocketBase
PB_DATA_DIR=./pb_data
PB_ENCRYPTION_KEY=your-encryption-key-here

# Aplicação
PORT=8090
ENV=development
```

## Desenvolvimento

### Executar Aplicação

```bash
# Desenvolvimento
go run cmd/stela-crm/main.go serve

# Ou build e executar
go build -o bin/stela-crm cmd/stela-crm/main.go
./bin/stela-crm serve
```

### Acessar Admin UI

Após iniciar o servidor, acesse:
- **Admin UI**: http://localhost:8090/_/
- **API**: http://localhost:8090/api/

### Criar Primeiro Usuário Admin

1. Acesse http://localhost:8090/_/
2. Configure o primeiro administrador
3. Crie as collections necessárias via Admin UI ou migrations

## Collections (PocketBase)

### Collections Principais

As collections principais do sistema são:

1. **organizations** - Organização/Empresa (única por instalação)
2. **profiles** - Perfis de permissões
3. **users** - Usuários (collection padrão PocketBase + campos custom)
4. **funnels** - Funis de vendas
5. **funnel_stages** - Etapas de funis
6. **leads** - Leads de vendas
7. **opportunities** - Oportunidades de venda
8. **tasks** - Tarefas
9. **notes** - Anotações
10. **products** - Produtos e serviços
11. **proposals** - Propostas comerciais
12. **email_templates** - Templates de email
13. **workflow_rules** - Regras de workflow
14. **activity_log** - Log de atividades
15. **stage_history** - Histórico de mudanças de etapa
16. **status_history** - Histórico de mudanças de status

### Definir Collections via Migrations

Ver `internal/migrations/` para migrations que criam as collections.

## Testes

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

```go
// Exemplo: tests/integration/leads_test.go
package integration

import (
    "testing"
    "github.com/pocketbase/pocketbase"
)

func TestCreateLead(t *testing.T) {
    app := pocketbase.New()
    // Setup de teste
    // ...
}
```

## Próximos Passos

1. **Configurar Collections**: Criar migrations para definir collections no PocketBase
2. **Implementar Models**: Criar structs Go para as entidades (ver `data-model.md`)
3. **Implementar Services**: Criar lógica de negócio (ver `spec.md`)
4. **Implementar Handlers**: Criar rotas HTTP customizadas (quando necessário)
5. **Implementar Hooks**: Criar hooks do PocketBase para lógica automática
6. **Criar Templates**: Criar templates Go para frontend (ver `research.md`)
7. **Implementar Testes**: Criar testes unitários e de integração

## Recursos

### Documentação

- **Spec**: `specs/1-stela-crm/spec.md`
- **Data Model**: `specs/main/data-model.md`
- **Research**: `specs/main/research.md`
- **API Contract**: `specs/main/contracts/openapi.yaml`

### Referências Externas

- **PocketBase Docs**: https://pocketbase.io/docs/
- **PocketBase Go SDK**: https://github.com/pocketbase/pocketbase
- **Go Templates**: https://pkg.go.dev/html/template
- **HTMX**: https://htmx.org/

## Troubleshooting

### Problemas Comuns

1. **Porta já em uso**: Altere `PORT` no `.env` ou `pb_data/config.json`
2. **Erro de permissões**: Verifique permissões de escrita em `pb_data/`
3. **Migrations falhando**: Verifique logs em `pb_data/logs/`

### Logs

Logs do PocketBase são salvos em `pb_data/logs/` (quando configurado).

---

**Nota**: Este é um guia inicial. Consulte a documentação completa em `specs/` para detalhes de implementação.
