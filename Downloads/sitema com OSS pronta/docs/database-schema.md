# 🗄️ Database Schema — Equidade+

## Modelo Entidade-Relacionamento

### Relacionamentos Principais

```
Unit 1---N User (usuários podem estar em múltiplas unidades via pivot)
User 1---N Appointment (agendamentos)
Patient 1---N Appointment (um paciente tem vários agendamentos)
Patient 1---N Evolution (evoluções clínicas)
Appointment 1---0..1 Evolution (agendamento gera evolução)
Patient 1---N Assessment (avaliações)
Patient 1---N Document (documentos do prontuário)
Patient N---N Responsible (pivot - responsáveis)
Professional 1---1 User (profissional vincula com usuário)
Professional N---N Specialty (pivot - especialidades)
Appointment N---1 Professional (agendamento com profissional)
Material N---N Appointment (materiais usados na sessão)
Notification N---1 User (notificações)
Unit 1---N Schedule (horários de funcionamento)
```

---

## Estrutura de Tabelas

### users
```sql
- id (bigint, PK)
- name (string)
- email (string, unique)
- email_verified_at (timestamp, nullable)
- password (string)
- role (enum: admin, coordenador, profissional, secretaria)
- active_unit_id (bigint, nullable, FK -> units.id)
- phone (string, nullable)
- created_at, updated_at, deleted_at (timestamps)
```

### units
```sql
- id (bigint, PK)
- name (string)
- address (text)
- phone (string, nullable)
- active (boolean, default: true)
- created_at, updated_at, deleted_at (timestamps)
```

### unit_user (pivot)
```sql
- id (bigint, PK)
- unit_id (bigint, FK -> units.id)
- user_id (bigint, FK -> users.id)
- created_at, updated_at
```

### patients
```sql
- id (bigint, PK)
- unit_id (bigint, FK -> units.id)
- name (string)
- rg (string, nullable)
- cpf (string, nullable)
- birth_date (date)
- gender (enum: M, F, O)
- phone (string, nullable)
- email (string, nullable)
- address (text, nullable)
- cep (string, nullable)
- emergency_contact (string, nullable)
- emergency_phone (string, nullable)
- diagnosis (text, nullable)
- secondary_diagnoses (json, nullable)
- tags (json, nullable) // TEA, TDAH, etc
- allergies (text, nullable)
- medications (text, nullable)
- crisis_plan (text, nullable)
- insurance_plan (string, nullable) // Nome do plano sem valores
- responsible_name (string, nullable)
- responsible_rg (string, nullable)
- responsible_cpf (string, nullable)
- active (boolean, default: true)
- notes (text, nullable)
- created_at, updated_at, deleted_at (timestamps)
```

### professionals
```sql
- id (bigint, PK)
- user_id (bigint, FK -> users.id, unique)
- crm_number (string, nullable)
- crm_state (string, nullable)
- registry_type (string, nullable) // CRP, CREFITO, etc
- hire_date (date, nullable)
- notes (text, nullable)
- created_at, updated_at, deleted_at (timestamps)
```

### specialties
```sql
- id (bigint, PK)
- name (string, unique)
- description (text, nullable)
- created_at, updated_at, deleted_at (timestamps)
```

### professional_specialty (pivot)
```sql
- id (bigint, PK)
- professional_id (bigint, FK -> professionals.id)
- specialty_id (bigint, FK -> specialties.id)
- created_at, updated_at
```

### appointments
```sql
- id (bigint, PK)
- unit_id (bigint, FK -> units.id)
- patient_id (bigint, FK -> patients.id)
- professional_id (bigint, FK -> professionals.id)
- scheduled_at (datetime)
- duration (integer) // minutos
- status (enum: agendado, confirmado, check-in, em-andamento, concluido, cancelado)
- type (string, nullable) // consulta, retorno, etc
- notes (text, nullable)
- reason (text, nullable)
- absence_reason (text, nullable) // para faltas
- created_at, updated_at, deleted_at (timestamps)
```

### evolutions
```sql
- id (bigint, PK)
- appointment_id (bigint, FK -> appointments.id, nullable)
- unit_id (bigint, FK -> units.id)
- patient_id (bigint, FK -> patients.id)
- professional_id (bigint, FK -> professionals.id)
- status (enum: pendente, concluida, assinada)
- report (text)
- plan (text, nullable)
- objectives (text, nullable)
- addendum (text, nullable) // para correções
- signed_at (datetime, nullable)
- created_at, updated_at, deleted_at (timestamps)
```

### assessment_templates
```sql
- id (bigint, PK)
- unit_id (bigint, FK -> units.id, nullable) // null = global
- specialty_id (bigint, FK -> specialties.id, nullable)
- name (string)
- description (text, nullable)
- fields (json) // estrutura do formulário
- active (boolean, default: true)
- created_at, updated_at, deleted_at (timestamps)
```

