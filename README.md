# StelaCRM

> Plataforma SaaS Multitenant para gestão de vendas e relacionamento com clientes

[![Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)](https://github.com)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## 📋 Sobre o Projeto

O **StelaCRM** é uma solução SaaS multitenant desenvolvida especificamente para pequenas empresas que buscam obter sucesso nas vendas através de uma ferramenta simples, flexível e poderosa.

Nosso objetivo é proporcionar uma experiência de uso intuitiva, onde empresas possam configurar e começar a usar o sistema em minutos, sem necessidade de treinamento extensivo ou suporte constante.

### 🎯 Objetivos Principais

- **Simplicidade**: Interface intuitiva e fácil de usar
- **Flexibilidade**: Adapte o sistema às necessidades do seu negócio
- **Autonomia**: Self-service onboarding - configure sozinho em minutos
- **Performance**: Respostas rápidas, operações não-bloqueantes
- **Segurança**: Isolamento estrito de dados por cliente (Multitenancy)

## ✨ Principais Funcionalidades

### 🚀 Core (P1 - MVP)

- **✅ Funis de Vendas Customizáveis**
  - Múltiplos funis por tenant
  - Etapas editáveis, reordenáveis e personalizáveis
  - Visualização Kanban, Lista e Tabela

- **✅ Gerenciamento de Leads e Oportunidades**
  - Cadastro manual de leads
  - Qualificação com score e campos customizáveis
  - Conversão de leads em oportunidades
  - Histórico completo de movimentações

- **✅ Onboarding Self-Service**
  - Configuração inicial em menos de 10 minutos
  - Funil padrão pré-configurado
  - Tutorial interativo e tooltips contextuais

### 📊 Funcionalidades Essenciais (P2)

- **Importação/Exportação**: CSV/Excel para leads e oportunidades
- **Tarefas e Anotações**: Organize follow-ups e histórico de interações
- **Produtos e Propostas**: Cadastre produtos, gere cotações e envie PDFs
- **Perfis e Permissões**: Controle de acesso granular por usuário
- **Distribuição de Leads**: Regras automáticas de atribuição
- **Dashboard Personalizado**: Métricas relevantes por perfil

### 🔧 Funcionalidades Avançadas (P3)

- **Integração com Formulários**: Webhooks e código de embed
- **Email Marketing**: Templates, envio manual e automatizado
- **Workflows Automatizados**: Ações automáticas baseadas em eventos
- **Relatórios e Analytics**: Conversão, performance e pipeline

## 🏗️ Arquitetura e Princípios

O StelaCRM é desenvolvido seguindo princípios rigorosos definidos na [Constituição do Projeto](.specify/memory/constitution.md):

### Princípios Fundamentais

1. **Multitenancy**: Isolamento estrito de dados por cliente
2. **API First**: API é o produto principal, UI é consumidor
3. **Library First**: Funcionalidades complexas como bibliotecas independentes
4. **Cloud Native**: Arquitetura nativa de nuvem (12-Factor App)
5. **Non-Blocking**: Usuário nunca bloqueado esperando servidor
6. **Self-Service Onboarding**: Produto se vende e se explica sozinho
7. **Sensible Defaults**: Funciona "out of the box"
8. **Test First (TDD)**: Desenvolvimento orientado por testes (obrigatório)
9. **Least Privilege**: Menor privilégio em acessos e permissões
10. **SOLID**: Princípios de design orientado a objetos
11. **Clean Code & Clean Architecture**: Código limpo e arquitetura em camadas
12. **DRY, KISS, YAGNI**: Evitar duplicação, manter simples, implementar só o necessário
13. **Design Patterns**: Soluções comprovadas para problemas recorrentes

## 📁 Estrutura do Projeto

```
stela-crm/
├── .specify/                 # Especificações e templates do projeto
│   ├── memory/
│   │   └── constitution.md   # Constituição do projeto (princípios e regras)
│   └── templates/            # Templates para specs, planos e tarefas
├── specs/                    # Especificações de features
│   └── 1-stela-crm/
│       ├── spec.md           # Especificação completa do StelaCRM
│       └── checklists/       # Checklists de validação
└── README.md                 # Este arquivo
```

## 📚 Documentação

- **[Especificação Completa](specs/1-stela-crm/spec.md)**: Detalhamento de todas as funcionalidades, user stories e requisitos
- **[Constituição do Projeto](.specify/memory/constitution.md)**: Princípios, regras e padrões de desenvolvimento
- **[Checklist de Requisitos](specs/1-stela-crm/checklists/requirements.md)**: Validação da especificação

## 🚀 Status do Projeto

**Status Atual**: 📝 Especificação Completa

- ✅ Constituição do projeto definida
- ✅ Especificação completa criada (11 user stories, 56 requisitos funcionais)
- ✅ Checklist de qualidade validado
- 🔄 Planejamento técnico (próximo passo)
- ⏳ Implementação (a iniciar)

## 🎯 Critérios de Sucesso

O StelaCRM será considerado bem-sucedido quando:

- ✅ Usuários completam onboarding em menos de 10 minutos sem assistência
- ✅ Sistema responde 95% das requisições em menos de 200ms
- ✅ Dashboard carrega em menos de 2 segundos
- ✅ Isolamento total de dados entre tenants (0% de vazamento)
- ✅ 90% dos usuários conseguem criar e enviar proposta completa em menos de 5 minutos

## 🔐 Segurança

- **Multitenancy**: Isolamento estrito de dados por tenant
- **Autenticação**: Autenticação segura com tokens
- **Autorização**: Controle de acesso baseado em perfis e permissões granulares
- **Auditoria**: Log de operações críticas
- **Princípio do Menor Privilégio**: Usuários têm apenas permissões mínimas necessárias

## 🤝 Contribuindo

Este é um projeto em desenvolvimento ativo. Para contribuir:

1. Leia a [Constituição do Projeto](.specify/memory/constitution.md)
2. Revise a [Especificação](specs/1-stela-crm/spec.md)
3. Siga os princípios de Clean Code e TDD
4. Garanta que todos os testes passem antes de submeter alterações

## 📝 Licença

Este projeto está licenciado sob a [MIT License](LICENSE).

## 📞 Contato

Para dúvidas, sugestões ou colaborações, entre em contato através dos canais oficiais do projeto.

---

**Desenvolvido com ❤️ seguindo princípios de Clean Architecture e Best Practices**
