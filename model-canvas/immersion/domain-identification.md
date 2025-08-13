# Canvas de Identificação do Domínio

## Visão Geral
Este canvas define e mapeia o domínio de atuação do CheckIn, estabelecendo fronteiras claras, identificando subdomínios críticos e mapeando as regras de negócio que governam cada área da plataforma social de check-in.

---

### 1. Nome do Domínio

- Descoberta Social Contextual e Coordenação de Saídas em Estabelecimentos

### 2. Descrição do Domínio

- Plataforma que ajuda pessoas a decidirem “para onde sair agora” com base em contexto pessoal (horário, localização, companhia, orçamento, restrições), sinais sociais (amigos/grupos, convites, RSVPs) e informações de estabelecimentos (catálogo inicial via TripAdvisor), com suporte a check-in/avaliações e privacidade controlada.

---

### 3. Justificativa da Escolha

- Alto impacto no problema de paralisia de escolha e frustração com rolês inadequados.
- Relevância de negócio: aumenta engajamento, retenção e cria valor B2B para estabelecimentos (visibilidade, fluxo, reservas no futuro).
- Viabilidade técnica: stack atual (React + FastAPI), dados de venues (TripAdvisor), sinais internos (check-ins/avaliações) e LLM para curadoria explicável.

---

## 4. Problemas/Desafios Atuais

### 4.1 Principais Problemas
- Fragmentação de fontes e falta de contexto em tempo real (vibe, lotação, presença de amigos).
- Informações desatualizadas e heterogêneas; normalização e deduplicação de venues (TripAdvisor x internos).
- Privacidade e consentimento de presença/visibilidade; configurações granulares.
- Cold start de dados para novas regiões/usuários; escassez inicial de sinais sociais.
- Coordenação social trabalhosa (convites/RSVP) e baixa taxa de confirmação.
- Constrangimentos de ToS/licenciamento de dados externos; quotas e latência móvel.

---

## 5. Oportunidades de IA Generativa

### 5.1 Onde aplicar IA
- Assistente conversacional para coleta de contexto e geração de 2–3 sugestões com explicação.
- Re-ranking explicável do motor de recomendação (contexto + sinais sociais + LLM).
- Sumarização/normalização semântica de descrições e reviews de venues (TripAdvisor) para “vibe”.
- Geração de convites/mensagens contextuais e sugestão de locais compatíveis ao grupo.
- Criação de “roteiros leves” (ex.: jantar → bar) quando pertinente.
- Coleta de feedback leve (salvar/rejeitar) e aprendizado contínuo.

---

## 6. Benefícios Esperados

- Redução do tempo até decisão (objetivo ≤ 2 min no fluxo “Quero sair”).
- Maior assertividade e satisfação com os rolês; aumento de check-ins/RSVPs.
- Engajamento social e recorrência; efeito de rede.
- Valor para estabelecimentos (descoberta qualificada) e base de dados rica para curadoria.
- Diferencial competitivo: descoberta com contexto e explicabilidade.
**Responsabilidade**: Gerar sugestões personalizadas baseadas em contexto

**Entidades Principais**:
- `RecommendationRequest` (contexto do usuário)
- `Suggestion` (recomendação gerada)
- `UserContext` (situação atual)
- `PreferenceProfile` (histórico e preferências)

**Regras de Negócio**:
- Sugestões devem considerar localização (raio configurável)
- Horário determina adequação do estabelecimento
- Preferências alimentares são obrigatórias quando aplicável
- Histórico negativo reduz probabilidade de sugestão similar

**Linguagem Ubíqua**:
- **"Contexto"**: Situação atual do usuário (onde, quando, com quem)
- **"Vibe"**: Energia/ambiente de um estabelecimento 
- **"Sugestão"**: Recomendação gerada pela IA
- **"Match"**: Adequação entre contexto e estabelecimento

### 2.2 Subdomínio Core: Social Coordination
**Responsabilidade**: Facilitar organização e coordenação de experiências sociais

**Entidades Principais**:
- `Event` (encontro planejado)
- `Invitation` (convite para participar)
- `RSVP` (confirmação de presença)
- `SocialGroup` (grupo de pessoas)

**Regras de Negócio**:
- Eventos devem ter ao menos um organizador
- Convites expiram após tempo determinado
- RSVP pode ser alterado até deadline definido
- Grupos têm limite máximo de participantes
- Privacidade de presença é configurável por usuário

**Linguagem Ubíqua**:
- **"Evento"**: Encontro organizado em local específico
- **"RSVP"**: Confirmação de presença (vai/não vai/talvez)
- **"Organizador"**: Pessoa que criou o evento
- **"Deadline"**: Prazo limite para confirmação

### 2.3 Subdomínio Supporting: Check-in Management
**Responsabilidade**: Gerenciar presença e experiência em estabelecimentos

