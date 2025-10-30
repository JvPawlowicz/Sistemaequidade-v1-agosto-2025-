# 🚀 Roadmap — Sistema Equidade+ (Laravel)

## Sprint 0: Preparação e Setup
- [x] Documentação completa do sistema
- [ ] Criar repositório Git
- [ ] Configurar ambiente de desenvolvimento

## Sprint 1: Base do Projeto
- [ ] Criar projeto Laravel 11
- [ ] Configurar Sanctum e RBAC (Spatie Permission)
- [ ] Criar migrations: users, roles, units, model_has_permissions
- [ ] Implementar auth e policies básicas
- [ ] Middleware multiunidade (`ScopeByUnit`)
- [ ] Dropdown de troca de unidade no header
- [ ] Seeder inicial (usuários admin, roles, unidades)

## Sprint 2: Módulos Estruturais
- [ ] Dashboard (métricas gerais por perfil)
- [ ] Pacientes (CRUD completo + uploads + alertas)
- [ ] Profissionais (CRUD + vínculo com users + especialidades)
- [ ] Unidades (CRUD + filtro global)

## Sprint 3: Agenda e Atendimentos
- [ ] Agenda (CRUD + visualizações Dia/Semana/Mês)
- [ ] Drag & Drop para remarcação
- [ ] Recorrência de agendamentos (até 4x)
- [ ] Lista de espera
- [ ] Check-in de pacientes
- [ ] Histórico de faltas
- [ ] Frequência de pacientes

## Sprint 4: Operação Clínica
- [ ] Evoluções automáticas (após usual concluído)
- [ ] Modelos de evolução customizáveis
- [ ] Avaliações (Form Builder drag & drop)
- [ ] Prontuário (timeline consolidada)
- [ ] Exportação de prontuário em PDF

## Sprint 5: Gestão e Administração
- [ ] Financeiro simplificado (CRUD + relatórios básicos)
- [ ] Usuários e permissões (CRUD + reset de senha)
- [ ] Logs e auditoria (Laravel Auditing)
- [ ] Mural interno e notificações in-app
- [ ] Backup automático (comando artisan)

## Sprint 6: Relatórios e Painel
- [ ] Relatórios clínicos (evoluções, avaliações)
- [ ] Relatórios de atendimentos (ocupação, faltas)
- [ ] Relatórios de profissionais (produtividade)
- [ ] FiddyDiagramans (anfityDiagram avançados)
- [ ] Exportações PDF/CSV/Excel
- [ ] Filtros salvos e presets

## Sprint 7: Estoque e Materiais
- [ ] CRUD de materiais
- [ ] Controle de entrada/saída
- [ ] Relatório de consumo
- [ ] Alertas de estoque baixo

## Sprint 8: Testes e Qualidade
- [ ] Testes Pest para autenticação
- [ ] Testes de CRUDs
- [ ] Testes de policies
- [ ] Testes de multi-tenant
- [ ] Validação de responsividade mobile/tablet/desktop

## Sprint 9: Deploy e Documentação
- [ ] Gerar documentação Swagger para API
- [ ] Configurar deploy Hostinger/Railway
- [ ] CI/CD (GitHub Actions)
- [ ] Validação final e auditoria completa
- [ ] Manual do usuário

## Sprint 10: Otimizações e Escala
- [ ] Otimização de queries (Eager Loading)
- [ ] Cache de consultas frequentes
- [ ] Validação de performance
- [ ] Testes de carga
- [ ] Documentação técnica final

