# 🧹 Guia de Limpeza — OpenEMR → Equidade+

Este documento registra todas as limpezas realizadas no OpenEMR para adaptação ao Equidade+.

## ⚠️ IMPORTANTE

- Backup criado em branch: `backup-antes-limpeza`
- Todas as mudanças estão sendo commitadas incrementalmente
- Validar após cada fase

---

## 📋 FASE 1: Limpeza de Pastas/Arquivos da Interface

### ✅ 1.1 Remover Módulo de Billing (Financeiro)
- [x] `interface/billing/` removida (29 arquivos)

### ✅ 1.2 Remover Módulo eRx (Prescrições Eletrônicas)
- [x] Arquivos eRx da interface removidos
- [x] Templates eRx removidos

### ✅ 1.3 Remover Formulários Não Relevantes
- [x] `interface/forms/fee_sheet/` removida
- [x] `interface/forms/misc_billing_options/` removida
- [x] `interface/forms/prior_auth/` removida
- [x] Formulários específicos removidos (ankleinjury, bronchitis)

---

## 📋 FASE 2: Limpeza do Código Fonte (src/)

### ✅ 2.1 Remover Classes de Billing
- [x] `src/Billing/` removida (24 classes)

### ✅ 2.2 Limpar Testes Relacionados
- [x] Testes de billing removidos

---

## 📋 FASE 3: Limpeza do composer.json

### ✅ 3.1 Dependências Removidas
- [x] `academe/omnipay-authorizenetapi`
- [x] `google/apiclient`
- [x] `league/omnipay`
- [x] `omnipay/stripe`
- [x] `ringcentral/ringcentral-php`
- [x] `stripe/stripe-php`
- [x] `twilio/sdk`

### ✅ 3.2 Dependências Mantidas
- [x] `dompdf/dompdf` (PDFs)
- [x] `mpdf/mpdf` (PDFs alternativo)
- [x] `phpoffice/phpspreadsheet` (Excel)
- [x] `league/csv` (CSV exports)
- [x] Todas as outras dependências essenciais

---

## 📋 VALIDAÇÃO PÓS-LIMPEZA

### Estrutura Validada
- [x] `interface/main/` existe ✅
- [x] `interface/patient_file/` existe ✅
- [x] `interface/main/calendar/` existe ✅
- [x] `src/Common/` existe ✅
- [x] `src/Services/` existe ✅

### Composer Validado
- [x] `composer.json` válido ✅
- [x] Dependências removidas (7 dependências) ✅
- [x] Nenhuma referência restante a módulos removidos ✅

### Git
- [x] Branch de backup criada: `backup-antes-limpeza` ✅
- [x] Branch de trabalho criada: `equidade-limpeza` ✅
- [x] Commit realizado com todas as mudanças ✅

---

## 📝 Notas

- Módulos mantidos para avaliação posterior:
  - `interface/batchcom/` (notificações)
  - `interface/fax/` (comunicação)
  - `interface/forms/questionnaire_assessments/` (avaliações)

---

## ✅ FASE 7: Limpeza de Referências nos Menus (CONCLUÍDA)

### Menu standard.json
- [x] Removida seção completa "Fees" (billing) ✅
- [x] Removida seção completa "New Crop" (e-Rx) ✅
- [x] Removido item "eRx Logs" do menu Admin ✅
- [x] Removidos itens de billing dos relatórios ✅
- [x] JSON validado após alterações ✅

### menu_analysis.js
- [x] Referências a billing comentadas ✅

### Composer
- [x] Dependências atualizadas (`composer update`) ✅
- [x] Autoload atualizado ✅

### Notas sobre Referências Restantes
- Algumas referências a `billing_location` e `billing_facility` permanecem no código
- Essas são referências a campos do banco de dados que existem ainda
- Serão tratadas na fase de adaptação do banco de dados

---

## 🎯 Próximos Passos

1. ✅ **Limpeza de módulos** - CONCLUÍDA
2. ✅ **Limpeza de referências nos menus** - CONCLUÍDA
3. ⏭️ **Adaptação do banco de dados** (unit_id, novas tabelas)
4. ⏭️ **Tradução da interface** para português
5. ⏭️ **Criação de novos módulos** (evolutions, assessments)

