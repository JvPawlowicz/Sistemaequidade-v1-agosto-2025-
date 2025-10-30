# 🗄️ Guia de Migração do Banco de Dados — Equidade+

Este documento explica como aplicar a migration do OpenEMR para o Equidade+.

---

## ⚠️ ANTES DE COMEÇAR

### 1. **FAÇA BACKUP DO BANCO DE DADOS**

```bash
# No seu servidor MySQL/MariaDB
mysqldump -u root -p openemr > backup_antes_migration.sql

# Ou via phpMyAdmin: Export > Go
```

### 2. **Verifique Versão do MySQL**

```sql
SELECT VERSION();
-- Deve ser MySQL 5.7+ ou MariaDB 10.2+
```

### 3. **Verifique Espaço em Disco**

A migration adiciona novas tabelas e colunas. Garanta espaço suficiente.

---

## 📋 ESTRUTURA DO SCRIPT DE MIGRATION

O script `sql/equidade_migration.sql` contém:

### **Parte 1: Criar Tabela UNITS**
- Cria tabela `units` baseada em `facility`
- Migra dados existentes de `facility` para `units`

### **Parte 2: Adicionar unit_id**
- Adiciona `unit_id` em `patient_data`
- Adiciona `unit_id` em `openemr_postcalendar_events`
- Migra dados existentes

### **Parte 3: Adaptar Tabela USERS**
- Adiciona campo `role` (admin, coordenador, profissional, secretaria)
- Adiciona `active_unit_id`
- Cria tabela `users_units` (pivot)
- Migra dados de `users_facility`

### **Parte 4-9: Criar Novas Tabelas**
- `professionals` - Profissionais da clínica
- `specialties` - Especialidades
- `professional_specialty` - Pivot especialidades
- `evolutions` - Evoluções clínicas
- `assessment_templates` - Templates de avaliações
- `assessments` - Avaliações preenchidas
- `notifications` - Notificações internas
- `materials` - Estoque de materiais
- `material_movements` - Movimentações de estoque
- `financial_records` - Registros financeiros simplificados

---

## 🚀 COMO APLICAR A MIGRATION

### **Opção 1: Via MySQL CLI**

```bash
cd "/Users/joaovictorgonzalezpawlowicz/Downloads/sitema com OSS pronta/openemr"

# Conectar ao MySQL
mysql -u root -p openemr

# Executar migration
source sql/equidade_migration.sql;

# Ou direto:
mysql -u root -p openemr < sql/equidade_migration.sql
```

### **Opção 2: Via phpMyAdmin**

1. Acesse phpMyAdmin
2. Selecione o banco `openemr`
3. Vá em "SQL"
4. Copie e cole o conteúdo de `sql/equidade_migration.sql`
5. Clique em "Go"

### **Opção 3: Via script PHP (recomendado para produção)**

```php
<?php
// migration_executor.php
require_once 'config.php';

$sql = file_get_contents('sql/equidade_migration.sql');
$statements = explode(';', $sql);

foreach ($statements as $statement) {
    $statement = trim($statement);
    if (!empty($statement)) {
        // Executar cada statement
        // (usar método seguro do seu framework)
    }
}
?>
```

---

## ✅ VERIFICAÇÃO PÓS-MIGRATION

### **1. Verificar Tabelas Criadas**

```sql
SHOW TABLES LIKE '%units%';
SHOW TABLES LIKE '%evolutions%';
SHOW TABLES LIKE '%assessments%';
SHOW TABLES LIKE '%notifications%';
SHOW TABLES LIKE '%materials%';
```

Deve retornar:
- `units`
- `users_units`
- `professionals`
- `specialties`
- `professional_specialty`
- `evolutions`
- `assessment_templates`
- `assessments`
- `notifications`
- `materials`
- `material_movements`
- `financial_records`

### **2. Verificar Colunas Adicionadas**

```sql
-- Verificar patient_data
DESCRIBE patient_data;
-- Deve ter coluna `unit_id`

-- Verificar users
DESCRIBE users;
-- Deve ter colunas `role` e `active_unit_id`

-- Verificar openemr_postcalendar_events
DESCRIBE openemr_postcalendar_events;
-- Deve ter coluna `unit_id`
```

### **3. Verificar Dados Migrados**

```sql
-- Verificar unidades migradas
SELECT COUNT(*) FROM units;

-- Verificar pacientes com unit_id
SELECT COUNT(*) FROM patient_data WHERE unit_id IS NOT NULL;

-- Verificar usuários com role
SELECT role, COUNT(*) FROM users GROUP BY role;
```

### **4. Verificar Índices**

```sql
SHOW INDEX FROM patient_data WHERE Key_name LIKE '%unit%';
SHOW INDEX FROM users WHERE Key_name LIKE '%role%' OR Key_name LIKE '%unit%';
```

---

## 🔧 CORREÇÃO DE PROBLEMAS

### **Erro: "Column already exists"**
- O script verifica se coluna existe antes de criar
- Se mesmo assim der erro, remova manualmente o `IF NOT EXISTS` equivalente

### **Erro: "Foreign key constraint fails"**
- Verifique se tabelas relacionadas existem
- Verifique ordem de criação das tabelas

### **Erro: "Table already exists"**
- O script usa `CREATE TABLE IF NOT EXISTS`
- Se tabela já existe, migration não vai recriar

### **Dados não migrados**
- Verifique se dados existem na tabela origem
- Execute queries de UPDATE manualmente se necessário

---

## 📊 ESTRUTURA FINAL

Após a migration, você terá:

```
units (nova)
  ├── id
  ├── name
  ├── address
  └── ...

patient_data (adaptada)
  ├── id
  ├── unit_id (NOVO)
  └── ...

users (adaptada)
  ├── id
  ├── role (NOVO)
  ├── active_unit_id (NOVO)
  └── ...

users_units (nova - pivot)
  ├── user_id → users.id
  └── unit_id → units.id

evolutions (nova)
  ├── id
  ├── patient_id → patient_data.id
  ├── professional_id → professionals.id
  └── ...

assessments (nova)
  ├── id
  ├── patient_id → patient_data.id
  └── ...
```

---

## 🎯 PRÓXIMOS PASSOS

Após aplicar a migration:

1. **Validar dados migrados**
2. **Configurar unidades no sistema**
3. **Atribuir roles aos usuários**
4. **Testar funcionalidades básicas**

---

## 📝 NOTAS

- A migration é **idempotente** (pode executar múltiplas vezes)
- Dados existentes são preservados
- Tabelas antigas (`facility`, `users_facility`) NÃO são removidas
- Soft deletes implementados nas novas tabelas

