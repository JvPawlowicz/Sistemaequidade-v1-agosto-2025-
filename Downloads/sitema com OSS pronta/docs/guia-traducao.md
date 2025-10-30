# 🌐 Guia de Tradução — Equidade+

Este documento explica como traduzir o sistema OpenEMR/Equidade+ para Português (Brasil).

---

## 📋 SISTEMA DE TRADUÇÃO DO OPENEMR

O OpenEMR usa um sistema de tradução baseado em **constantes e definições**:

- **`lang_constants`** - Armazena as constantes (chaves) em inglês
- **`lang_definitions`** - Armazena as traduções por idioma
- **`lang_languages`** - Lista de idiomas disponíveis

### Função de Tradução

```php
xl('Dashboard')  // Retorna tradução ou string original
xlt('Dashboard') // Tradução com escape HTML
xla('Dashboard') // Tradução para atributos HTML
```

---

## 🚀 ETAPA 1: Adicionar Português como Idioma

### Via Script SQL (Recomendado)

```bash
mysql -u root -p openemr < sql/add_portuguese_language.sql
```

Este script:
- ✅ Adiciona "Portuguese (Brazil)" como idioma
- ✅ Insere traduções básicas essenciais
- ✅ É idempotente (pode executar múltiplas vezes)

### Via Interface (Manual)

1. Acesse: **Administration > Language > Add Language**
2. Adicione:
   - **Language Code:** `pt`
   - **Description:** `Portuguese (Brazil)`

---

## 📝 ETAPA 2: Traduzir Strings

### Método A: Via Interface Administrativa

1. Acesse: **Administration > Language > Edit Definitions**
2. Selecione: **Portuguese (Brazil)**
3. Para cada constante:
   - Veja a definição em inglês
   - Digite a tradução em português
   - Clique em **"Load Definition"**

### Método B: Via SQL (Script Criado)

O script `add_portuguese_language.sql` já adiciona traduções básicas:
- Menu principal (Dashboard, Calendar, Patients, etc.)
- Roles (admin, coordenador, profissional, secretaria)
- Módulos Equidade+ (Evolutions, Assessments, etc.)

### Método C: Adicionar Traduções em Massa via SQL

```sql
-- Exemplo: Traduzir strings comuns
INSERT INTO lang_definitions (cons_id, lang_id, definition)
SELECT 
    c.cons_id,
    (SELECT lang_id FROM lang_languages WHERE lang_code = 'pt'),
    CASE c.constant_name
        WHEN 'Save' THEN 'Salvar'
        WHEN 'Cancel' THEN 'Cancelar'
        WHEN 'Delete' THEN 'Excluir'
        WHEN 'Edit' THEN 'Editar'
        WHEN 'New' THEN 'Novo'
        -- Adicione mais aqui
    END
FROM lang_constants c
WHERE c.constant_name IN ('Save', 'Cancel', 'Delete', 'Edit', 'New')
AND NOT EXISTS (
    SELECT 1 FROM lang_definitions d 
    WHERE d.cons_id = c.cons_id 
    AND d.lang_id = (SELECT lang_id FROM lang_languages WHERE lang_code = 'pt')
);
```

---

## 🔍 ETAPA 3: Identificar Strings Não Traduzidas

### Criar Script de Análise

Use o script `sql/find_untranslated_strings.php` (a ser criado) para:
- Listar todas as strings usadas no código
- Verificar quais têm tradução em português
- Gerar relatório de strings faltantes

---

## 📊 ESTRUTURA DE TRADUÇÕES

### Tabelas Envolvidas:

```sql
lang_languages
  - lang_id (PK)
  - lang_code (ex: 'pt', 'en')
  - lang_description (ex: 'Portuguese (Brazil)')

lang_constants
  - cons_id (PK)
  - constant_name (ex: 'Dashboard')

lang_definitions
  - def_id (PK)
  - cons_id (FK -> lang_constants)
  - lang_id (FK -> lang_languages)
  - definition (tradução)
```

---

## ✅ CHECKLIST DE TRADUÇÃO

### Prioridade Alta (Já incluído no script):
- [x] Menu principal (Dashboard, Calendar, Patients, Reports)
- [x] Roles/perfis (admin, coordenador, profissional, secretaria)
- [x] Módulos Equidade+ (Evolutions, Assessments, Notifications)

### Prioridade Média:
- [ ] Botões comuns (Save, Cancel, Delete, Edit, New)
- [ ] Mensagens de erro comuns
- [ ] Formulários de pacientes
- [ ] Formulários de agendamento

### Prioridade Baixa:
- [ ] Mensagens do sistema
- [ ] Tooltips
- [ ] Ajuda contextual
- [ ] Relatórios

---

## 🛠️ FERRAMENTAS ÚTEIS

### 1. Buscar Strings no Código

```bash
# Buscar todas as chamadas xl() no código
grep -r "xl(" interface/ | wc -l

# Listar strings únicas
grep -rho "xl('[^']*')" interface/ | sort -u
```

### 2. Verificar Traduções Existentes

```sql
-- Ver quantas traduções existem em português
SELECT COUNT(*) 
FROM lang_definitions d
JOIN lang_languages l ON d.lang_id = l.lang_id
WHERE l.lang_code = 'pt';

-- Ver strings sem tradução em português
SELECT c.constant_name, c.cons_id
FROM lang_constants c
WHERE NOT EXISTS (
    SELECT 1 FROM lang_definitions d
    JOIN lang_languages l ON d.lang_id = l.lang_id
    WHERE d.cons_id = c.cons_id
    AND l.lang_code = 'pt'
)
LIMIT 50;
```

---

## 🎯 CONFIGURAR PORTUGUÊS COMO IDIOMA PADRÃO

### Via Interface:
1. **Administration > Globals**
2. Seção **"Language"**
3. **Default Language:** Selecione "Portuguese (Brazil)"
4. Salvar

### Via SQL:
```sql
UPDATE globals 
SET gl_value = (SELECT lang_id FROM lang_languages WHERE lang_code = 'pt')
WHERE gl_name = 'language_default';
```

---

## 📝 NOTAS IMPORTANTES

1. **Idempotência**: Scripts de tradução verificam se tradução já existe
2. **Preservação**: Traduções existentes não são sobrescritas
3. **Sincronização**: Use "Manage Translations" para sincronizar após atualizações
4. **Cache**: Limpe cache do navegador após traduzir

---

## 🚀 PRÓXIMOS PASSOS

Após adicionar português:

1. ✅ Executar `sql/add_portuguese_language.sql`
2. ⏭️ Configurar português como idioma padrão
3. ⏭️ Testar interface em português
4. ⏭️ Adicionar mais traduções conforme necessário
5. ⏭️ Criar script para identificar strings faltantes

---

## 📚 REFERÊNCIAS

- Sistema de tradução: `library/translation.inc.php`
- Interface de admin: `interface/language/`
- Documentação: `interface/language/lang.info.html`