**Entidades Principais**:
- `CheckIn` (registro de presença)
- `Experience` (avaliação da visita)
- `VibeTag` (tag de ambiente em tempo real)
- `Stay` (duração da permanência)

**Regras de Negócio**:
- Usuário pode ter apenas um check-in ativo
- Check-in expira automaticamente após 8 horas sem atividade
- Avaliação só pode ser feita após check-out
- Tags de vibe têm validade temporal (2 horas)
- Check-in pode ser privado ou público

**Linguagem Ubíqua**:
- **"Check-in"**: Registro de chegada ao estabelecimento
- **"Check-out"**: Registro de saída (manual ou automático)
- **"Vibe Tags"**: Etiquetas descrevendo ambiente atual
- **"Experiência"**: Avaliação completa da visita

### 2.4 Subdomínio Supporting: Venue Management
**Responsabilidade**: Manter informações atualizadas sobre estabelecimentos

**Entidades Principais**:
- `Venue` (estabelecimento)
- `VenueProfile` (informações detalhadas)
- `OperatingHours` (horário de funcionamento)
- `Capacity` (capacidade e disponibilidade)

**Regras de Negócio**:
- Estabelecimentos devem ter localização válida
- Horários podem ter exceções (feriados, eventos especiais)
- Capacidade pode ser estimada ou exata
- Informações desatualizadas são sinalizadas automaticamente
- Estabelecimentos inativos por 90 dias são marcados como suspensos

**Linguagem Ubíqua**:
- **"Venue"**: Estabelecimento físico (bar, restaurante, café)
- **"Capacidade"**: Número máximo de pessoas
- **"Horário de Pico"**: Período de maior movimento
- **"Status Operacional"**: Aberto/fechado/suspenso

### 2.5 Subdomínio Supporting: User Identity & Preferences
**Responsabilidade**: Gerenciar identidade, perfis e preferências dos usuários

**Entidades Principais**:
- `User` (usuário da plataforma)
- `UserProfile` (perfil público)
- `PreferenceSettings` (configurações pessoais)
- `PrivacySettings` (configurações de privacidade)

**Regras de Negócio**:
- Email é identificador único obrigatório
- Perfil público pode ser limitado por configurações
- Preferências afetam algoritmo de recomendação
- Dados pessoais seguem rigorosamente LGPD
- Usuário pode deletar conta e todos os dados

**Linguagem Ubíqua**:
- **"Perfil"**: Informações visíveis para outros usuários
- **"Preferências"**: Configurações que afetam recomendações
- **"Privacidade"**: Controle sobre visibilidade de dados
- **"Conexão"**: Relacionamento entre usuários (amigos)

### 2.6 Subdomínio Generic: Notification System
**Responsabilidade**: Gerenciar todas as comunicações com usuários

**Entidades Principais**:
- `Notification` (notificação)
- `NotificationChannel` (canal de entrega)
- `NotificationPreference` (preferências de recebimento)

**Regras de Negócio**:
- Usuários podem optar por não receber notificações
- Notificações urgentes sempre são enviadas
- Frequência é limitada para evitar spam
- Notificações expiram após tempo determinado

---

## 3. Mapeamento de Contextos Limitados

### 3.1 Context Map - Relacionamentos entre Subdomínios

```
┌─────────────────────┐    ┌─────────────────────┐
│  Recommendation     │◄──►│  Social             │
│  Engine             │    │  Coordination       │
└─────────────────────┘    └─────────────────────┘
           │                           │
           │                           │
           ▼                           ▼
┌─────────────────────┐    ┌─────────────────────┐
│  User Identity &    │◄──►│  Check-in           │
│  Preferences        │    │  Management         │
└─────────────────────┘    └─────────────────────┘
           │                           │
           │                           │
           ▼                           ▼
┌─────────────────────┐    ┌─────────────────────┐
│  Venue Management   │◄──►│  Notification       │
│                     │    │  System             │
└─────────────────────┘    └─────────────────────┘
```

### 3.2 Padrões de Integração

#### **Shared Kernel**: User Identity & Preferences ↔ Todos os contextos
- Conceitos compartilhados: `UserId`, `UserPreferences`, `PrivacyLevel`
- Mudanças devem ser coordenadas entre equipes
- Versionamento cuidadoso de interfaces

#### **Customer-Supplier**: Recommendation Engine → Social Coordination
- Engine fornece sugestões para eventos sociais
- Social Coordination não pode alterar algoritmo de recomendação
- SLA definido para tempo de resposta

#### **Conformist**: Notification System ← Todos os contextos
- Sistema de notificação consome eventos de todos os outros
- Não influencia regras de negócio dos outros contextos
- Implementa apenas lógica de entrega

