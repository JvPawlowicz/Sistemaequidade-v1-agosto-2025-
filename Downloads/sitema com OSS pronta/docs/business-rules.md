# 📜 Regras de Negócio — Equidade+

## 🔐 Autenticação e Autorização

### Login e Sessão
- Sistema usa **Laravel Sanctum** para autenticação
- Sessão mantida por 8 horas (configurável)
- Logout automático após inatividade de 2 horas
- Admin NUNCA pode se auto-deletar

### Roles e Permissões
- Cada usuário tem um **role** principal: admin, coordenador, profissional, secretaria
- Um usuário pode estar vinculado a **múltiplas unidades**
- Um usuário tem uma **unidade ativa** (selecionada via dropdown)
- Admin sempre vê todos os dados (ignora filtro de unidade)
- Demais roles só veem dados da unidade ativa

---

## 👥 Usuários e Profissionais

### Criação de Usuário
- Email deve ser único no sistema
- Senha mínima: 8 caracteres, sem validação adicional
- Ao criar usuário com role "profissional", criar registro em `professionals`
- Profissionais podem ter múltiplas especialidades

### Permissões por Role

**Admin:**
- Acesso total a todas as unidades
- CRUD de usuários, unidades, profissionais
- Gestão de backups e logs
- Não precisa de validação de policies

**Coordenador:**
- Acesso às unidades vinculadas
- Pode ver TODAS as evoluções da unidade (não apenas próprias)
- Pode editar perfil de profissionais da equipe
- Pode gerar todos os relatórios da unidade

**Profissional:**
- Acesso apenas aos próprios pacientes
- Pode criar e editar suas próprias evoluções
- Pode criar avaliações para seus pacientes
- NÃO pode alterar status financeiro

**Secretária:**
- Acesso aos pacientes da unidade
- Acesso a todos os agendamentos da unidade
- Pode fazer check-in de pacientes
- Pode registrar pagamentos
- NÃO pode criar/editar evoluções
- NÃO pode criar avaliações

---

## 👨‍👩‍👦 Pacientes

### Cadastro de Paciente
- Nome obrigatório
- RG ou CPF obrigatório (pelo menos um)
- Data de nascimento obrigatória
- Unit_id obrigatório (unidade de vinculação)
- Tags (TEA, TDAH) são opcionais, armazenadas em JSON

### Prontuário
- Prontuário único por paciente
- Timeline inclui: agendamentos, evoluções, avaliações, documentos
- Nenhum dado pode ser deletado definitivamente (soft delete)
- Timeline ordenada por data descendente

### Alertas Clínicos
- Gerados automaticamente para:
  - Paciente com plano de crise ativo
  - Ausências consecutivas (2+ faltas)
  - Prescrição de medicamentos sensíveis

---

## 📅 Agenda e Agendamentos

### Criação de Agendamento
- Profissional e Paciente devem ser da MESMA unidade
- Data/hora obrigatórias
- Duração padrão: 50 minutos (configurável)
- Status inicial: "agendado"

### Status de Agendamento
Fluxo permitido:
```
agendado → confirmado → check-in → em-andamento → concluído
agendado → cancelado
confirmado → cancelado
```

### Remarcação
- Apenas Secretária e Profissional responsável podem remarcar
- Remarcação cria novo agendamento e cancela o antigo
- Drag & Drop permite mover visualmente na agenda

### Recorrência
- Máximo de 4 recorrências (semanais, quinzenais, mensais)
- Cada recorrência é um agendamento independente
- Cancelar um agendamento recorrente NÃO cancela os outros

### Faltas
- Agendamento com status diferente de "concluído" após o horário = ausência
- Secretária pode marcar como ausência previamente
- Histórico de faltas no perfil do paciente

### Now de Conflitos
- Sistema valida se profissional está disponível no horário
- Não permite agendar dois atendimentos simultâneos
- Mensagem de erro clara em caso de conflito

---

## 📝 Evoluções

### Criação
- Criação **automática** quando agendamento muda para "concluído"
- Evolução fica com status "pendente"
- Apenas o profissional responsável pode preencher
- Campos obrigatórios: relato

### Edição
- Apenas profissional dono pode editar evolução pendente ou concluída
- Evolução "assinada" NÃO pode ser editada
- Correções via adendo (campo adendum)

