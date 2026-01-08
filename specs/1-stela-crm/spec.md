# Feature Specification: StelaCRM - Plataforma SaaS Multitenant

**Feature Branch**: `1-stela-crm`  
**Created**: 2026-01-08  
**Status**: Draft  
**Input**: User description: "Saas Multitenant para pequenas empresas que precisam de obter sucesso nas evendas e que querem de uma solução simples e flexível com Vários Funis, Etapas de funil editáveis, Visualização de funis/pipelines (kanban e outros), Qualificação de leads, Importar/Exportar leads e oportunidades, Cadastro de tarefas, Cadastro de anotações, Cadastro de produtos/serviços, Cotação/proposta, Perfis e Permissões de Usuários, Dasboard por usuário, Distribuição de leads por usuário, Integrar com Formulários, Envio de E-mail, Modelo de E-mail, Envio de propostas PDF, Workflow de atividades (tarefas, e-mail), Relatórios e dashboard"

## Clarifications

### Session 2026-01-08

- Q: Como o sistema lida com leads duplicados na importação? → A: Sistema detecta duplicados por email OU telefone e oferece opções ao usuário: mesclar dados, pular registro duplicado, ou criar mesmo assim
- Q: O que acontece quando uma etapa de funil é deletada mas possui oportunidades? → A: Sistema impede deleção e exige que usuário mova/realoque manualmente todas as oportunidades antes de deletar a etapa
- Q: O que acontece quando workflow dispara ação mas usuário alvo não existe mais? → A: Sistema valida usuário antes de executar ação, pula ação se usuário inválido, registra em log e notifica administrador do tenant
- Q: Como o sistema lida com permissões quando um usuário muda de perfil? → A: Sistema aplica novo perfil imediatamente, mantém histórico de ações com perfil anterior para auditoria, e registra mudança de perfil em log de auditoria

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Onboarding e Configuração Inicial (Priority: P1)

Como empresário de uma pequena empresa, quero configurar minha conta no StelaCRM de forma simples e intuitiva, para começar a gerenciar minhas vendas rapidamente sem necessidade de treinamento extensivo.

**Why this priority**: O onboarding é a primeira experiência do usuário. Se não conseguir configurar e começar a usar rapidamente, não haverá adoção. Além disso, a constituição exige Self-Service Onboarding, então este é fundamental.

**Independent Test**: Um novo tenant pode criar conta, configurar seu primeiro funil de vendas com etapas padrão, adicionar um produto/serviço, e criar seu primeiro lead, tudo em menos de 10 minutos, sem assistência externa.

**Acceptance Scenarios**:

1. **Given** um novo usuário acessa o sistema pela primeira vez, **When** cria uma conta de tenant, **Then** o sistema configura automaticamente um funil de vendas padrão com etapas comuns (Lead, Contato, Proposta, Negociação, Fechado) e apresenta um guia de boas-vindas interativo
2. **Given** um tenant recém-criado, **When** o usuário acessa o dashboard pela primeira vez, **Then** o sistema mostra um tutorial interativo explicando as funcionalidades principais
3. **Given** um tenant configurado, **When** o usuário navega pelas telas principais, **Then** o sistema oferece tooltips contextuais e exemplos para facilitar o aprendizado

---

### User Story 2 - Gerenciamento de Funis e Pipelines (Priority: P1)

Como vendedor ou gestor de vendas, quero criar e personalizar funis de vendas com etapas editáveis, para adaptar o processo às necessidades específicas do meu negócio.

**Why this priority**: O funil é o coração de um CRM. Sem a capacidade de gerenciar funis customizados, o sistema não atende ao propósito básico de gerenciar o processo de vendas.

**Independent Test**: Um usuário pode criar um novo funil de vendas, definir etapas customizadas com nomes e ordens específicas, e visualizar leads/oportunidades neste funil em formato kanban, tudo funcionando independentemente de outras funcionalidades.

**Acceptance Scenarios**:

