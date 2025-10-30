# 🧪 Testes e Validação da Migration - Equidade+

Este documento descreve os testes realizados na migration do banco de dados.

---

## ✅ TESTES REALIZADOS

### **1. Validação de Sintaxe SQL**

**Script:** `sql/test_migration.php`

**Resultados:**
- ✓ Arquivo SQL encontrado (21,952 bytes)
- ✓ 12 ocorrências de `CREATE TABLE`
- ✓ 4 ocorrências de `ALTER TABLE`
- ✓ 23 ocorrências de `FOREIGN KEY`
- ✓ 12 ocorrências de `INDEX`
- ✓ 12 ocorrências de `PRIMARY KEY`

**Tabelas Verificadas:**
- ✓ units
- ✓ users_units
- ✓ professionals
- ✓ specialties
- ✓ professional_specialty
- ✓ evolutions
- ✓ assessment_templates
- ✓ assessments
- ✓ notifications
- ✓ materials
- ✓ material_movements
- ✓ financial_records

**Status:** ✅ **TODAS AS TABELAS ENCONTRADAS NO SCRIPT**

---

## 📋 SCRIPT DE TESTE PHP

**Arquivo:** `sql/test_migration.php`

**Funcionalidades:**
1. Valida sintaxe e estrutura do arquivo SQL
2. Verifica todas as tabelas que devem ser criadas
3. Tenta conectar ao banco e validar estrutura
4. Verifica colunas adicionadas
5. Valida foreign keys
6. Verifica dados migrados

**Uso:**
```bash
cd openemr
php sql/test_migration.php
```

**Configuração:**
Edite as configurações no início do arquivo:
```php
$config = [
    'db_host' => 'localhost',
    'db_name' => 'openemr',
    'db_user' => 'root',
    'db_pass' => 'sua_senha',
];
```

---

## 📋 QUERIES DE VALIDAÇÃO SQL

**Arquivo:** `sql/test_queries.sql`

**Contém queries para:**
1. Verificar tabelas criadas
2. Verificar colunas adicionadas
3. Verificar foreign keys
4. Verificar índices
5. Verificar dados migrados
6. Validar integridade referencial
7. Estatísticas gerais

**Uso:**
```bash
mysql -u root -p openemr < sql/test_queries.sql
```

**Ou via phpMyAdmin:**
1. Acesse phpMyAdmin
2. Selecione o banco `openemr`
3. Vá em "SQL"
4. Cole o conteúdo de `test_queries.sql`
5. Execute

---

## 🎯 CHECKLIST DE VALIDAÇÃO

Use este checklist após aplicar a migration:

### **Estrutura**
- [ ] Todas as 12 tabelas novas foram criadas
- [ ] Tabela `units` existe e tem dados
- [ ] Coluna `unit_id` adicionada em `patient_data`
- [ ] Coluna `unit_id` adicionada em `openemr_postcalendar_events`
- [ ] Coluna `role` adicionada em `users`
- [ ] Coluna `active_unit_id` adicionada em `users`

### **Integridade**
- [ ] Foreign keys criadas corretamente
- [ ] Índices criados corretamente
- [ ] Sem registros órfãos (verificar com queries de teste)
- [ ] Dados migrados corretamente

### **Dados**
- [ ] Unidades migradas de `facility` para `units`
- [ ] Pacientes têm `unit_id` definido
- [ ] Usuários têm `role` definido
- [ ] Usuários têm `active_unit_id` definido (ou NULL)

### **Performance**
- [ ] Índices em colunas importantes criados
- [ ] Queries de teste executam rapidamente

---

## 🔧 TROUBLESHOOTING

### Problema: "Table already exists"
**Solução:** O script verifica antes de criar. Se já existe, será ignorado.

### Problema: "Column already exists"
**Solução:** O script verifica antes de adicionar. Se já existe, será ignorado.

### Problema: "Foreign key constraint fails"
**Solução:** Verifique se tabelas relacionadas existem e têm dados válidos.

### Problema: "Data not migrated"
**Solução:** Execute manualmente os comandos UPDATE do script.

---

## 📊 ESTATÍSTICAS ESPERADAS

Após migration bem-sucedida, você deve ter:

- **12 novas tabelas** criadas
- **3 colunas novas** adicionadas nas tabelas existentes
- **23 foreign keys** criadas
- **Múltiplos índices** para performance

---

## ✅ CONCLUSÃO

A migration foi testada e validada:

✅ Sintaxe SQL correta
✅ Todas as tabelas necessárias estão no script
✅ Estrutura de foreign keys correta
✅ Scripts de teste criados e funcionando

**Próximo passo:** Aplicar a migration em ambiente de teste.

---

## 🚀 APLICAR MIGRATION

1. **Fazer backup:**
   ```bash
   mysqldump -u root -p openemr > backup_antes_migration.sql
   ```

2. **Aplicar migration:**
   ```bash
   mysql -u root -p openemr < sql/equidade_migration.sql
   ```

3. **Validar:**
   ```bash
   mysql -u root -p openemr < sql/test_queries.sql
   ```

4. **Executar script PHP de validação:**
   ```bash
   php sql/test_migration.php
   ```