### Assinatura
- Assinatura marca evolução como finalizada
- Timestamp salvo em `signed_at`
- Evolução assinada NÃO pode ser editada

### Permissões de Visualização
- Profissional: apenas suas evoluções
- Coordenador: todas evoluções da unidade
- Admin: todas evoluções
- Secretária: NÃO pode ver evoluções

---

## 📋 Avaliações (Assessments)

### Form Builder
- Coordenador e Admin podem criar templates
- Templates podem ser globais (unit_id null) ou por unidade/especialidade
- Campos suportados: texto, número, seleção única, seleção múltipla, texto longo

### Preenchimento
- Profissional cria avaliação a partir do template
- Todas as respostas salvadas em JSON no campo `answers`
- Avaliação integrada automaticamente no prontuário
- **SEM assinatura digital**

---

## 💰 Financeiro

### Registro Manual
- Secretária ou Admin registra pagamento/isento manualmente
- Campo amount null para isentos
- Vínculo opcional com agendamento
- Não há integração com tabela de valores

### Relatórios
- Total por mês
- Total por profissional
- Lista de isentos

---

## 🏢 Multi-Unidade

### Troca de Unidade
- Dropdown no header mostra unidades vinculadas ao usuário
- Ao trocar unidade, todos os dados filtrados por nova unidade
- Admin sempre vê dados de todas as unidades

### Policies
- Toda Policy deve verificar:
  ```php
  if ($user->isAdmin()) return true;
  if ($resource->unit_id === $user->active_unit_id) return true;
  return false;
  ```

---

## 📊 Relatórios

### Tipos
1. **Clínico**: Evoluções, avaliações, prontuários
2. **Pacientes**: Frequência, evolução temporal
3. **Profissionais**: Produtividade (atendimentos/mês, tempo médio)
4. **Atendimentos**: Ocupação de agenda, taxa de faltas

### Ocupação de Agenda
```
Ocupação = (Total agendamentos realizados / Total horários disponíveis) × 100
```

### Filtros
- Período: obrigatório
- Unidade: opcional (Admin pode filtrar)
- Profissional: opcional
- Status: opcional

---

## 📦 Estoque e Materiais

### Movimentação
- Entrada: aumenta estoque
- Saída: diminui estoque (vinculada a agendamento)
- Estoque não pode ficar negativo

### Alertas
- Quantidade atual <= quantidade mínima = alerta
- Alerta visível no dashboard

---

## 🔔 Notificações

### Tipos
- **Sistema**: Pendências, alertas
- **Clínica**: Comunicados da unidade
- **Mural**: Avisos gerais

### Regras
- Notificações de unidade visíveis para todos da unidade
- Notificações do sistema personalizadas por usuário
- Mark as read ao clicar

---

## 🗑️ Soft Deletes e LGPD

### Exclusão
- Todas as exclusões são soft deletes
- Dados permanecem no banco por 5 anos (conformidade)
- Admin pode restaurar registros excluídos
- Exclusão definitiva apenas após período de retenção

### Auditoria
- Laravel Auditing registra TODAS as ações
- Não pode ser desabilitado
- Logs incluem IP e user agent

---

## ⚠️ Regras de Negócio Críticas

### NÃO Permitir
- Deletar agendamento concluído
- Editar evolução assinada
- Secretária criar evoluções
- Profissional agendar fora da própria unidade
- Estoque negativo
- Usuário sem unidade ativa

### OBRIGATÓRIO
- Unit_id em TODAS as tabelas principais
- Validação de policies em todas as rotas
- Soft deletes em todas as entidades
- Auditoria automática
- Validação de conflitos de agenda

---

## 🚨 Validações Importantes

### Agendamento
```php
- scheduled_at não pode ser no passado (exceto check-in)
- Duração mínima: 15 minutos
- Duração máxima: 240 minutos
- Profissional deve ter especialidade adequada
```

### Paciente
```php
- CPF: 11 dígitos, formatado ou não
- Data nascimento: não pode ser futura
- Telefone: DDD + 9 dígitos
- Email: validação de formato
```

### Upload de Documentos
```php
- Tamanho máximo: 10MB
- Extensões permitidas: pdf, jpg, jpeg, png, doc, docx
- Máximo 50 arquivos por paciente
```