1. **Given** um usuário autenticado, **When** cria um novo funil de vendas, **Then** o sistema permite definir nome, descrição, e etapas customizadas com ordem definida pelo usuário
2. **Given** um funil existente, **When** o usuário edita as etapas (adiciona, renomeia, reordena), **Then** as alterações são salvas e leads/oportunidades existentes são preservados
2. **Given** um funil com etapa contendo oportunidades, **When** o usuário tenta deletar a etapa, **Then** o sistema impede a deleção, informa quantas oportunidades estão nessa etapa, e exige que todas sejam movidas manualmente antes de permitir a deleção
3. **Given** um funil com leads/oportunidades, **When** o usuário visualiza em formato kanban, **Then** cada card representa uma oportunidade mostrando informações essenciais (nome, valor, data de atualização) e pode ser arrastado entre colunas para mudar de etapa
4. **Given** múltiplos funis criados, **When** o usuário navega entre eles, **Then** o sistema mantém o estado de visualização e filtros de cada funil independentemente

---

### User Story 3 - Gerenciamento de Leads e Oportunidades (Priority: P1)

Como vendedor, quero cadastrar, qualificar e mover leads através das etapas do funil, para acompanhar o progresso das vendas e garantir que nenhuma oportunidade seja perdida.

**Why this priority**: Leads e oportunidades são a matéria-prima do CRM. Sem a capacidade de criar, visualizar e gerenciar leads, o sistema não tem valor para o usuário.

**Independent Test**: Um usuário pode criar um novo lead manualmente, qualificá-lo atribuindo informações e score, converter em oportunidade atribuindo valor e produtos, e movê-lo entre etapas do funil, completando todo o ciclo básico de vendas.

**Acceptance Scenarios**:

1. **Given** um usuário em qualquer etapa do funil, **When** cria um novo lead manualmente, **Then** o sistema permite preencher informações básicas (nome, empresa, email, telefone, origem) e o lead é criado na etapa inicial do funil selecionado
2. **Given** um lead existente, **When** o usuário qualifica o lead adicionando informações de qualificação (score, interesse, orçamento, timeline), **Then** essas informações são salvas e o lead pode ser filtrado e ordenado por critérios de qualificação
3. **Given** um lead qualificado, **When** o usuário converte em oportunidade atribuindo valor estimado e produtos/serviços, **Then** o lead se torna uma oportunidade rastreável com valor potencial
4. **Given** uma oportunidade em qualquer etapa, **When** o usuário move manualmente entre etapas (via drag-and-drop ou seleção), **Then** a oportunidade é atualizada com nova etapa, data de atualização é registrada, e histórico é mantido

---

### User Story 4 - Importação e Exportação de Dados (Priority: P2)

Como gestor ou administrador, quero importar leads e oportunidades de planilhas ou exportar dados para análise externa, para migrar dados existentes e gerar relatórios customizados.

**Why this priority**: Empresas já possuem dados em planilhas. Sem capacidade de importar, a adoção é bloqueada. Exportação é essencial para análise e backup.

**Independent Test**: Um usuário pode fazer upload de um arquivo CSV/Excel com dados de leads, o sistema valida e importa os dados criando leads no funil correto, e o mesmo usuário pode exportar todos os leads/oportunidades em formato CSV/Excel.

**Acceptance Scenarios**:

1. **Given** um usuário com permissão de importação, **When** faz upload de arquivo CSV/Excel com colunas mapeadas para campos do sistema (nome, email, telefone, etc.), **Then** o sistema valida os dados, mostra preview das linhas a importar, permite correções, e importa criando leads/oportunidades no funil selecionado
2. **Given** dados importados, **When** ocorrem erros de validação (emails inválidos, campos obrigatórios faltando) ou duplicados são detectados (email ou telefone já existe), **Then** o sistema reporta erros específicos por linha, para duplicados oferece opções (mesclar/pular/criar), permite correção antes da importação, e importa apenas registros válidos conforme decisões do usuário
3. **Given** leads e oportunidades no sistema, **When** o usuário solicita exportação, **Then** o sistema gera arquivo CSV/Excel com todos os dados filtrados (por funil, etapa, período, etc.) incluindo histórico relevante

