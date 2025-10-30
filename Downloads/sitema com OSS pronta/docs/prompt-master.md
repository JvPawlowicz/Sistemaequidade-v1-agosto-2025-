# 🏥 Equidade+ — Sistema de Gestão Clínica Multidisciplinar

## Stack
- Backend: Laravel 11 (PHP 8.3)
- Frontend: Blade + TailwindCSS + Alpine.js
- Banco de Dados: MySQL 8
- Autenticação: Laravel Sanctum + RBAC
- Relatórios: DomPDF
- Auditoria: Laravel Auditing
- Deploy: Hostinger / Railway
- Dependências externas: NENHUMA (APIs externas desabilitadas)
- Multi-tenant: por unidade (`unit_id`)

---

## 🎯 Objetivo do Sistema
Criar um sistema web **completo**, estável e escalável, para **gestão de clínicas multidisciplinares** que atendem pessoas com **deficiências, TEA, TDAH e neurodivergências**.

O sistema deve incluir módulos clínicos, administrativos e de relatórios, com **gestão multiunidades**, **fluxos automatizados**, **perfis de acesso diferenciados** e **painel administrativo robusto**.

⚠️ **IMPORTANTE**: Sistema opera completamente offline, sem integrações com APIs externas de comunicação.

---

## 👥 Hierarquia de Usuários e Permissões (RBAC)

| Função | Acesso | Permissões |
|--------|---------|-------------|
| **Admin** | Total (todas as unidades) | CRUD completo, auditoria, backups |
| **Coordenador** | Unidades vinculadas | Relatórios, gestão de equipe, ver todas evoluções |
| **Profissional** | Próprios pacientes | Evoluções próprias, avaliações |
| **Secretária** | Unidade vinculada | Agendamentos, check-ins, pacientes |

**Regras:**
- Cada usuário pode pertencer a uma ou mais unidades
- Dropdown no header permite troca de unidade ativa
- Policies controlam acesso a registros por unidade
- Middleware `ScopeByUnit` define o escopo global
- Admin sempre vê todos os dados

---

## 🧱 Arquitetura

```
/app
  /Models
  /Http/Controllers
  /Http/Requests
  /Http/Middleware
  /Policies
  /Services
  /Notifications (in-app only)
/config
/database
  /migrations
  /seeders
  /factories
/resources
  /views
  /js
  /css
/routes
  api.php
  web.php
/tests
```

---

## 🧩 Módulos do Sistema

### 1. Dashboard
- Cards de métricas por perfil:
  - Admin: Total geral + por unidade
  - Coordenador: Unidades + equipe
  - Profissional: Seus pacientes + agendamentos
  - Secretária: Agendamentos do dia
- Gráficos: ocupação de agenda, evolução de pacientes
- Ações rápidas: Novo paciente, Novo agendamento
- Filtro temporal (dia/semana/mês)
- **SEM notificações em tempo real** (atualização manual)

### 2. Agenda
- Visualizações: Dia, Semana, Mês, Lista
- Drag & Drop para remarcação
- Status: Agendado → Confirmado → Check-in → Em andamento → Concluído → Cancelado
- Recorrência (máx. 4x)
- Lista de espera
- Bloqueio de horários (feriados, manutenção)
- Histórico de faltas e ausências
- Frequência do paciente
- Exportação PDF/CSV
- Conflito automático de horários
- Fluxo de atendimento → gera evolução automática
- Permissões: Secretária / Profissional / Coordenador / Admin

### 3. Pacientes
- Campos principais: Nome, RG/CPF, nascimento, sexo
- Contatos: Telefone, email, contato de emergência
- Endereço completo com CEP
- Responsável financeiro e responsável legal
- Diagnóstico principal e secundários
- Alergias e restrições
- Medicamentos em uso
- Plano de crise
- Tags: TEA, TDAH, etc.
- Convenio/Plano de saúde (apenas cadastro textual, sem valores)
- Upload de documentos: laudos, relatórios
- Anamnese inicial
- Prontuário único (timeline)
- Alertas clínicos automáticos
- Permissões: Profissionais da unidade

### 4. Evoluções
- Criação automática após atendimento concluído
- Campos: relato, plano terapêutico, objetivos
- Status: pendente / concluída / assinada
- Modelos customizáveis por especialidade
- Exportação PDF
- Adendo para correção (reabertura controlada)
- Permissões:
  - Visualizar: Profissional dono / Coordenador / Admin
  - Editar: Apenas profissional dono