#### **Anti-Corruption Layer**: Venue Management ↔ External APIs
- Protege modelo interno de mudanças em APIs externas
- Traduz dados de Google Places, Foursquare, etc.
- Mantém consistência mesmo com falhas externas

---

## 4. Eventos de Domínio

### 4.1 Eventos Críticos do Negócio

#### **Recommendation Engine**
- `SuggestionRequested`: Usuário solicita recomendações
- `SuggestionGenerated`: IA gera lista de sugestões
- `SuggestionAccepted`: Usuário aceita sugestão
- `SuggestionRejected`: Usuário rejeita sugestão
- `FeedbackReceived`: Usuário avalia qualidade da sugestão

#### **Social Coordination**
- `EventCreated`: Novo evento criado
- `InvitationSent`: Convite enviado para usuário
- `RSVPReceived`: Confirmação de presença recebida
- `EventCancelled`: Evento cancelado pelo organizador
- `EventCompleted`: Evento finalizado com sucesso

#### **Check-in Management**
- `CheckInStarted`: Usuário faz check-in em estabelecimento
- `CheckInCompleted`: Check-in finalizado (check-out)
- `ExperienceRated`: Usuário avalia experiência
- `VibeTagAdded`: Tag de ambiente adicionada
- `StayExtended`: Permanência no local estendida

#### **Venue Management**
- `VenueRegistered`: Novo estabelecimento cadastrado
- `VenueUpdated`: Informações do estabelecimento atualizadas
- `VenueStatusChanged`: Status operacional alterado
- `CapacityUpdated`: Informação de capacidade atualizada
- `VenueVerified`: Estabelecimento verificado por moderação

### 4.2 Fluxo de Eventos - Cenário Típico

```
1. UserRequestedSuggestion (contexto: sozinho, jantar, centro)
   ↓
2. RecommendationEngineProcessed (3 sugestões geradas)
   ↓
3. SuggestionAccepted (usuário escolhe Bistrô do Chef)
   ↓
4. CheckInStarted (usuário chega no local)
   ↓
5. VibeTagAdded (usuário marca como "romântico, tranquilo")
   ↓
6. ExperienceRated (5 estrelas, comentário positivo)
   ↓
7. CheckInCompleted (usuário sai do local)
   ↓
8. UserPreferencesUpdated (algoritmo aprende preferência por bistrôs)
```

---

## 5. Agregados e Invariantes

### 5.1 Agregado: Recommendation Session
**Root**: `RecommendationSession`
**Entidades**: `SuggestionRequest`, `GeneratedSuggestion`, `UserFeedback`

**Invariantes**:
- Uma sessão deve ter pelo menos uma sugestão
- Feedback só pode ser dado para sugestões da mesma sessão
- Sessão expira em 24 horas se não houver interação

### 5.2 Agregado: Social Event
**Root**: `SocialEvent`
**Entidades**: `EventDetails`, `Invitation`, `RSVP`, `Attendee`

**Invariantes**:
- Evento deve ter pelo menos um organizador
- Número de RSVPs confirmados não pode exceder capacidade do local
- Convites só podem ser enviados antes do deadline
- Organizador não pode cancelar evento com menos de 2 horas de antecedência

### 5.3 Agregado: Venue Profile
**Root**: `Venue`
**Entidades**: `VenueDetails`, `OperatingSchedule`, `CapacityInfo`, `ContactInfo`

**Invariantes**:
- Venue deve ter localização válida (lat/lng)
- Horário de funcionamento deve ser consistente
- Capacidade, se definida, deve ser número positivo
- Informações de contato devem ser verificáveis

### 5.4 Agregado: User Check-in
**Root**: `CheckInSession`
**Entidades**: `CheckInDetails`, `StayDuration`, `Experience`, `VibeContribution`

**Invariantes**:
- Usuário pode ter apenas um check-in ativo
- Check-in deve estar associado a venue válido
- Experiência só pode ser avaliada após check-out
- Tags de vibe devem ser relevantes ao estabelecimento

---

## 6. Serviços de Domínio

### 6.1 RecommendationService
**Responsabilidade**: Orquestrar geração de recomendações

**Operações**:
- `generateRecommendations(context: UserContext): List<Suggestion>`
- `refineBasedOnFeedback(feedback: UserFeedback): void`
- `adaptToGroupContext(users: List<User>): GroupRecommendation`

### 6.2 SocialCoordinationService
**Responsabilidade**: Facilitar organização de eventos sociais

**Operações**:
- `createEvent(details: EventDetails, organizer: User): SocialEvent`
- `sendInvitations(event: SocialEvent, invitees: List<User>): void`
- `processRSVP(event: SocialEvent, user: User, response: RSVPResponse): void`

### 6.3 CheckInService
**Responsabilidade**: Gerenciar ciclo de vida de check-ins

