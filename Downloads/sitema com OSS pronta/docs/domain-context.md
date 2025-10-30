# 🧠 Contexto de Domínio — Equidade+

## Sobre o Negócio

O **Equidade+** é um sistema de gestão clínica desenvolvido para clínicas multidisciplinares que atendem pessoas com:
- Deficiências (física, intelectual, múltiplas)
- TEA (Transtorno do Espectro Autista)
- TDAH (Transtorno de Déficit de Atenção e Hiperatividade)
- Outras condições de neurodiversidade

---

## Perfis de Usuários

### Psicólogo
- Realiza avaliações psicológicas e acompanhamento terapêutico
- Cria evoluções após cada sessão
- Preenche avaliações padronizadas
- Consulta prontuário do paciente

### Terapeuta Ocupacional (TO)
- Trabalha com atividades de vida diária
- Realiza avaliações funcionais
- Registra evoluções com foco em objetivos funcionais
- Trabalha com materiais específicos

### Fonoaudiólogo
- Atua na comunicação e linguagem
- Realiza avaliações de comunicação
- Registra evoluções focadas em desenvolvimento da fala
- Trabalha com pacientes TEA e outros

### Coordenador Clínico
- Supervisiona a equipe de profissionais
- Acompanha evoluções de todos os pacientes
- Gerencia escalas e horários
- Gera relatórios gerenciais

### Secretária
- Realiza agendamentos
- Faz check-in de pacientes
- Gerencia dados administrativos
- Registra pagamentos

### Administrador
- Gestão completa do sistema
- Acesso a todos os dados
- Configurações gerais
- Backups e auditoria

---

## Ciclo de Atendimento Clínico

### 1. Cadastro Inicial
- Paciente é cadastrado no sistema
- Dados completos coletados (anamnese)
- Documentos uploadados
- Avaliação inicial pode ser registrada

### 2. Agendamento
- Secretária agenda primeira sessão
- Profissional e horário definidos
- Recorrência configurada (semanal, quinzenal)

### 3. Check-in
- Ao chegar na clínica, secretária faz check-in
- Status muda para "check-in"
- Avisa o profissional que paciente chegou

### 4. Atendimento
- Profissional inicia atendimento
- Status muda para "em andamento"
- Trabalho terapêutico realizado

### 5. Finalização
- Atendimento concluído
- Status muda para "concluído"
- **Evolução é criada automaticamente** (status: pendente)

### 6. Evolução Clínica
- Profissional preenche evolução
- Campos: relato da sessão, plano terapêutico, objetivos
- Assinatura digital (timestamp)
- Evolução integra prontuário

### 7. Avaliações Periódicas
- Avaliações podem ser criadas a qualquer momento
- Form Builder permite criar formulários customizados
- Integram prontuário do paciente

---

## Terminologia Clínica

| Termo | Descrição | Contexto de Uso |
|-------|-----------|-----------------|
| **Paciente** | Pessoa atendida pela clínica | Cadastro principal |
| **Responsável** | Tutor legal ou familiar | Dados complementares |
| **Profissional** | Terapeuta que atende | Vínculo com usuário do sistema |
| **Evolução** | Registro clínico pós-atendimento | Documentação obrigatória |
| **Avaliação** | Formulário clínico estruturado | Documentação complementar |
| **Prontuário** | Histórico completo do paciente | Timeline consolidada |
| **Plano de Crise** | Protocolo para crises | Alerta automático |
| **Anamnese** | Histórico inicial do paciente | Questionário completo |

---

## Dados Sensíveis

### Informações Pessoais (LGPD)
- Nome completo
- RG/CPF
- Data de nascimento
- Endereço
- Telefone/Email
- Foto (se houver)

### Informações Médicas
- Diagnósticos
- Alergias
- Medicamentos em uso
- Histórico de atendimentos
- Evoluções clínicas
- Avaliações

### Documentos
- Laudos médicos
- Relatórios clínicos
- Atestados
- Anexos diversos

**Proteção:** Todos os dados devem ser protegidos conforme LGPD, com auditoria completa e soft deletes.

---

## Fluxos Importantes

### Fluxo de Agendamento Recorrente
1. Secretária cria primeiro agendamento
2. Define recorrência (ex: semanal, 4x)
3. Sistema cria automaticamente os próximos 3 agendamentos
4. Cada agendamento é independente
5. Cancelamento individual não afeta os demais

### Fluxo de Evolução Automática
1. Agendamento concluído
2. Sistema dispara evento `AppointmentCompleted`
3. Listener cria evolução com status "pendente"
4. Profissional recebe notificação
5. Preenche e assina evolução

### Fluxo de Lista de Espera
1. Secretária verifica disponibilidade
2. Se não há horário, adiciona à lista de espera
3. Quando horário libera, sistema notifica
4. Secretária liga e confirma com responsável

---

## Materiais e Estoque

### Tipos de Materiais
- **Recreativos**: Brinquedos, jogos, materiais artísticos
- **Terapêuticos**: Roletes, coletes, bolas terapêuticas
- **Educacionais**: Papéis, lápis, material didático
- **Higiênicos**: Papel toalha, álcool, etc.

### Uso em Sessões
- Profissional consome material durante atendimento
- Registro automático no sistema
- Estoque diminui
- Alertas quando estoque baixo

---

## Requisitos Éticos

### Confidencialidade
- Apenas profissionais da unidade veem dados dos pacientes
- Coordenador vê todas evoluções (supervisão)
- Logs de acesso são obrigatórios

### Sigilo Profissional
- Dados não podem ser compartilhados externamente
- Exportações só para fins clínicos
- Backup seguro e criptografado

### Consentimento
- Termo de uso deve ser aceito
- LGPD compliance
- Retenção de dados por 5 anos

---

## Terminologia Técnica

| Termo | Significado no Código |
|-------|----------------------|
| `unit_id` | ID da unidade (multi-tenant) |
| `active_unit_id` | Unidade atualmente selecionada pelo usuário |
| `Evolution` | Model de evolução clínica |
| `Assessment` | Model de avaliação |
| `Appointment` | Model de agendamento |
| `Patient` | Model de paciente |
| `Professional` | Model de profissional |
| `ScopeByUnit` | Middleware de filtro por unidade |

---

## Observações para IA

Ao desenvolver o sistema, SEMPRE considere:

1. **Contexto clínico**: Não é sistema comercial, é para saúde
2. **Precisão de dados**: Erros podem impactar tratamento
3. **Privacidade**: Dados extremamente sensíveis
4. **Simplicidade**: Profissionais precisam de interface intuitiva
5. **Auditoria**: Tudo deve ser rastreável
6. **Disponibilidade**: Sistema deve estar sempre online durante horário comercial

---

## Padrões de Código

- Use inglês para código (models, controllers, functions)
- Use português para interface (views, mensagens)
- Mantenha nomenclatura consistente com o contexto clínico
- Documente funções complexas
- Priorize segurança sobre performance (em dados sensíveis)


