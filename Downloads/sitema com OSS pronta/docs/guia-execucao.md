# 🤖 Guia de Execução — IA / Agente de Desenvolvimento

## Objetivo
Guiar a IA para construir o sistema **Equidade+** de forma completa, sem erros, conforme boas práticas Laravel e requisitos clínicos do Grupo Equidade.

---

## 1. Ordem de Geração

### Passo 1: Preparar Ambiente
1. Ler `/docs/prompt-master.md` completamente
2. Ler `/docs/database-schema.md` para entender a estrutura de dados
3. Ler `/docs/business-rules.md` para conhecer as regras de negócio
4. Ler `/docs/routes-map.md` para entender os endpoints

### Passo 2: Criar Estrutura Base
1. Criar projeto Laravel 11: `composer create-project laravel/laravel equidade-plus`
2. Instalar dependências do `composer.json`:
   ```bash
   composer require laravel/sanctum
   composer require spatie/laravel-permission
   composer require spatie/laravel-auditing
   composer require barryvdh/laravel-dompdf
   composer require pestphp/pest --dev
   ```
3. Configurar serviços em `config/`
4. Publicar migrations dos pacotes

### Passo 3: Criar Migrations
1. Criar todas as migrations na ordem correta (respeitando foreign keys)
2. Adicionar `SoftDeletes` em todas as tabelas principais
3. Adicionar índices conforme `database-schema.md`
4. Executar migrations: `php artisan migrate`

### Passo 4: Criar Models
Para cada modelo:
1. Criar Model com `use SoftDeletes`, `use Auditable`
2. Definir `fillable` e `guarded`
3. Definir relationships (belongsTo, hasMany, etc)
4. Criar Factory para testes

### Passo 5: Criar Controllers
1. Criar Controllers organizados por módulo
2. Manter controllers FINOS (lógica em Services)
3. Aplicar middleware apropriado
4. Usar Form Requests para validação

### Passo 6: Criar Services
Serviços recomendados:
- `PatientService` (lógica de prontuário)
- `AppointmentService` (lógica de agenda e conflitos)
- `EvolutionService` (criação automática)
- `ReportService` (geração de relatórios)
- `NotificationService` (notificações in-app)

### Passo 7: Criar Policies
Para cada model que precisa de autorização:
1. Criar Policy
2. Implementar métodos: view, create, update, delete
3. Verificar sempre unit_id para não-admin
4. Admin sempre retorna true

### Passo 8: Criar Middleware
1. `ScopeByUnit` - filtro global por unidade
2. `EnsureUserHasUnit` - garante que user tem unidade ativa

### Passo 9: Criar Views
1. Layout principal com sidebar e header
2. Partial para dropdown de unidades
3. Componentes Blade reutilizáveis:
   - `card.blade.php`
   - `modal.blade.php`
   - `table.blade.php`
   - `alert.blade.php`
4. Views específicas de cada módulo

### Passo 10: Criar Seeders
1. `RolesSeeder` - roles padrão
2. `UnitsSeeder` - unidade padrão
3. `UsersSeeder` - usuário admin
4. `SpecialtiesSeeder` - especialidades clínicas
5. `DatabaseSeeder` - chama todos

### Passo 11: Criar Testes
Para cada módulo:
1. Feature tests de CRUD
2. Policy tests
3. Multi-tenant tests
4. Integração tests

### Passo 12: Validar e Testar
1. Executar: `php artisan migrate:fresh --seed`
2. Executar: `php artisan serve`
3. Testar login como admin
4. Criar dados de teste manualmente
5. Validar cada funcionalidade

---

## 2. Boas Práticas Obrigatórias

### Código
- ✅ PSR-12 para formatação
- ✅ Controllers finos → Services para lógica
- ✅ Requests para validação (não validar no controller)
- ✅ Policies para autorização
- ✅ Models com fillable seguro
- ✅ SoftDeletes em todas as tabelas
- ✅ Auditoria automática (Laravel Auditing)

### Segurança
- ✅ Validação em todos os inputs
- ✅ Policies em todas as rotas
- ✅ Proteção CSRF em formulários
- ✅ Rate limiting na API
- ✅ Sanitização de uploads
- ✅ Hashing de senhas (bcrypt)

### Performance
- ✅ Eager Loading em relationships
- ✅ Índices em foreign keys e campos de busca
- ✅ Paginação em listagens
- ✅ Cache em dados estáticos

### Testes
- ✅ Pest PHP para testes
- ✅ Cobertura mínima de 70%
- ✅ Testes de integração

---

## 3. Estrutura de Pastas

```
/app
  /Console/Commands
  /Events
  /Exceptions
  /Http
    /Controllers
      /API
      /Admin
    /Middleware
    /Requests
  /Jobs
  /Listeners
  /Models
  /Policies
  /Providers
  /Services
  /Notifications
/config
/database
  /factories
  /migrations
  /seeders
/public
/resources
  /js
  /css
  /views
    /layouts
    /components
    /partials
    /patients
    /appointments
    /evolutions
    etc.
/routes
  api.php
  web.php
/tests
  /Feature
  /Unit
```

---

## 4. Checklist de Conclusão

### Inicialização
- [ ] Projeto Laravel criado
- [ ] Dependências instaladas
- [ ] .env configurado
- [ ] Banco de dados criado

### Base
- [ ] Migrations criadas e executadas
- [ ] Models criados com relationships
- [ ] Seeder inicial executado
- [ ] Login funciona

### Módulos
- [ ] Dashboard (métricas)
- [ ] Pacientes (CRUD + prontuário __+ upload__)
- [ ] Profissionais (CRUD)
- [ ] Unidades (CRUD)
- [ ] Agenda (CRUD + calendário)
- [ ] Evoluções (automáticas)
- [ ] Avaliações (form builder)
- [ ] Relatórios (PDF/CSV)
- [ ] Financeiro (simplificado)
- [ ] Estoque (CRUD)
- [ ] Notificações (in-app)
- [ ] Administração (Admin)

### Qualidade
- [ ] Policies implementadas
- [ ] Middleware multi-tenant
- [ ] Auditoria ativa
- [ ] Testes escritos
- [ ] Responsividade mobile

### Deploy
- [ ] Documentação Swagger
- [ ] Deploy em produção
- [ ] CI/CD configurado

---

## 5. Comandos Importantes

### Desenvolvimento
```bash
# Instalar dependências
composer install
npm install

# Executar migrations
php artisan migrate

# Executar seeders
php artisan db:seed

# Limpar cache
php artisan cache:clear
php artisan config:clear
php artisan view:clear

# Gerar key
php artisan key:generate

# Rodar servidor
php artisan serve

# Compilar assets
npm run dev

# Rodar testes
php artisan test
```

### Produção
```bash
# Otimizar autoload
composer install --optimize-autoloader --no-dev

# Cachear configurações
php artisan config:cache
php artisan route:cache
php artisan view:cache

# Executar migrations
php artisan migrate --force

# Publicar vendor assets
php artisan vendor:publish --all
```

---

## 6. Fluxo de Trabalho Recomendado

1. **Criar Migration** → `php artisan make:migration create_patients_table`
2. **Editar Migration** → Definir estrutura
3. **Criar Model** → `php artisan make:model Patient`
4. **Criar Factory** → `php artisan make:factory PatientFactory`
5. **Criar Seeder** → `php artisan make:seeder PatientSeeder`
6. **Criar Controller** → `php artisan make:controller PatientController`
7. **Criar Form Requests** → `php artisan make:request StorePatientRequest`
8. **Criar Policy** → `php artisan make:policy PatientPolicy`
9. **Criar Service** → Criar arquivo manualmente em `app/Services`
10. **Criar Views** → Criar arquivos Blade
11. **Criar Testes** → `php artisan make:test PatientTest`
12. **Testar** → Executar testes e validar manualmente

---

## 7. Dicas para IA

- Não pular etapas - siga a ordem correta
- Testar cada módulo antes de avançar
- Usar Laravel collections sempre que possível
- Evitar queries N+1 (usar Eager Loading)
- Manter controllers com máximo 7 actions
- Usar Eloquent ao invés de Query Builder quando possível
- Validar todas as foreign keys
- Aplicar SoftDeletes em todas as exclusões
- Implementar auditoria em todos os models importantes

---

## 8. Problemas Comuns e Soluções

### Erro: Foreign key constraint fails
→ Verificar ordem das migrations
→ Executar `migrate:fresh --seed`

### Erro: Column not found
→ Verificar se migration foi executada
→ Limpar cache: `php artisan config:clear`

### Erro: Policy não funciona
→ Verificar se Policy está registrada em `AuthServiceProvider`
→ Verificar se user tem role correta

### Erro: Unidade não filtra
→ Verificar middleware `ScopeByUnit` nas rotas
→ Verificar se user tem `active_unit_id`

---

## 9. Validação Final

Execute esta sequência para validar o sistema completo:

```bash
# 1. Limpar tudo
php artisan migrate:fresh

# 2. Criar estrutura
php artisan migrate

# 3. Popular dados
php artisan db:seed

# 4. Rodar testes
php artisan test

# 5. Iniciar servidor
php artisan serve
```

Testar manualmente:
1. Login com admin@equidade.com / admin123
2. Trocar unidade no dropdown
3. Criar um paciente
4. Criar um profissional
5. Criar um agendamento
6. Concluir atendimento (gera evolução automática)
7. Preencher evolução
8. Gerar relatório PDF
9. Ver prontuário (timeline)
10. Testar responsividade mobile

Se tudo funcionar, **sistema está pronto para deploy**! 🎉


