# 🚀 Próximos Passos — Equidade+

## 📋 Status Atual

✅ **Completado:**
- Limpeza do código (remoção de billing, e-Rx, dependências externas)
- Migração completa do banco de dados
- Modelos PHP e Services
- Interfaces de Evoluções, Avaliações e Notificações
- Sistema de autenticação e permissões (RBAC)
- Integração com agendamentos (criação automática de evoluções)
- Componentes de interface (seletor de unidade, contador de pendentes)

---

## 🎯 Próximos Passos Prioritários

### 1. **Testes e Validação** 🔴 CRÍTICO

#### 1.1 Aplicar Migração do Banco de Dados
```bash
# Fazer backup primeiro!
mysql -u usuario -p database_name < sql/equidade_migration.sql

# Validar migração
php sql/test_migration.php
```

**Checklist:**
- [ ] Backup do banco antes da migração
- [ ] Executar migração em ambiente de teste
- [ ] Validar todas as tabelas criadas
- [ ] Verificar foreign keys
- [ ] Testar integridade dos dados migrados

#### 1.2 Testes Funcionais Básicos
- [ ] Login com diferentes roles (admin, coordenador, profissional, secretaria)
- [ ] Criar evolução manualmente
- [ ] Verificar criação automática após agendamento
- [ ] Testar filtros por unidade
- [ ] Verificar permissões por role
- [ ] Testar seletor de unidade

---

### 2. **Completar Funcionalidades Essenciais** 🟡 IMPORTANTE

#### 2.1 Formulário de Nova Avaliação
**Arquivo:** `interface/assessments/new.php`

**Necessário:**
- Formulário dinâmico baseado em JSON do template
- Validação de campos obrigatórios
- Salvamento de respostas em JSON
- Integração com busca de pacientes

#### 2.2 Visualização de Avaliações
**Arquivo:** `interface/assessments/view.php`

**Necessário:**
- Exibir template preenchido
- Mostrar respostas formatadas
- Opção de edição (se permitido)

#### 2.3 Template de Avaliação
**Arquivo:** `interface/assessments/templates.php`

**Funcionalidades:**
- Listar templates disponíveis
- Criar/editar templates
- Estrutura JSON para formulários dinâmicos

#### 2.4 Visualização de Notificações (Melhorar)
**Arquivo:** `interface/notifications/index.php`

**Melhorias:**
- Marcar como lida individualmente
- Filtros por tipo (sistema, clínica, mural)
- Notificações em tempo real (opcional)

---

### 3. **Integrações com Sistema OpenEMR** 🟡 IMPORTANTE

#### 3.1 Adicionar Menu Equidade+ ao Menu Principal
**Arquivo:** `interface/main/tabs/menu/menus/standard.json`

**Ação:**
- Incluir `equidade_menu.json` no menu padrão
- Ou adicionar itens diretamente no `standard.json`

**Localização sugerida:** Após o menu "Patient" ou criar seção própria

#### 3.2 Integrar Seletor de Unidade no Header
**Arquivo:** `templates/interface/main/tabs/user_data_template.html.twig`

**Ação:**
- Adicionar `<?php include("../../common/unit_selector.php"); ?>` no header do usuário

#### 3.3 Adicionar Contador no Menu
**Arquivo:** `interface/main/tabs/menu/menus/equidade_menu.json`

**Ação:**
- Adicionar badge de contador no item "Evoluções Clínicas"
- Usar componente `evolution_pending_count.php`

#### 3.4 Session Load após Login
**Arquivo:** `interface/login/login.php` ou onde login é processado

**Ação:**
- Garantir que `session_load.php` seja chamado após login bem-sucedido

---

### 4. **Dados Iniciais e Configuração** 🟢 RECOMENDADO

#### 4.1 Criar Scripts de Seed Data
**Arquivo:** `sql/equidade_seed_data.sql`

**Conteúdo:**
- Criar unidade padrão (se não existir)
- Criar roles de exemplo (se necessário)
- Criar templates de avaliação exemplo
- Criar especialidades básicas

#### 4.2 Configurar Roles dos Usuários Existentes
**Script:** Criar script PHP para migrar usuários existentes

**Ação:**
- Atribuir role 'profissional' para usuários que são providers
- Criar registros em `professionals` para usuários relevantes
- Vincular usuários a unidades através de `users_units`