---

### User Story 5 - Tarefas e Anotações (Priority: P2)

Como vendedor, quero cadastrar tarefas e anotações relacionadas a leads/oportunidades, para manter histórico de interações e garantir follow-up adequado.

**Why this priority**: Tarefas e anotações são essenciais para gestão do relacionamento com clientes. Sem isso, o CRM não mantém o contexto necessário para vendas efetivas.

**Independent Test**: Um usuário pode criar uma tarefa associada a uma oportunidade (ex: "Ligar cliente em 2 dias"), criar anotações de reuniões ou ligações, e visualizar histórico completo de tarefas e anotações de uma oportunidade.

**Acceptance Scenarios**:

1. **Given** uma oportunidade aberta, **When** o usuário cria uma tarefa (tipo: ligar, email, reunião, etc.) com data/hora e descrição, **Then** a tarefa é salva vinculada à oportunidade e aparece no dashboard do usuário na data agendada
2. **Given** uma oportunidade ou lead, **When** o usuário adiciona uma anotação (texto livre, possivelmente com data/hora automática), **Then** a anotação é salva no histórico da oportunidade/lead e fica visível na timeline
3. **Given** tarefas agendadas, **When** o usuário visualiza seu dashboard, **Then** o sistema mostra lista de tarefas pendentes ordenadas por prioridade/data, permitindo marcar como concluída

---

### User Story 6 - Produtos, Serviços e Propostas (Priority: P2)

Como vendedor, quero cadastrar produtos/serviços, criar cotações/propostas para oportunidades, e enviar propostas em PDF, para formalizar ofertas comerciais aos clientes.

**Why this priority**: Propostas comerciais são parte essencial do processo de vendas. Sem capacidade de gerar e enviar propostas, o processo fica incompleto.

**Independent Test**: Um usuário pode cadastrar produtos/serviços com preços, criar uma proposta associada a uma oportunidade selecionando produtos e quantidades, gerar PDF da proposta, e enviar por email ao cliente.

**Acceptance Scenarios**:

1. **Given** um usuário autenticado, **When** cadastra um produto/serviço (nome, descrição, preço unitário, categoria), **Then** o produto fica disponível para seleção em propostas e pode ser editado ou desativado
2. **Given** uma oportunidade e produtos cadastrados, **When** o usuário cria uma proposta selecionando produtos, quantidades, aplicando descontos se necessário, **Then** o sistema calcula total automaticamente e salva a proposta vinculada à oportunidade
3. **Given** uma proposta criada, **When** o usuário solicita geração de PDF, **Then** o sistema gera documento PDF formatado com informações da proposta, produtos, valores, e dados da empresa/cliente
4. **Given** uma proposta com PDF gerado, **When** o usuário envia por email, **Then** o sistema envia email ao cliente com PDF anexado e registra o envio no histórico da oportunidade

---

### User Story 7 - Perfis, Permissões e Distribuição de Leads (Priority: P2)

Como administrador da empresa, quero definir perfis de usuário com permissões específicas e configurar distribuição automática de leads, para controlar acesso e otimizar atribuição de oportunidades.

**Why this priority**: Multitenancy requer isolamento e controle de acesso. Sem permissões adequadas, o sistema não é seguro. Distribuição de leads otimiza operação de vendas.

**Independent Test**: Um administrador pode criar perfis de usuário (vendedor, gerente, admin) com permissões específicas, criar usuários atribuindo perfis, configurar regras de distribuição automática de leads, e o sistema distribui novos leads conforme regras definidas.

**Acceptance Scenarios**:

1. **Given** um administrador do tenant, **When** cria um perfil de usuário definindo permissões (visualizar, editar, deletar leads; acessar relatórios; gerenciar usuários, etc.), **Then** o perfil é salvo e pode ser atribuído a múltiplos usuários
2. **Given** perfis criados, **When** o administrador cria um novo usuário atribuindo email, nome, perfil, **Then** o usuário recebe convite por email e ao aceitar tem acesso conforme permissões do perfil
3. **Given** usuários vendedores cadastrados, **When** o administrador configura distribuição automática de leads (round-robin, por região, por origem, etc.), **Then** novos leads são automaticamente atribuídos conforme regras definidas
4. **Given** um lead distribuído a um vendedor, **When** o vendedor visualiza seu dashboard, **Then** o lead aparece em sua lista de leads atribuídos e pode ser gerenciado

---

### User Story 8 - Dashboard Personalizado (Priority: P2)

Como usuário do sistema, quero visualizar um dashboard personalizado com informações relevantes às minhas responsabilidades, para ter visibilidade rápida do status de vendas e ações pendentes.

**Why this priority**: Dashboard é essencial para produtividade. Usuários precisam ver rapidamente o que é importante para eles.

**Independent Test**: Um usuário pode acessar seu dashboard e visualizar métricas relevantes (leads atribuídos, oportunidades em cada etapa, valor total em pipeline, tarefas pendentes, conversão por funil), com filtros por período e visualizações gráficas.

**Acceptance Scenarios**:

1. **Given** um usuário autenticado, **When** acessa o dashboard, **Then** o sistema mostra widgets personalizados baseados no perfil (vendedor: suas oportunidades, tarefas; gerente: visão da equipe; admin: visão completa do tenant)
2. **Given** um dashboard configurado, **When** o usuário filtra por período (hoje, semana, mês, customizado), **Then** todas as métricas e gráficos são atualizados para refletir o período selecionado
3. **Given** dados no sistema, **When** o dashboard é carregado, **Then** as métricas são calculadas e exibidas rapidamente (< 2 segundos) com indicadores visuais claros (gráficos, cards, listas)

---

### User Story 9 - Integração com Formulários e Envio de Email (Priority: P3)

Como gestor de marketing ou vendas, quero integrar o CRM com formulários web e enviar emails automatizados, para capturar leads automaticamente e manter comunicação com clientes.

**Why this priority**: Automação reduz trabalho manual e melhora captação de leads. Email é canal essencial de comunicação.

**Independent Test**: Um usuário pode criar um webhook/formulário de captura, configurar que submissões criem leads automaticamente, criar templates de email, e enviar emails manuais ou automatizados vinculados a oportunidades.

**Acceptance Scenarios**:

1. **Given** um administrador, **When** cria uma integração de formulário web (gerando URL de webhook ou código de embed), **Then** o sistema fornece instruções de integração e novos envios criam leads automaticamente no funil e etapa configurados
2. **Given** leads/oportunidades no sistema, **When** o usuário envia email manual selecionando template ou escrevendo livremente, **Then** o email é enviado, cópia é salva no histórico da oportunidade, e status de entrega é registrado quando possível
3. **Given** templates de email cadastrados, **When** o usuário cria novo template com variáveis dinâmicas (nome cliente, valor proposta, etc.), **Then** o template pode ser reutilizado em envios manuais ou automatizados, com variáveis sendo substituídas pelos valores reais

---

### User Story 10 - Workflow de Atividades Automatizadas (Priority: P3)

Como gestor, quero configurar workflows automatizados que disparem ações (tarefas, emails) baseados em eventos, para aumentar produtividade e garantir follow-up consistente.

**Why this priority**: Automação reduz trabalho manual repetitivo e garante que processos sejam seguidos consistentemente.

**Independent Test**: Um administrador pode criar uma regra de workflow (ex: "Quando lead é criado, criar tarefa de qualificação em 1 dia"), ativá-la, e o sistema executa a regra automaticamente quando condições são atendidas.

**Acceptance Scenarios**:

1. **Given** um administrador, **When** cria regra de workflow definindo trigger (evento: lead criado, etapa alterada, etc.), condição (se aplicável), e ações (criar tarefa, enviar email, mover etapa, etc.), **Then** a regra é salva e fica ativa
2. **Given** regras de workflow ativas, **When** evento trigger ocorre (ex: lead criado na etapa "Novo Lead"), **Then** o sistema avalia condições e executa ações configuradas automaticamente
3. **Given** workflows executados, **When** o usuário visualiza histórico de uma oportunidade, **Then** ações automáticas são identificadas e registradas no histórico (sucessos e falhas)
4. **Given** uma regra de workflow que cria tarefa atribuída a usuário específico, **When** o workflow dispara mas o usuário alvo foi deletado ou desativado, **Then** o sistema valida usuário antes de executar, pula a ação, registra falha em log de auditoria e notifica administrador do tenant

---

### User Story 11 - Relatórios e Analytics (Priority: P3)

Como gestor ou administrador, quero gerar relatórios e análises sobre performance de vendas, funis, e equipe, para tomar decisões baseadas em dados.

**Why this priority**: Relatórios são essenciais para gestão estratégica, mas podem ser desenvolvidos após funcionalidades core estarem estáveis.

**Independent Test**: Um usuário com permissão pode acessar seção de relatórios, selecionar tipo de relatório (conversão por funil, performance de vendedores, pipeline por período), aplicar filtros, e visualizar dados em tabelas e gráficos exportáveis.

**Acceptance Scenarios**:

1. **Given** dados de vendas no sistema, **When** o usuário acessa relatórios e seleciona "Conversão por Funil", **Then** o sistema mostra taxa de conversão entre etapas, tempo médio em cada etapa, e funil visual com volumes
2. **Given** múltiplos vendedores com oportunidades, **When** o usuário visualiza relatório de "Performance por Vendedor", **Then** o sistema mostra métricas por vendedor (leads atribuídos, oportunidades criadas, valor fechado, taxa de conversão)
3. **Given** um relatório gerado, **When** o usuário aplica filtros (período, funil, vendedor, etc.), **Then** os dados são recalculados e exibidos conforme filtros
4. **Given** um relatório visualizado, **When** o usuário solicita exportação (PDF, Excel), **Then** o sistema gera arquivo formatado com os dados filtrados

---

### Edge Cases

- O que acontece quando um usuário tenta acessar dados de outro tenant? (Isolamento multitenancy) - Sistema bloqueia acesso e retorna erro de autorização
- Como o sistema lida com leads duplicados na importação? - Sistema detecta por email OU telefone e oferece opções: mesclar, pular ou criar mesmo assim
- O que acontece quando uma proposta é enviada mas o email falha? (retry, notificação)
- Como o sistema lida com usuários que têm múltiplos funis ativos simultaneamente?
- O que acontece quando uma etapa de funil é deletada mas possui oportunidades? - Sistema impede deleção e exige que usuário mova todas as oportunidades manualmente antes
- Como o sistema lida com permissões quando um usuário muda de perfil? - Sistema aplica novo perfil imediatamente, mantém histórico de ações com perfil anterior, e registra mudança em log de auditoria
- O que acontece quando workflow dispara ação mas usuário alvo não existe mais? - Sistema valida usuário antes de executar, pula ação se inválido, registra em log e notifica administrador
- Como o sistema lida com dados corrompidos na importação? (validação, rollback parcial)

## Requirements *(mandatory)*

### Functional Requirements

#### Multitenancy e Segurança
- **FR-001**: Sistema deve garantir isolamento estrito de dados entre tenants - nenhum tenant pode acessar dados de outro tenant sob nenhuma circunstância
- **FR-002**: Sistema deve autenticar usuários e validar permissões em todas as operações
- **FR-003**: Sistema deve implementar princípio do menor privilégio - usuários têm apenas permissões mínimas necessárias conforme perfil
- **FR-004**: Sistema deve registrar auditoria de operações críticas (criação/edição/deleção de leads, alterações de permissões, etc.)

