# Research: StelaCRM - Decisões Técnicas

**Date**: 2026-01-08  
**Feature**: StelaCRM - Plataforma SaaS Single-Tenant  
**Purpose**: Resolver questões técnicas pendentes (NEEDS CLARIFICATION) do Technical Context

## Decisões Técnicas

### 1. Frontend Framework: Go Templates + HTMX vs SvelteKit

**Questão**: Escolher entre Go Templates + HTMX (server-side) ou SvelteKit (SPA moderna) para o frontend.

**Pesquisa Realizada**:
- Comparação de Go Templates + HTMX vs SvelteKit para aplicações web
- Análise de requisitos do spec (interatividade, performance, complexidade)
- Avaliação de alinhamento com princípios da constituição (KISS, YAGNI)

**Decisão**: **Go Templates + HTMX**

**Rationale**:
1. **KISS (Keep It Simple, Stupid)**: Go Templates + HTMX é mais simples. Não requer build step, JavaScript complexo, ou gerenciamento de estado no cliente. Alinhado com princípio de simplicidade.
2. **YAGNI**: O spec não requer funcionalidades que demandem SPA completa (offline, PWA, etc.). A maioria das funcionalidades pode ser implementada com server-side rendering + interatividade incremental via HTMX.
3. **Performance**: Server-side rendering com Go Templates garante carregamento inicial rápido. HTMX adiciona interatividade sem overhead de bundle JavaScript grande.
4. **Manutenibilidade**: Equipe Go pode trabalhar no frontend sem necessidade de conhecimento profundo de JavaScript/SPA. Menos stack tecnológico = menos complexidade.
5. **Requisitos do Spec**: 
   - Kanban com drag-and-drop: HTMX + biblioteca JavaScript leve (ex: Sortable.js) pode atender
   - Visualizações (Kanban, Lista, Tabela): Server-side rendering com templates diferentes
   - Filtros e busca: Forms HTML + HTMX para atualização parcial
   - Dashboard: Server-side rendering de métricas, HTMX para atualizações
6. **PocketBase Integration**: PocketBase fornece API REST. HTMX pode fazer requisições HTTP diretamente. Go Templates pode renderizar dados do PocketBase no servidor.

**Alternativas Consideradas**:
- **SvelteKit**: Oferece SPA moderna, SSR, performance excelente. **Rejeitado porque**: Adiciona complexidade desnecessária (build step, estado no cliente, JavaScript mais complexo). Violaria KISS e YAGNI para requisitos atuais.
- **React/Vue/Angular**: Frameworks SPA tradicionais. **Rejeitado porque**: Similar a SvelteKit, adiciona complexidade sem necessidade.

**Implementação**:
- Usar `html/template` (Go stdlib) para renderização server-side
- HTMX 1.x para interatividade (atualizações parciais, forms, AJAX)
- Bibliotecas JavaScript mínimas quando necessário (ex: Sortable.js para drag-and-drop do kanban)
- CSS framework simples (ex: Tailwind CSS ou custom CSS)

---

### 2. Testing: Integração com PocketBase

**Questão**: Como testar aplicação Go que estende PocketBase (hooks, custom routes, handlers)?

**Pesquisa Realizada**:
- Documentação de PocketBase sobre testing
- Melhores práticas para testar aplicações Go com PocketBase
- Estrutura de testes (unit, integration)

**Decisão**: **Go testing package + PocketBase test utilities + testify**

**Rationale**:
1. **Go testing package**: Padrão da linguagem, suporta testes unitários e de integração.
2. **testify**: Biblioteca popular para assertions (`assert`, `require`) e mocking (`mockery`) quando necessário.
3. **PocketBase Testing**: PocketBase permite criar instâncias de teste com banco SQLite em memória ou arquivo temporário.
4. **Estrutura de Testes**:
   - **Unit Tests**: Testar lógica de negócio isolada (services, models, validators). Mockar dependências quando necessário.
   - **Integration Tests**: Testar integração com PocketBase (hooks, custom routes, handlers). Usar instância PocketBase de teste.
5. **Test Utilities**:
   - Criar helper functions para setup/teardown de instância PocketBase de teste
   - Fixtures para dados de teste (leads, funis, usuários)
   - Test helpers para autenticação e criação de usuários de teste

**Alternativas Consideradas**:
- **Testcontainers**: Usar containers Docker para testes. **Rejeitado porque**: Overhead desnecessário. SQLite em memória/arquivo temporário é suficiente para testes.
- **SQL Mock**: Mockar SQL diretamente. **Rejeitado porque**: PocketBase abstrai SQLite. Testar através da API do PocketBase é mais apropriado.

**Implementação**:
```go
// Exemplo de estrutura de testes
tests/
├── integration/
│   ├── hooks_test.go
│   ├── routes_test.go
│   └── helpers.go      // Setup PocketBase de teste
├── unit/
│   ├── services/
│   ├── models/
│   └── validators/
└── fixtures/
    └── data.go         // Dados de teste
```

---

## Resumo das Decisões

| Aspecto | Decisão | Justificativa Principal |
|---------|---------|------------------------|
| Frontend | Go Templates + HTMX | KISS, YAGNI, simplicidade |
| Testing | Go testing + testify | Padrão Go, integração com PocketBase |
| Backend Framework | PocketBase | Definido pelo usuário |
| Language | Go 1.21+ | Definido pelo usuário |
| Storage | SQLite (embedded) | PocketBase padrão |

## Próximos Passos

1. ✅ Frontend framework decidido → Go Templates + HTMX
2. ✅ Testing strategy decidida → Go testing + testify
3. → Phase 1: Gerar data-model.md
4. → Phase 1: Gerar contracts/ (OpenAPI)
5. → Phase 1: Gerar quickstart.md