### assessments
```sql
- id (bigint, PK)
- template_id (bigint, FK -> assessment_templates.id)
- unit_id (bigint, FK -> units.id)
- patient_id (bigint, FK -> patients.id)
- professional_id (bigint, FK -> professionals.id)
- answers (json) // respostas do formulário
- notes (text, nullable)
- created_at, updated_at, deleted_at (timestamps)
```

### documents
```sql
- id (bigint, PK)
- unit_id (bigint, FK -> units.id)
- patient_id (bigint, FK -> patients.id)
- name (string)
- type (string)
- path (string)
- size (bigint)
- mime_type (string)
- uploaded_by (bigint, FK -> users.id)
- created_at, updated_at, deleted_at (timestamps)
```

### materials
```sql
- id (bigint, PK)
- unit_id (bigint, FK -> units.id)
- name (string)
- description (text, nullable)
- quantity (integer)
- min_stock (integer)
- unit (string) // unidade, caixa, etc
- created_at, updated_at, deleted_at (timestamps)
```

### material_movements
```sql
- id (bigint, PK)
- material_id (bigint, FK -> materials.id)
- appointment_id (bigint, FK -> appointments.id, nullable)
- type (enum: entrada, saida)
- quantity (integer)
- notes (text, nullable)
- created_by (bigint, FK -> users.id)
- created_at, updated_at
```

### notifications
```sql
- id (bigint, PK)
- unit_id (bigint, FK -> units.id, nullable)
- user_id (bigint, FK -> users.id, nullable)
- title (string)
- message (text)
- type (enum: sistema, clinica, mural)
- read_at (datetime, nullable)
- created_at, updated_at, deleted_at (timestamps)
```

### financial_records
```sql
- id (bigint, PK)
- unit_id (bigint, FK -> units.id)
- patient_id (bigint, FK -> patients.id)
- appointment_id (bigint, FK -> appointments.id, nullable)
- professional_id (bigint, FK -> professionals.id)
- type (enum: pago, isento)
- amount (decimal 10,2, nullable) // null se isento
- date (date)
- notes (text, nullable)
- created_by (bigint, FK -> users.id)
- created_at, updated_at, deleted_at (timestamps)
```

### unit_schedules
```sql
- id (bigint, PK)
- unit_id (bigint, FK -> units.id)
- day_of_week (integer) // 0-6 (domingo-sábado)
- start_time (time)
- end_time (time)
- active (boolean, default: true)
- created_at, updated_at
```

### audit_logs (Laravel Auditing automático)
```sql
- id (bigint, PK)
- auditable_type (string)
- auditable_id (bigint)
- event (string) // created, updated, deleted
- user_id (bigint, FK -> users.id, nullable)
- ip_address (string, nullable)
- user_agent (string, nullable)
- old_values (json, nullable)
- new_values (json, nullable)
- created_at, updated_at
```

### failed_jobs
```sql
- id (bigint, PK)
- uuid (string, unique)
- connection (text)
- queue (text)
- payload (longtext)
- exception (longtext)
- failed_at (timestamp)
```

### migrations, personal_access_tokens (Sanctum), password_reset_tokens
(Tabelas padrão do Laravel)

---

## Índices e Otimizações

```sql
-- Usuários
CREATE INDEX idx_users_role ON users(role);
CREATE INDEX idx_users_active_unit ON users(active_unit_id);

-- Pacientes
CREATE INDEX idx_patients_unit ON patients(unit_id);
CREATE INDEX idx_patients_cpf ON patients(cpf);
CREATE INDEX idx_patients_active ON patients(active);

-- Agendamentos
CREATE INDEX idx_appointments_unit ON appointments(unit_id);
CREATE INDEX idx_appointments_patient ON appointments(patient_id);
CREATE INDEX idx_appointments_professional ON appointments(professional_id);
CREATE INDEX idx_appointments_scheduled ON appointments(scheduled_at);
CREATE INDEX idx_appointments_status ON appointments(status);

-- Evoluções
CREATE INDEX idx_evolutions_patient ON evolutions(patient_id);
CREATE INDEX idx_evolutions_professional ON evolutions(professional_id);
CREATE INDEX idx_evolutions_appointment ON evolutions(appointment_id);

-- Notificações
CREATE INDEX idx_notifications_user ON notifications(user_id);
CREATE INDEX idx_notifications_read ON notifications(read_at);
```

---

## Soft Deletes
Todas as tabelas principais devem ter `deleted_at` (Soft Deletes) para conformidade com LGPD e auditoria.

---

## Convenções
- Todos os timestamps em UTC
- Nomes de tabelas em snake_case, plural
- Nomes de campos em snake_case
- Todos os IDs são bigint unsigned
- Todos os campos de texto têm charset utf8mb4
- Default created_at e updated_at automaticamente