#### Funis e Pipelines
- **FR-005**: Sistema deve permitir criação de múltiplos funis de vendas por tenant
- **FR-006**: Sistema deve permitir que usuários criem, editem, reordenem e deletem etapas de funil
- **FR-006a**: Sistema deve impedir deleção de etapa de funil se existirem oportunidades associadas a ela, exigindo que usuário mova/realoque todas as oportunidades manualmente antes de deletar
- **FR-007**: Sistema deve fornecer visualização kanban de leads/oportunidades por etapa do funil
- **FR-008**: Sistema deve permitir drag-and-drop de oportunidades entre etapas no kanban
- **FR-009**: Sistema deve fornecer visualização de lista de leads/oportunidades com colunas configuráveis e ordenação
- **FR-010**: Sistema deve fornecer visualização de tabela de leads/oportunidades com filtros avançados e exportação

#### Leads e Oportunidades
- **FR-011**: Sistema deve permitir criação manual de leads com campos: nome, empresa, email, telefone, origem, funil, etapa inicial
- **FR-012**: Sistema deve permitir qualificação de leads com campos customizáveis e score numérico
- **FR-013**: Sistema deve permitir conversão de lead em oportunidade atribuindo valor estimado e produtos/serviços
- **FR-014**: Sistema deve manter histórico completo de mudanças de etapa de oportunidades (data, usuário, etapa anterior, etapa nova)
- **FR-015**: Sistema deve permitir atribuição manual de leads/oportunidades a usuários
- **FR-016**: Sistema deve permitir distribuição automática de leads baseada em regras configuráveis (round-robin, por região, por origem, etc.)

#### Importação e Exportação
- **FR-017**: Sistema deve permitir importação de leads/oportunidades via arquivo CSV ou Excel
- **FR-018**: Sistema deve validar dados durante importação (emails válidos, campos obrigatórios, formatos corretos)
- **FR-019**: Sistema deve detectar leads duplicados durante importação comparando email OU telefone com registros existentes
- **FR-020**: Sistema deve oferecer opções ao usuário para cada duplicado detectado: mesclar dados com lead existente, pular registro duplicado, ou criar mesmo assim
- **FR-021**: Sistema deve reportar erros de importação por linha com detalhes específicos, incluindo duplicados detectados
- **FR-022**: Sistema deve permitir exportação de leads/oportunidades em CSV ou Excel com filtros aplicáveis
- **FR-023**: Sistema deve permitir mapeamento de colunas durante importação (coluna do arquivo → campo do sistema)

#### Tarefas e Anotações
- **FR-024**: Sistema deve permitir criação de tarefas vinculadas a leads/oportunidades com tipo, descrição, data/hora, responsável
- **FR-025**: Sistema deve permitir criação de anotações (texto livre com data/hora) vinculadas a leads/oportunidades
- **FR-026**: Sistema deve manter histórico cronológico de tarefas e anotações por lead/oportunidade
- **FR-027**: Sistema deve permitir marcar tarefas como concluídas e mostrar status no dashboard

#### Produtos, Serviços e Propostas
- **FR-028**: Sistema deve permitir cadastro de produtos/serviços com: nome, descrição, preço unitário, categoria, status (ativo/inativo)
- **FR-029**: Sistema deve permitir criação de propostas/cotações vinculadas a oportunidades selecionando produtos, quantidades, aplicando descontos
- **FR-030**: Sistema deve calcular automaticamente totais de propostas (subtotal, descontos, total)
- **FR-031**: Sistema deve gerar PDF de propostas com formatação profissional incluindo dados da empresa, cliente, produtos, valores
- **FR-032**: Sistema deve permitir envio de propostas por email com PDF anexado

#### Usuários, Perfis e Permissões
- **FR-033**: Sistema deve permitir criação de perfis de usuário com permissões granulares (visualizar/editar/deletar leads, acessar relatórios, gerenciar usuários, etc.)
- **FR-034**: Sistema deve permitir criação de usuários atribuindo perfil, email, nome
- **FR-035**: Sistema deve enviar convite por email para novos usuários
- **FR-036**: Sistema deve permitir que usuários redefinam senha via email
- **FR-037**: Sistema deve permitir edição de perfil do próprio usuário (nome, senha, preferências)
- **FR-037a**: Sistema deve permitir alteração de perfil de usuário (atribuição de novo perfil), aplicando novas permissões imediatamente
- **FR-037b**: Sistema deve manter histórico de ações do usuário com referência ao perfil que tinha no momento da ação, preservando contexto de auditoria
- **FR-037c**: Sistema deve registrar mudanças de perfil de usuário em log de auditoria incluindo: usuário alterado, perfil anterior, perfil novo, quem fez a alteração, data/hora

#### Dashboard
- **FR-038**: Sistema deve fornecer dashboard personalizado por usuário mostrando métricas relevantes ao perfil
- **FR-039**: Sistema deve calcular e exibir métricas: leads atribuídos, oportunidades por etapa, valor total em pipeline, tarefas pendentes, taxa de conversão
- **FR-040**: Sistema deve permitir filtrar dashboard por período (hoje, semana, mês, período customizado)
- **FR-041**: Sistema deve exibir dashboard com tempo de carregamento < 2 segundos para 95% dos casos

#### Integração e Email
- **FR-042**: Sistema deve fornecer integração com formulários web via webhook ou código de embed
- **FR-043**: Sistema deve permitir criação de templates de email com variáveis dinâmicas (nome cliente, valor proposta, etc.)
- **FR-044**: Sistema deve permitir envio manual de emails vinculados a oportunidades
- **FR-045**: Sistema deve salvar cópia de emails enviados no histórico da oportunidade
- **FR-046**: Sistema deve registrar status de entrega de emails quando disponível (enviado, entregue, falhou)

#### Workflow Automatizado
- **FR-047**: Sistema deve permitir criação de regras de workflow com trigger (eventos: lead criado, etapa alterada, etc.), condições opcionais, e ações (criar tarefa, enviar email, mover etapa)
- **FR-048**: Sistema deve executar workflows automaticamente quando triggers ocorrem e condições são atendidas
- **FR-048a**: Sistema deve validar existência e status de usuários alvo antes de executar ações de workflow que referenciam usuários
- **FR-048b**: Sistema deve pular ação de workflow se usuário alvo for inválido (não existe ou está desativado), registrar falha em log de auditoria e notificar administrador do tenant
- **FR-049**: Sistema deve registrar execuções de workflow no histórico de oportunidades (sucessos e falhas)
- **FR-050**: Sistema deve permitir ativar/desativar workflows individualmente

#### Relatórios
- **FR-051**: Sistema deve fornecer relatórios: conversão por funil, performance por vendedor, pipeline por período, taxa de conversão geral
- **FR-052**: Sistema deve permitir aplicar filtros em relatórios (período, funil, vendedor, etapa)
- **FR-053**: Sistema deve exibir relatórios em formato visual (tabelas, gráficos)
- **FR-054**: Sistema deve permitir exportação de relatórios em PDF ou Excel

#### Performance e Usabilidade
- **FR-055**: Sistema deve responder a 95% das requisições de API em menos de 200ms
- **FR-056**: Sistema deve funcionar corretamente em navegadores modernos (Chrome, Firefox, Safari, Edge - versões dos últimos 2 anos)
- **FR-057**: Sistema deve ser responsivo e utilizável em dispositivos móveis (tablets e smartphones)
- **FR-058**: Sistema deve fornecer feedback visual claro para todas as ações do usuário (loading, sucesso, erro)

### Key Entities *(include if feature involves data)*

- **Tenant**: Representa uma empresa cliente do SaaS. Possui configurações próprias, usuários, funis, produtos. Isolamento total de dados entre tenants.

- **User**: Representa um usuário do sistema pertencente a um tenant. Possui email, nome, perfil, preferências. Autenticação e autorização baseada em tenant + perfil.

- **Profile**: Representa um conjunto de permissões. Definido por tenant, pode ser atribuído a múltiplos usuários. Permissões granulares por recurso (leads, oportunidades, relatórios, usuários, etc.).

- **Funnel**: Representa um funil de vendas. Pertence a um tenant, possui nome, descrição, etapas ordenadas. Um tenant pode ter múltiplos funis.

- **FunnelStage**: Representa uma etapa dentro de um funil. Pertence a um funil, possui nome, ordem, propriedades (cor, ícone). Etapas são editáveis e reordenáveis.

- **Lead**: Representa um lead de vendas. Pertence a um tenant e funil, possui dados de contato (nome, empresa, email, telefone), qualificação (score, interesse, orçamento, timeline), origem, etapa atual, usuário atribuído, data de criação/atualização.

- **Opportunity**: Representa uma oportunidade de venda (lead convertido). Estende Lead com valor estimado, produtos/serviços associados, propostas criadas. Movimenta-se entre etapas do funil.

- **Task**: Representa uma tarefa. Vinculada a lead/oportunidade ou independente, possui tipo (ligar, email, reunião, etc.), descrição, data/hora agendada, responsável, status (pendente, concluída), data de conclusão.

- **Note**: Representa uma anotação. Vinculada a lead/oportunidade, possui texto livre, data/hora de criação, autor. Histórico cronológico.

- **Product/Service**: Representa um produto ou serviço vendável. Pertence a um tenant, possui nome, descrição, preço unitário, categoria, status (ativo/inativo).

- **Proposal**: Representa uma proposta comercial. Vinculada a uma oportunidade, possui lista de produtos com quantidades, descontos aplicados, totais calculados, status (rascunho, enviada, aceita, recusada), data de criação/envio.

- **EmailTemplate**: Representa um template de email. Pertence a um tenant, possui nome, assunto, corpo com variáveis dinâmicas, tipo (proposta, follow-up, etc.).

- **WorkflowRule**: Representa uma regra de workflow automatizado. Pertence a um tenant, possui trigger (evento), condições opcionais, ações a executar, status (ativo/inativo).

- **ActivityLog**: Representa log de atividades. Registra ações do sistema (criação de lead, mudança de etapa, envio de email, execução de workflow) com timestamp, usuário/sistema, detalhes.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Usuários podem completar onboarding inicial (criar conta, configurar primeiro funil, adicionar produto, criar primeiro lead) em menos de 10 minutos sem assistência externa

- **SC-002**: Sistema suporta 100 tenants simultâneos com isolamento completo de dados (0% de vazamento de dados entre tenants)

- **SC-003**: Sistema responde a 95% das requisições de API em menos de 200ms

- **SC-004**: Dashboard carrega e exibe métricas em menos de 2 segundos para 95% dos acessos

- **SC-005**: Usuários podem importar arquivo com até 1000 leads em menos de 30 segundos com validação completa

- **SC-006**: Sistema permite criação e edição de funil com até 10 etapas em menos de 5 segundos

- **SC-007**: Visualização kanban suporta até 100 oportunidades por etapa sem degradação de performance perceptível

- **SC-008**: 90% dos usuários conseguem criar e enviar uma proposta completa (produtos, valores, PDF, email) em menos de 5 minutos na primeira tentativa

- **SC-009**: Workflows automatizados executam ações em menos de 5 segundos após trigger

- **SC-010**: Sistema processa e distribui automaticamente novos leads de formulários web em menos de 10 segundos após submissão

- **SC-011**: Usuários podem gerar e visualizar relatório completo (com filtros aplicados) em menos de 3 segundos

- **SC-012**: 95% das operações de criação/edição de leads e oportunidades são concluídas sem erros na primeira tentativa
