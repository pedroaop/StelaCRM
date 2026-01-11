<!--
Sync Impact Report:
Version: 2.0.0 (Removed Single-Tenant principle)
Date: 2026-01-08
Changes:
- Removed principle I. Single-Tenant (system will not be Single-Tenant)
- Renumbered all remaining principles (II->I, III->II, ..., XV->XIV)
- Reduced from 15 to 14 core principles
- Updated plan-template.md to remove Single-Tenant check

Templates Status:
✅ plan-template.md - Updated to remove Single-Tenant check
✅ spec-template.md - Compatible with principles (no changes needed)
✅ tasks-template.md - Compatible with principles (no changes needed)
⏭️ commands/*.md - Directory not created yet, will be created when needed

Follow-up TODOs:
- Review and update existing plan.md files to remove Single-Tenant references
- Review README.md and other docs for Single-Tenant references
-->

# Stela CRM Constitution

## Core Principles

### I. API First
A API é o produto principal; a interface gráfica é apenas um consumidor. Todas as funcionalidades devem ser expostas primeiro via API REST/GraphQL com documentação completa (OpenAPI/Swagger). Interfaces gráficas são consumidores opcionais da mesma API que outros clientes utilizam. A API deve ser estável, versionada e documentada antes do desenvolvimento de qualquer interface.

**Rationale**: Permite múltiplos clientes (web, mobile, integrações), facilita testes, e garante que a lógica de negócio esteja centralizada na API.

### II. Library First
Funcionalidades complexas devem ser desenvolvidas como bibliotecas independentes. Cada biblioteca deve ser auto-contida, independentemente testável, com propósito claro e documentação própria. Bibliotecas devem poder ser reutilizadas em diferentes contextos sem dependências desnecessárias. Evite bibliotecas apenas organizacionais sem propósito funcional claro.

**Rationale**: Facilita testes, reutilização, manutenção e permite evolução independente de componentes.

### III. Cloud Native
A arquitetura deve ser nativa de nuvem. Utilize princípios de 12-Factor App: configuração via variáveis de ambiente, stateless quando possível, logs como streams, processos descartáveis, deploy contínuo. Compatível com containers (Docker/Kubernetes), escalável horizontalmente, resiliente a falhas. Utilize serviços gerenciados de nuvem quando apropriado.

**Rationale**: Permite escalabilidade, alta disponibilidade, deploy facilitado e redução de custos operacionais.

### IV. Non-Blocking Architecture
O usuário nunca deve ficar bloqueado esperando o servidor. Operações longas devem ser assíncronas (jobs, filas, webhooks). APIs devem retornar rapidamente (<200ms para 95% das requisições). Use padrões como polling, callbacks ou websockets para feedback de operações assíncronas.

**Rationale**: Experiência do usuário degrada drasticamente com esperas. Operações assíncronas permitem que o sistema escale e o usuário continue trabalhando.

### V. Self-Service Onboarding
O produto deve se vender e se explicar sozinho. O processo de onboarding deve ser intuitivo, guiado, e permitir que usuários comecem a usar o sistema sem intervenção manual. Documentação inline, tutoriais interativos, e configuração automática de padrões sensatos devem ser priorizados.

**Rationale**: Reduz custos de suporte, acelera adoção, e permite escalar o negócio sem escalar o suporte proporcionalmente.

### VI. Sensible Defaults
O sistema deve funcionar "out of the box" com padrões sensatos para a maioria dos usuários. Configurações padrão devem ser escolhidas para otimizar a experiência da maioria (80/20). Configurações avançadas devem estar disponíveis, mas não devem ser necessárias para uso básico.

**Rationale**: Reduz fricção na adoção, acelera time-to-value, e permite que usuários comecem a trabalhar imediatamente.

### VII. Test First (TDD) - NON-NEGOTIABLE
TDD é obrigatório para todas as funcionalidades: escreva o teste → usuário aprova → teste falha → então implemente. O ciclo Red-Green-Refactor deve ser estritamente seguido. Testes são parte do requisito, não uma opção posterior. Cobertura mínima de testes unitários e integração deve ser mantida.

**Rationale**: TDD garante qualidade, reduz bugs, facilita refatoração e serve como documentação viva do comportamento esperado.

### VIII. Least Privilege
Princípio do menor privilégio em acessos e permissões. Usuários e serviços devem ter apenas as permissões mínimas necessárias para executar suas funções. Permissões devem ser granulares, auditadas, e revisadas regularmente. Evite permissões amplas (admin, root) a menos que absolutamente necessário.

**Rationale**: Minimiza superfície de ataque, reduz impacto de comprometimentos, e facilita compliance com regulamentações de segurança.

### IX. SOLID Principles
Adesão estrita aos princípios SOLID:
- **SRP (Single Responsibility Principle)**: Cada classe/módulo deve ter uma única razão para mudar
- **OCP (Open/Closed Principle)**: Aberto para extensão, fechado para modificação
- **LSP (Liskov Substitution Principle)**: Subclasses devem ser substituíveis por suas classes base
- **ISP (Interface Segregation Principle)**: Interfaces específicas são melhores que uma interface geral
- **DIP (Dependency Inversion Principle)**: Dependa de abstrações, não de implementações concretas

**Rationale**: SOLID garante código manutenível, extensível, e facilita evolução do sistema ao longo do tempo.

### X. Clean Code & Clean Architecture
Código deve ser limpo, legível, expressivo e autodocumentado. Use nomes descritivos, funções pequenas, comentários apenas quando necessário (código deve ser auto-explicativo). Siga Clean Architecture: separação de camadas (entities, use cases, interfaces), dependências apontam para dentro, regras de negócio independentes de frameworks.

**Rationale**: Código limpo reduz custo de manutenção, facilita onboarding de novos desenvolvedores, e reduz bugs.

### XI. DRY (Don't Repeat Yourself)
Evite duplicação de conhecimento e lógica. Se a mesma lógica aparece em múltiplos lugares, extraia para função/módulo reutilizável. Código duplicado é dívida técnica e aumenta risco de inconsistências.

**Rationale**: Duplicação aumenta manutenção, risco de bugs, e dificulta evolução. Uma mudança deve ocorrer em um único lugar.

### XII. KISS (Keep It Simple, Stupid)
A simplicidade é o grau máximo de sofisticação. Escolha a solução mais simples que resolva o problema. Evite over-engineering. Prefira código direto e legível sobre soluções "inteligentes" que poucos conseguem entender.

**Rationale**: Soluções simples são mais fáceis de entender, manter, depurar e evoluir. Complexidade desnecessária é dívida técnica.

### XIII. YAGNI (You Ain't Gonna Need It)
Implemente apenas o necessário para os requisitos atuais. Não adicione funcionalidades "por precaução" ou porque "pode ser útil no futuro". Quando o requisito real surgir, implemente. Arquitetura deve ser preparada para extensão, mas não deve incluir código não utilizado.

**Rationale**: Funcionalidades não utilizadas adicionam complexidade, custo de manutenção, e podem nunca ser necessárias. Evite desperdício.

### XIV. Design Patterns
Use soluções comprovadas para problemas recorrentes. Aplique Design Patterns apropriados (Factory, Strategy, Repository, Observer, etc.) quando resolverem problemas conhecidos. Não force padrões onde não se aplicam. Conheça os padrões, mas escolha-os baseado em necessidade real.

**Rationale**: Patterns são soluções testadas para problemas comuns. Usá-los corretamente acelera desenvolvimento e facilita comunicação entre desenvolvedores.

## Development Workflow

### Code Review Requirements
Todas as mudanças devem passar por code review. Revisores devem verificar:
- Conformidade com esta constituição
- Adequação de testes (TDD aplicado)
- Qualidade do código (Clean Code, SOLID)
- Segurança (Least Privilege, validações adequadas)

### Quality Gates
- Testes devem passar antes de merge
- Cobertura mínima de testes deve ser mantida (métrica específica a definir)
- Linting/formatting deve passar
- Verificação de segurança (dependências, vulnerabilidades conhecidas)

### Documentation
- APIs devem ser documentadas (OpenAPI/Swagger)
- Bibliotecas devem ter README com exemplos
- Decisões arquiteturais significativas devem ser documentadas (ADR - Architecture Decision Records)

## Governance

Esta constituição governa todas as decisões técnicas do projeto Stela CRM. Ela supera práticas locais e preferências individuais.

### Amendment Procedure
Alterações nesta constituição requerem:
1. Proposta documentada com justificativa
2. Revisão e aprovação da equipe
3. Atualização de todos os templates e documentação relacionada
4. Versionamento semântico (MAJOR.MINOR.PATCH)

### Versioning Policy
- **MAJOR**: Mudanças incompatíveis, remoção de princípios, redefinições fundamentais
- **MINOR**: Novos princípios adicionados, seções expandidas materialmente
- **PATCH**: Clarificações, correções de texto, refinamentos não-semânticos

### Compliance Review
Todos os PRs/revisões devem verificar conformidade com esta constituição. Complexidade adicional deve ser justificada. Violações devem ser documentadas e remediadas ou justificadas excepcionalmente.

**Version**: 2.0.0 | **Ratified**: 2025-01-27 | **Last Amended**: 2026-01-08