**Operações**:
- `startCheckIn(user: User, venue: Venue): CheckInSession`
- `updateVibeInfo(checkIn: CheckInSession, tags: List<VibeTag>): void`
- `completeCheckIn(checkIn: CheckInSession, experience: Experience): void`

---

## 7. Especificações e Políticas

### 7.1 Política de Recomendação
```
Uma recomendação é válida quando:
- Estabelecimento está aberto no horário solicitado
- Distância está dentro do raio preferido do usuário
- Tipo de estabelecimento alinha com contexto (ex: romântico para casal)
- Não foi rejeitada pelo usuário nas últimas 2 semanas
- Tem avaliação média >= 3.5 estrelas ou é novo com potencial
```

### 7.2 Política de Privacidade Social
```
Informações de presença são visíveis quando:
- Usuário optou por check-in público OU
- Visualizador está na lista de amigos E usuário permite amigos OU
- Visualizador está no mesmo evento confirmado
```

### 7.3 Política de Qualidade de Dados
```
Dados de estabelecimento são considerados válidos quando:
- Informações foram verificadas nos últimos 30 dias OU
- Estabelecimento tem parceria ativa OU
- Recebeu check-ins confirmados nos últimos 7 dias
```

---

## 8. Linguagem Ubíqua Consolidada

### 8.1 Termos Principais
| Termo | Definição | Contexto |
|-------|-----------|----------|
| **Contexto** | Situação atual do usuário (localização, horário, companhia, humor) | Recommendation |
| **Sugestão** | Recomendação de local gerada pela IA | Recommendation |
| **Vibe** | Energia/ambiente atual de um estabelecimento | Check-in, Venue |
| **Check-in** | Registro de presença em estabelecimento | Check-in |
| **RSVP** | Confirmação de presença em evento (vai/não vai/talvez) | Social |
| **Match** | Adequação entre contexto do usuário e estabelecimento | Recommendation |
| **Experience** | Avaliação completa da visita a um estabelecimento | Check-in |
| **Venue** | Estabelecimento físico (bar, restaurante, café) | Venue |

### 8.2 Verbos de Ação
| Verbo | Significado | Contexto |
|-------|-------------|----------|
| **Recomendar** | IA gera sugestões baseadas em contexto | Recommendation |
| **Check-in** | Registrar presença em estabelecimento | Check-in |
| **Convidar** | Enviar convite para evento social | Social |
| **RSVP** | Confirmar presença em evento | Social |
| **Avaliar** | Dar feedback sobre experiência | Check-in |
| **Curar** | Selecionar e recomendar manualmente | Venue |
| **Matchear** | Encontrar adequação entre contexto e local | Recommendation |

---

## 9. Fronteiras e Responsabilidades

### 9.1 O que está DENTRO do domínio CheckIn
- ✅ Recomendação contextual de locais
- ✅ Coordenação social para saídas
- ✅ Check-in e avaliação de experiências
- ✅ Gestão de perfis e preferências sociais
- ✅ Curadoria de informações de estabelecimentos
- ✅ Notificações sobre atividades sociais

### 9.2 O que está FORA do domínio CheckIn
- ❌ Processamento de pagamentos
- ❌ Delivery de produtos
- ❌ Transporte para locais
- ❌ Gestão interna de estabelecimentos
- ❌ Sistema de reviews detalhados de comida
- ❌ Hospedagem ou acomodação

### 9.3 Integrações Necessárias (Anti-Corruption Layers)
- 🔗 APIs de mapas (Google Maps, Mapbox)
- 🔗 Sistemas de pagamento (futuro)
- 🔗 APIs de estabelecimentos (Google Places)
- 🔗 Serviços de notificação (push notifications)
- 🔗 Sistemas de autenticação (OAuth providers)

---

## 10. Evolução do Domínio

### 10.1 Domínios Futuros Identificados
#### **Event Discovery** (6-12 meses)
- Descoberta de eventos públicos na cidade
- Integração com Eventbrite, Facebook Events
- Recomendação de eventos baseada em interesses

#### **Group Dynamics** (12+ meses)
- Análise de dinâmicas de grupo
- Sugestões para grupos com preferências diversas
- Facilitação de decisões coletivas

#### **Venue Intelligence** (12+ meses)
- Analytics avançados para estabelecimentos
- Previsão de demanda e otimização
- Insights sobre comportamento de clientes

### 10.2 Evolução de Subdomínios Existentes
#### **Recommendation Engine → Hyper-Personalization**
- Machine learning avançado
- Consideração de dados biométricos (humor via wearables)
- Recomendações proativas baseadas em padrões

#### **Social Coordination → Community Building**
- Formação automática de comunidades
- Eventos recorrentes e tradições
- Gamificação de interações sociais

---
