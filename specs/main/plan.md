# Implementation Plan: StelaCRM - Plataforma SaaS Single-Tenant

**Branch**: `main` | **Date**: 2026-01-08 | **Spec**: `specs/1-stela-crm/spec.md`
**Input**: Feature specification from `/specs/1-stela-crm/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

Plataforma SaaS Single-Tenant para pequenas empresas gerenciarem vendas com funis personalizáveis, leads/oportunidades, tarefas, propostas e relatórios. Desenvolvida em Go nativo utilizando PocketBase como framework backend, com frontend usando Go Templates + HTMX (server-side rendering com interatividade incremental).

## Technical Context

**Language/Version**: Go 1.21+ (nativo)  
**Primary Dependencies**: PocketBase (framework backend), Go Templates (html/template), HTMX 1.x (frontend interatividade)  
**Storage**: SQLite (embedded via PocketBase)  
**Testing**: Go testing package (testing, testify), integração com PocketBase test utilities (instâncias de teste)  
**Target Platform**: Web application (Linux/Windows/Mac server), navegadores modernos (Chrome, Firefox, Safari, Edge - últimas 2 versões)  
**Project Type**: web (backend + frontend)  
**Performance Goals**: 95% das requisições de API < 200ms (FR-055), dashboard < 2s para 95% dos casos (FR-041)  
**Constraints**: Single-tenant (uma instalação = uma empresa), <200ms p95 para APIs, suporte a navegadores modernos  
**Scale/Scope**: Pequenas empresas (até ~50 usuários por instalação), múltiplos funis, até 100 oportunidades por etapa com virtualização (FR-007c), sistema completo de CRM

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.* ✅ **PASSED**

**Re-avaliação após Phase 1 (Design)**: Todas as verificações mantidas. Design segue princípios da constituição. APIs documentadas (OpenAPI), modelo de dados definido, estrutura de projeto alinhada com princípios.

Verificar conformidade com os princípios da Constituição Stela CRM:

- ✅ **API First**: PocketBase fornece API REST nativa. APIs serão documentadas (OpenAPI) antes da UI. Frontend consumirá essas APIs.
- ✅ **Library First**: Funcionalidades complexas (workflows, relatórios, geração de PDF) serão módulos/bibliotecas independentes reutilizáveis.
- ✅ **Cloud Native**: PocketBase é single binary, compatível com containers. Sistema seguirá 12-Factor App (config via env vars, stateless, logs como streams).
- ✅ **Non-Blocking**: Operações longas (importação, geração de PDF, workflows) serão assíncronas. APIs REST do PocketBase respondem rapidamente. Meta: <200ms p95 (FR-055).
- ✅ **Self-Service Onboarding**: User Story 1 define onboarding <10min sem assistência. Sistema cria recursos padrão automaticamente.
- ✅ **Sensible Defaults**: Sistema cria funil padrão, configurações sensatas, permite começar imediatamente após configuração inicial.
- ✅ **TDD**: Testes serão escritos antes da implementação. Go testing package + testes de integração com PocketBase.
- ✅ **Least Privilege**: Sistema de perfis com permissões granulares por recurso e ação (FR-033). Usuários têm apenas permissões mínimas necessárias.
- ✅ **SOLID**: Arquitetura em camadas (models, services, handlers). Princípios SOLID aplicados no design Go.
- ✅ **Clean Code/Architecture**: Código limpo, arquitetura em camadas (entities, use cases, interfaces), regras de negócio independentes de framework.
- ✅ **DRY**: Lógica de negócio centralizada, reutilização de componentes, evitar duplicação.
- ✅ **KISS**: Solução mais simples possível. PocketBase simplifica backend, escolha de frontend ainda em avaliação (HTMX vs SvelteKit).
- ✅ **YAGNI**: Implementar apenas requisitos atuais do spec. Não adicionar funcionalidades "por precaução".
- ✅ **Design Patterns**: Patterns apropriados (Repository para dados, Service para lógica de negócio, Handler para HTTP) aplicados quando necessário.

**Violações Justificadas**: Nenhuma violação identificada. Design segue princípios da constituição.

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
cmd/
└── stela-crm/
    └── main.go              # Entry point, PocketBase app initialization

internal/
├── models/                  # Domain entities (Lead, Opportunity, Funnel, etc.)
├── services/                # Business logic (LeadService, WorkflowService, etc.)
├── handlers/                # HTTP handlers/routes (extendendo PocketBase)
├── hooks/                   # PocketBase event hooks
├── migrations/              # Database migrations
└── lib/                     # Reusable libraries (PDF generation, email, etc.)

templates/                   # Go Templates (se HTMX escolhido)
├── layouts/
├── pages/
└── components/

static/                      # Static assets (CSS, JS, images)
├── css/
├── js/
└── images/

tests/
├── integration/             # Integration tests
├── unit/                    # Unit tests
└── fixtures/                # Test data

# Se SvelteKit escolhido (alternativa):
frontend/                    # SvelteKit application (se escolhido)
├── src/
│   ├── lib/
│   ├── routes/
│   └── components/
└── tests/
```

**Structure Decision**: Web application com backend em Go/PocketBase. Frontend será decidido na pesquisa (Phase 0): Go Templates + HTMX (simples, server-side) ou SvelteKit (SPA moderna). Estrutura atual assume Go Templates + HTMX (mais simples, alinhado com KISS). Se SvelteKit for escolhido, frontend/ será adicionado como projeto separado consumindo APIs do PocketBase.

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |
