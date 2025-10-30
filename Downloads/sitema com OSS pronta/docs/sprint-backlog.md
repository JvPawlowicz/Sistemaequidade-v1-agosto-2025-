# 🎯 Sprint Backlog — Sistema Equidade+

## 📊 Status Atual
**Data de Início:** [A definir]  
**Última Atualização:** [Data]  
**Sprint Atual:** Preparação

---

## Sprint 0: Documentação e Planejamento

### ✅ Concluído
- [x] Documentação inicial do sistema
- [x] Definição de módulos e funcionalidades
- [x] Elaboração de regras de negócio
- [x] Schema do banco de dados
- [x] Arquitetura do sistema
- [x] Roadmap de desenvolvimento

### 📋 Pendente
- [ ] Criação do repositório Git
- [ ] Configuração do ambiente de desenvolvimento (PHP 8.3, MySQL 8)
- [ ] Definição de variáveis de ambiente (.env.example)
- [ ] Estrutura de pastas do projeto

---

## Sprint 1: Base do Projeto (CRÍTICO)

### 📝 Tarefas
- [ ] Criar projeto Laravel 11 via composer
- [ ] Instalar dependências:
  - [ ] Laravel Sanctum (auth)
  - [ ] Spatie Permission (RBAC)
  - [ ] Laravel Auditing (auditoria)
  - [ ] DomPDF (relatórios PDF)
  - [ ] Pest PHP (testes)
- [ ] Criar migrations:
  - [ ] users (com role, active_unit_id)
  - [ ] units
  - [ ] unit_user (pivot)
  - [ ] personal_access_tokens (Sanctum)
  - [ ] permissions, roles, model_has_permissions (Spatie)
- [ ] Criar middleware `ScopeByUnit`
- [ ] Criar Policies base (UserPolicy, PatientPolicy, etc)
- [ ] Implementar dropdown de unidades no layout
- [ ] Criar seeder inicial:
  - [ ] Roles (admin, coordenador, profissional, secretaria)
  - [ ] Unidade padrão
  - [ ] Usuário admin (email: admin@equidade.com, senha: admin123)
- [ ] Configurar rotas de autenticação (Sanctum)
- [ ] Criar layout base com sidebar e header
- [ ] Configurar TailwindCSS + Alpine.js

### 🔴 Bloqueadores
- Nenhum identificado

### ⚡ Dependências
- PHP 8.3 instalado
- MySQL 8 instalado
- Composer instalado

### ✅ Critérios de Aceitação
- [ ] Migrations executam sem erro
- [ ] Login funciona com usuário admin
- [ ] Dropdown de unidades aparece no header
- [ ] Troca de unidade funciona
- [ ] Middleware multi-unidade aplica filtro correto

---

## Sprint 2: Módulos Estruturais

### 📝 Tarefas

#### Dashboard
- [ ] Controller DashboardController
- [ ] Método index() com métricas por perfil
- [ ] View dashboard com cards de métricas
- [ ] Gráficos de ocupação (Chart.js ou similar)
- [ ] Ações rápidas (botões para criar paciente/agendamento)

#### Pacientes
- [ ] Migration patients (todos os campos)
- [ ] Model Patient (fillable, relationships, auditing)
- [ ] PatientPolicy (verificação de unidade)
- [ ] PatientController (CRUD completo)
- [ ] Request classes: StorePatientRequest, UpdatePatientRequest
- [ ] Views:
  - [ ] Listagem com datatables
  - [ ] Formulário de cadastro
  - [ ] Visualização de prontuário (timeline)
- [ ] Upload de documentos (storage)
- [ ] Alertas clínicos (lógica)

#### Profissionais
- [ ] Migration professionals
- [ ] Migration specialties
- [ ] Migration professional_specialty (pivot)
- [ ] Models: Professional, Specialty
- [ ] ProfessionalController
- [ ] Views de CRUD
- [ ] Seeder de especialidades padrão

#### Unidades
- [ ] UnitController (apenas Admin)
- [ ] Views de CRUD
- [ ] Horários de funcionamento (unit_schedules)

### 🔴 Bloqueadores
- Necessita Sprint 1 completo

### ✅ Critérios de Aceitação
- [ ] Dashboard mostra métricas corretas por perfil
- [ ] Cadastro de paciente funciona com validações
- [ ] Upload de documentos funciona
- [ ] Prontuário mostra timeline
- [ ] Policies bloqueiam acesso indevido

---

## Sprint 3: Agenda e Atendimentos (CRÍTICO)

### 📝 Tarefas
- [ ] Migration appointments (todos os campos e status)
- [ ] Model Appointment com relationships
- [ ] AppointmentPolicy
- [ ] AppointmentController:
  - [ ] index() com filtros
  - [ ] store() com validações
  - [ ] update() (mudança de status)
  - [ ] destroy()
- [ ] Views:
  - [ ] Calendário mensal (FullCalendar ou similar)
  - [ ] Visualização semana
  - [ ] Visualização dia
  - [ ] Visualização lista
  - [ ] Formulário de criação
  - [ ] Modal de edição rápida
- [ ] Drag & Drop (FullCalendar)
- [ ] Recorrência (lógica de criar até 4)
- [ ] Lista de espera (tabela separada)
- [ ] Check-in (mudança de status)
- [ ] Cálculo de faltas
- [ ] Exportação PDF (lista de agendamentos)
- [ ] Conflito automático (validação)

### 🔴 Bloqueadores
- Necessita Pacientes e Profissionais (Sprint 2)

### ✅ Critérios aesita Aceitação
- [ ] Calendário renderiza corretamente
- [ ] Drag & Drop remarca agendamento
- [ ] Recorrência cria 4 agendamentos
- [ ] Conflito é detectado e impede criação
- [ ] Check-in muda status corretamente
- [ ] Exportação PDF funciona

---

## Sprint 4: Operação Clínica

### 📝 Tarefas

#### Evoluções
- [ ] Migration evolutions
- [ ] Model Evolution
- [ ] EvolutionPolicy (apenas profissional dono pode editar)
- [ ] EvolutionController
- [ ] Criação automática pós-atendimento (Observer ou Event)
- [ ] Modelos efficientes de evolução (templates)
- [ ] Views:
  - [ ] Formulário de preenchimento
  - [ ] Listagem
  - [ ] Visualização completa
- [ ] Assinatura digital (timestamp)
- [ ] Exportação PDF da evolução

#### Avaliações
- [ ] Migration assessment_templates (com campo fields JSON)
- [ ] Migration assessments
- [ ] Models: AssessmentTemplate, Assessment
- [ ] Form Builder visual (Vue.js ou Alpine.js)
- [ ] Controllers e Views
- [ ] Integração com prontuário

#### Prontuário
- [ ] Service: MedicalRecordService
- [ ] Método consolidarTimeline()
- [ ] View de timeline
- [ ] Exportação completa do prontuário em PDF

### 🔴 Bloqueadores
- Necessita Agenda (Sprint 3)

### ✅ Critérios de Aceitação
- [ ] Evolução criada automaticamente após atendimento
- [ ] Profissional pode preencher e assinar
- [ ] Coordenador vê todas as evoluções
- [ ] Form Builder cria templates
- [ ] Prontuário exporta corretamente

---

## Sprint 5: Gestão e Administração

### 📝 Tarefas

#### Financeiro
- [ ] Migration financial_records
- [ ] Model FinancialRecord
- [ ] FinancialRecordController
- [ ] Views de registro manual
- [ ] Relatórios mensais

#### Usuários e Permissões
- [ ] UserController (Admin apenas)
- [ ] Reset de senha
- [ ] Gestão de permissões por role

#### Criticism e Auditoria
- [ ] Configurar Laravel Auditing
- [ ] View de logs de auditoria
- [ ] Exportação de logs

#### Notificações
- [ ] Migration notifications
- [ ] Notification model
- [ ] Service de criação automática
- [ ] Badge de não lidas no header
- [ ] Modal de notificações

#### Backups
- [ ] Comando artisan: php artisan backup:create
- [ ] Schedule diário

### ✅ Critérios de Aceitação
- [ ] Registro manual funciona
- [ ] Logs de auditoria completos
- [ ] Notificações aparecem no header
- [ ] Backup automatizado configurado

---

## Sprint 6: Relatórios e Painel

### 📝 Tarefas
- [ ] ReportController
- [ ] Relatórios:
  - [ ] Clínico (evoluções, avaliações)
  - [ ] Pacientes (frequência, evolução)
  - [ ] Profissionais (produtividade)
  - [ ] Atendimentos (ocupação, faltas)
- [ ] Gráficos (Chart.js):
  - [ ] Ocupação de agenda
  - [ ] Taxa de faltas
  - [ ] Evolução de pacientes ao longo do tempo
- [ ] Filtros avançados
- [ ] Filtros salvos (presets)
- [ ] Exportações:
  - [ ] PDF (DomPDF)
  - [ ] CSV
  - [ ] Excel (Laravel Excel)

### ✅ Critérios de Aceitação
- [ ] Todos os relatórios geram corretamente
- [ ] Gráficos são informativos
- [ ] Exportações funcionam
- [ ] Filtros salvos persistem

---

## Sprint 7: Estoque e Materiais

### 📝 Tarefas
- [ ] Migration materials
- [ ] Migration material_movements
- [ ] Models: Material, MaterialMovement
- [ ] Controllers e Views
- [ ] Lógica de entrada/saída
- [ ] Relatório de consumo
- [ ] Alertas de estoque baixo
- [ ] Integração com agendamentos

### ✅ Critérios de Aceitação
- [ ] Movimentação funciona
- [ ] Estoque não fica negativo
- [ ] Alertas aparecem no dashboard

---

## Sprint 8: Testes e Qualidade

### 📝 Tarefas
- [ ] Configurar Pest PHP
- [ ] Testes de autenticação
- [ ] Testes de CRUDs (Feature tests)
- [ ] Testes de Policies
- [ ] Testes de multi-tenant
- [ ] Testes de integração
- [ ] Validação de responsividade:
  - [ ] Mobile (< 768px)
  - [ ] Tablet (768px - 1024px)
  - [ ] Desktop (> 1024px)

### ✅ Critérios de Aceitação
- [ ] Cobertura de testes > 70%
- [ ] Todos os testes passam
- [ ] Interface responsiva em todos os dispositivos

---

## Sprint 9: Deploy e Documentação

### 📝 Tarefas
- [ ] Instalar L5-Swagger
- [ ] Documentar todos os endpoints da API
- [ ] Configurar deploy Hostinger:
  - [ ] Instalar PHP 8.3
  - [ ] Configurar MySQL 8
  - [ ] Deploy via Git
  - [ ] Configurar .env de produção
- [ ] Configurar deploy Railway (alternativo)
- [ ] CI/CD GitHub Actions:
  - [ ] Testes automáticos
  - [ ] Deploy automático
- [ ] Manual do usuário (PDF)
- [ ] Documentação técnica final

### ✅ Critérios de Aceitação
- [ ] Sistema hospedado e funcionando
- [ ] Documentação Swagger completa
- [ ] CI/CD configurado
- [ ] Manual do usuário disponível

---

## Sprint 10: Otimizações e Escala

### 📝 Tarefas
- [ ] Adicionar Eager Loading em queries
- [ ] Implementar cache (Redis recomendado):
  - [ ] Cache de dashboard
  - [ ] Cache de relatórios
- [ ] Otimizar queries lentas
- [ ] Testes de carga
- [ ] Documentação de performance

### ✅ Critérios de Aceitação
- [ ] Queries otimizadas
- [ ] Sistema suporta 100+ usuários simultâneos
- [ ] Tempo de resposta < 500ms

---

## 🚨 Bloqueadores Críticos

### Alto Risco
1. **Sprint 3 (Agenda)** - Complexidade alta do calendário e drag & drop
2. **Sprint 4 (Evoluções)** - Lógica de criação automática precisa estar perfeita
3. **Sprint 8 (Testes)** - Cobertura de testes extensa pode atrasar

### Médio Risco
1. **Sprint 6 (Relatórios)** - Integração de múltiplas exportações
2. **Sprint 9 (Deploy)** - Configuração de servidor

---

## 📈 Progresso Geral

| Sprint | Status | Progresso | Estimativa |
|--------|--------|-----------|------------|
| Sprint 0 | 🟡 Em andamento | 70% | 1 dia |
| Sprint 1 | ⚪ Não iniciado | 0% | 5 dias |
| Sprint 2 | ⚪ Não iniciado | 0% | 7 dias |
| Sprint 3 | ⚪ Não iniciado | 0% | 10 dias |
| Sprint 4 | ⚪ Não iniciado | 0% | 8 dias |
| Sprint 5 | ⚪ Não iniciado | 0% | 5 dias |
| Sprint 6 | ⚪ Não iniciado | 0% | 7 dias |
| Sprint 7 | ⚪ Não iniciado | 0% | 4 dias |
| Sprint 8 | ⚪ Não iniciado | 0% | 7 dias |
| Sprint 9 | ⚪ Não iniciado | 0% | 5 dias |
| Sprint 10 | ⚪ Não iniciado | 0% | 5 dias |

**Total estimado:** 63 dias (9 semanas)

---

## 🔄 Revisões e Atualizações

Este documento deve ser atualizado:
- ✅ A cada finalização de tarefa
- ✅ A cada novo bloqueador identificado
- ✅ Semanalmente (revisão geral)
- ✅ Ao final de cada sprint (retro příspěvěk ica)

**Próxima revisão:** [Data]

---

## 📝 Notas Importantes

### Decisões Técnicas
- ⚠️ **Não usar APIs externas** (email, SMS, WhatsApp desabilitados)
- ✅ Usar Laravel Sanctum ao invés de JWT (mais simples)
- ✅ TailwindCSS para estilização (sem Bootstrap)
- ✅ Alpine.js para interatividade (sem Vue.js pesado)
- ✅ FullCalendar para calendário (biblioteca confiável)

### Requisitos LGPD
- Soft deletes em todas as tabelas
- Logs de auditoria completos
- Retenção de dados por 5 anos
- Backup automático diário

### Compatibilidade
- PHP 8.3+
- MySQL 8.0+
- Navegadores modernos (Chrome, Firefox, Edge, Safari)