### 5. Avaliações (Form Builder)
- Criação visual de formulários (drag & drop)
- Campos: texto, número, seleção única, seleção múltipla, texto longo
- Integração com prontuário (timeline)
- **SEM assinatura digital** (sistema simples)
- Permissões: Profissional / Coordenador / Admin

### 6. Profissionais
- CRUD de profissionais
- Vínculo com usuários do sistema
- CRMs e registros profissionais
- Especialidades (Psicologia, TO, Fono, etc.)
- Escala de trabalho
- Histórico de faltas e atestados
- Gestão de férias
- Relatórios por profissional: atendimentos, tempo médio
- Permissões: Admin / Coordenador

### 7. Relatórios
- Tipos:
  - Clínico: Evoluções, avaliações, prontuários
  - Pacientes: Frequência, evolução ao longo do tempo
  - Profissionais: Produtividade, tempo médio casos
  - Atendimentos: Taxa de ocupação, faltas
- Gráficos + exportação PDF/CSV/Excel
- Filtros salvos e presets
- Permissões: Coordenador / Admin

### 8. Financeiro (Simplificado)
- Registro manual de atendimentos pagos / isentos
- Relatórios mensais por profissional
- Histórico de pagamentos
- **SEM integração com tabelas de valores**
- Permissões: Secretária / Admin

### 9. Administração
- CRUD de usuários
- CRUD de unidades
- CRUD de especialidades
- CRUD de formulários de avaliação
- Logs e auditoria
- Mural interno (comunicados)
- Backup manual e automático
- Permissões: Admin

### 10. Notificações (In-App Only)
- Notificações do sistema (pendências, alertas)
- Mural interno por unidade
- Histórico de notificações lidas/não lidas
- **SEM notificações por email/SMS**
- Permissões: Todos os usuários logados

### 11. Auditoria e Logs
- Histórico completo de ações CRUD (Laravel Auditing)
- Logs de login, IP e dispositivo
- Exportação CSV/PDF
- Permissões: Admin

### 12. Estoque e Materiais
- Cadastro de materiais utilizados nas sessões
- Controle de entrada e saída
- Relatório de consumo por profissional/paciente
- Alertas de estoque baixo
- Permissões: Secretária / Admin

---

## 🔐 Segurança e Auditoria

- Middleware: `auth:sanctum`, `scope.unit`, `role:admin|coordenador|profissional|secretaria`
- Row Level Security (RLS) por unit_id
- Auditoria por modelo (Laravel Auditing)
- Logs de login, IP e dispositivo
- Backup automático diário
- SoftDeletes em todas as tabelas
- Hashing de senhas (bcrypt)
- Rate limiting na API

---

## 🎨 Interface e UX

- Layout responsivo (mobile/tablet/desktop)
- Sidebar fixa com ícones (Lucide Icons)
- Header com dropdown de unidade ativa
- Paleta institucional:
  - Indigo Dye #004684 (primária)
  - Orange #B70D04 (secundária)
  - Forest Green #01873B (sucesso)
  - Platinum #C5C8C8 (neutro)
- Tipografia: Inter / Nunito (Google Fonts)
- Ícones: Lucide Icons
- Componentes Blade reutilizáveis

---

## 🧾 Deploy

1. Clonar repositório
2. Configurar `.env`
3. `composer install`
4. `php artisan key:generate`
5. `php artisan migrate --seed`
6. `php artisan serve`

---

## 🧪 Testes

- Framework: Pest PHP
- Testes de autenticação, CRUDs e policies
- CI/CD via GitHub Actions

---

## 📦 Comando para IA (Cursor)

```
@workspace
Leia todos os documentos em /docs/.
Crie o sistema Equidade+ completo conforme as especificações.
Stack: Laravel 11 + PHP 8.3 + MySQL 8 + Blade + TailwindCSS + Sanctum + Laravel Auditing + DomPDF.
Implemente todos os módulos, rotas, migrations, seeders, policies, validações e testes.
Crie interface Blade e API RESTful completa.
IMPORTANTE: Não use APIs externas de comunicação (email, SMS, WhatsApp).
Ao finalizar, valide com:
php artisan migrate --seed && php artisan serve
```