#### 4.3 Configurar Unidade Padrão
- Garantir que todos os pacientes tenham `unit_id`
- Migrar `facility_id` para `unit_id` onde aplicável

---

### 5. **Melhorias de Interface** 🟢 MELHORIAS

#### 5.1 Dashboard Inicial
**Arquivo:** `interface/equidade/dashboard.php`

**Funcionalidades:**
- Estatísticas de evoluções (pendentes, concluídas, assinadas)
- Gráficos de atendimentos por período
- Lista de evoluções pendentes
- Notificações recentes

#### 5.2 Busca de Pacientes Melhorada
- Integrar com sistema de busca do OpenEMR
- Auto-complete melhorado
- Busca por CPF/RG

#### 5.3 Relatórios Básicos
**Arquivo:** `interface/equidade/reports.php`

**Relatórios:**
- Evoluções por profissional/período
- Atendimentos por unidade
- Avaliações realizadas

---

### 6. **Testes e Qualidade** 🔴 CRÍTICO

#### 6.1 Testes de Permissões
- Testar cada role com diferentes ações
- Verificar filtros por unidade
- Validar que admin vê tudo

#### 6.2 Testes de Integridade
- Verificar foreign keys
- Testar soft deletes
- Validar transações

#### 6.3 Testes de Performance
- Queries com filtros por unidade
- Índices adequados (já criados na migration)
- Cache de roles na sessão

---

### 7. **Documentação** 🟢 DOCUMENTAÇÃO

#### 7.1 Guia de Instalação
**Arquivo:** `docs/guia-instalacao.md`

**Conteúdo:**
- Pré-requisitos
- Passo a passo de instalação
- Aplicação da migração
- Configuração inicial

#### 7.2 Manual do Usuário
**Arquivo:** `docs/manual-usuario.md`

**Conteúdo:**
- Como criar evolução
- Como criar avaliação
- Como usar filtros
- Como trocar unidade

#### 7.3 Documentação Técnica
**Arquivo:** `docs/documentacao-tecnica.md`

**Conteúdo:**
- Arquitetura do sistema
- Estrutura do banco de dados
- APIs e Services
- Padrões de código

---

## 📅 Ordem de Prioridade Recomendada

### **Semana 1 (Crítico)**
1. ✅ Aplicar migração em ambiente de teste
2. ✅ Validação completa da migração
3. ✅ Testes básicos de funcionalidade
4. ✅ Integrar menu e seletor de unidade no header

### **Semana 2 (Importante)**
5. ✅ Completar formulário de avaliações
6. ✅ Criar visualização de avaliações
7. ✅ Scripts de seed data e migração de usuários
8. ✅ Testes de permissões

### **Semana 3 (Melhorias)**
9. ✅ Dashboard inicial
10. ✅ Relatórios básicos
11. ✅ Melhorias de interface
12. ✅ Documentação

---

## 🛠️ Comandos Úteis

### Aplicar Migração
```bash
cd openemr/sql
mysql -u root -p nome_database < equidade_migration.sql
```

### Validar Migração
```bash
php sql/test_migration.php
```

### Verificar Estrutura
```bash
mysql -u root -p nome_database -e "SHOW TABLES LIKE '%equidade%' OR LIKE '%evolutions%' OR LIKE '%units%';"
```

### Limpar Cache (se necessário)
```bash
# Limpar sessões
rm -rf sessions/*
```

---

## 🔍 Checklist de Deploy

Antes de colocar em produção:

- [ ] Backup completo do banco
- [ ] Migração testada em ambiente similar
- [ ] Todos os testes passando
- [ ] Permissões configuradas corretamente
- [ ] Roles atribuídos aos usuários
- [ ] Unidades criadas e usuários vinculados
- [ ] Menu integrado e visível
- [ ] Seletor de unidade funcionando
- [ ] Documentação atualizada
- [ ] Equipe treinada

---

## 💡 Dicas

1. **Sempre teste em ambiente de desenvolvimento primeiro**
2. **Faça backup antes de qualquer migração**
3. **Documente mudanças personalizadas**
4. **Mantenha branch separada para Equidade+**
5. **Use versionamento para migrations futuras**

---

**Última atualização:** Baseado nos commits até `a3486ade5`

