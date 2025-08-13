# Canvas de Nível de Contexto - CheckIn

## Visão Geral
Este canvas define o contexto de negócio de alto nível do CheckIn, mapeando como o sistema interage com usuários externos, sistemas externos, e como se posiciona dentro do ecossistema mais amplo de tecnologia e negócios.

---

## 1. Contexto de Sistema - Visão Macro

### 1.1 Diagrama de Contexto
```
                           ┌─────────────────────────────────────────┐
                           │              CHECKIN SYSTEM             │
                           │                                         │
                           │  ┌─────────────┐    ┌─────────────┐     │
                           │  │Recommendation│    │   Social    │     │
                           │  │   Engine     │    │Coordination │     │
                           │  └─────────────┘    └─────────────┘     │
                           │                                         │
                           │  ┌─────────────┐    ┌─────────────┐     │
                           │  │   Venue     │    │   User      │     │
                           │  │ Management  │    │ Management  │     │
                           │  └─────────────┘    └─────────────┘     │
                           └─────────────────────────────────────────┘
                                              │
                     ┌────────────────────────┼────────────────────────┐
                     │                        │                        │
          ┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
          │    End Users    │      │  Establishments │      │ External Systems│
          │                 │      │                 │      │                 │
          │ • App Users     │      │ • Restaurants   │      │ • Google Places │
          │ • Social Groups │      │ • Bars & Cafes  │      │ • OpenAI API    │
          │ • Event Org.    │      │ • Event Venues  │      │ • Weather API   │
          └─────────────────┘      └─────────────────┘      └─────────────────┘
```

### 1.2 Stakeholders e Relacionamentos

| Stakeholder | Relacionamento | Expectativa do CheckIn | CheckIn entrega |
|-------------|----------------|----------------------|-----------------|
| **App Users** | Consumidores primários | Decisões rápidas e precisas sobre onde sair | Recomendações personalizadas via IA |
| **Establishments** | Parceiros de negócio | Mais clientes e insights | Visibilidade e analytics |
| **Google Places** | Fornecedor de dados | Uso dentro dos ToS | Dados enriquecidos de locais |
| **OpenAI** | Provedor de IA | Conformidade com usage policies | Processamento inteligente de contexto |
| **Payment Providers** | Facilitadores financeiros | Transações seguras | Processamento de pagamentos B2B |
| **Government/Regulators** | Autoridades | Compliance com LGPD | Proteção de dados e privacidade |

---

## 2. Personas e Casos de Uso Contextuais

### 2.1 Persona Primária: Julia (Usuario Mobile)

#### **Contexto de Uso**:
- **Quando**: Sexta-feira 19h, saindo do trabalho
- **Onde**: Centro de São Paulo, no metrô
- **Com quem**: Namorado Pedro
- **Estado mental**: Cansada, quer algo especial mas sem stress
- **Dispositivo**: iPhone 12, conexão 4G instável
- **Tempo disponível**: 5 minutos antes do metrô chegar

#### **Journey no Sistema**:
```
1. Abre app no metrô → Tap "Quero sair"
2. IA pergunta: "Que vibe você quer hoje?"
3. Julia: "Romântico, não muito caro, perto da Vila Madalena"
4. Sistema processa → 3 sugestões com explicação
5. Julia escolhe "Bistrô do Chef" → Compartilha com Pedro
6. Pedro aceita → Sistema facilita coordenação
7. Chegam no local → Check-in confirma boa escolha
```

#### **Expectativas de Contexto**:
- Sistema entende linguagem natural casual
- Recomendações consideram trânsito atual
- Interface funciona bem em movimento (metrô)
- Baixo consumo de dados móveis

### 2.2 Persona Secundária: Carlos (Pai Social)

#### **Contexto de Uso**:
- **Quando**: Sábado 17h, em casa
- **Onde**: Zona Oeste, com família
- **Com quem**: Esposa Fernanda e filho Miguel (6 anos)
- **Estado mental**: Quer sair em família, socializar com outros pais
- **Dispositivo**: iPad, WiFi casa
- **Tempo disponível**: 15 minutos para planejar

#### **Journey no Sistema**:
```
1. Abre app → "Quero sair em família"
2. Sistema sugere locais family-friendly
3. Carlos vê que família Silva está no Parque Burger
4. Envia mensagem → "Vocês estão aí? Podemos nos juntar?"
5. Recebe confirmação → Organiza encontro
6. Check-in conjunto → Crianças brincam, pais conversam
```

### 2.3 Estabelecimento: Bistrô do Chef

#### **Contexto de Negócio**:
- **Tipo**: Restaurante francês, 40 lugares
- **Localização**: Vila Madalena, rua movimentada
- **Público**: Casais 25-40 anos, classe média alta
- **Problemas**: Dificuldade para comunicar vibe, lotação irregular
- **Objetivo**: Atrair clientes no perfil, evitar incompatibilidade

#### **Journey no Sistema**:
```
1. Cadastro gratuito → Perfil básico ativo
2. Recebe clientes via CheckIn → Experiência positiva
3. Vê analytics → Entende perfil dos clientes CheckIn
4. Upgrade para Premium → Acesso a dashboard avançado
5. Otimiza oferta → Mais clientes satisfeitos
```

---

## 3. Sistemas Externos e Integrações

### 3.1 Google Places API

#### **Contexto de Integração**:
- **Propósito**: Dados básicos de estabelecimentos
- **Dados consumidos**: Nome, endereço, horário, telefone, photos
- **Frequência**: Real-time para novos locais, cache para existentes
- **Rate limits**: 1000 requests/day no free tier
- **Fallback**: Dados manuais quando API indisponível

#### **Padrão de Uso**:
```python
class GooglePlacesService:
    async def enrich_venue_data(self, venue: Venue) -> EnrichedVenue:
        try:
            places_data = await self.google_client.get_place_details(venue.place_id)
            return self.merge_data(venue, places_data)
        except RateLimitExceeded:
            # Fallback to cached data
            return self.get_cached_venue_data(venue.id)
        except APIError:
            # Continue with local data only
            return venue
```

### 3.2 OpenAI API

#### **Contexto de Integração**:
- **Propósito**: Processamento de linguagem natural para recomendações
- **Modelos**: GPT-4 para conversação, text-embedding para similaridade
- **Usage patterns**: Batch processing quando possível
- **Cost management**: Caching agressivo, prompt optimization
- **Fallback**: Recomendações baseadas em regras

#### **Controle de Contexto**:
```python
class AIContextManager:
    def __init__(self):
        self.conversation_memory = {}
        self.cost_tracker = CostTracker()
    
    async def process_user_input(self, user_id: str, input: str) -> AIResponse:
        # Check cost limits
        if await self.cost_tracker.exceeds_daily_limit(user_id):
            return await self.fallback_recommendation(input)
        
        # Maintain conversation context
        context = self.conversation_memory.get(user_id, [])
        context.append({"role": "user", "content": input})
        
        # Call OpenAI with context
        response = await self.openai_client.chat.completions.create(
            model="gpt-4",
            messages=context,
            max_tokens=500
        )
        
        # Track costs and update context
        await self.cost_tracker.record_usage(user_id, response.usage)
        context.append({"role": "assistant", "content": response.choices[0].message.content})
        self.conversation_memory[user_id] = context[-10:]  # Keep last 10 messages
        
        return self.parse_ai_response(response)
```

### 3.3 Weather API (OpenWeatherMap)

#### **Contexto de Integração**:
- **Propósito**: Contextualizar recomendações baseadas no clima
- **Dados consumidos**: Temperatura atual, previsão, precipitação
- **Impacto**: Sugestões de locais cobertos vs. ao ar livre
- **Cache**: 15 minutos para dados atuais

### 3.4 Payment Systems (Stripe)

#### **Contexto de Integração**:
- **Propósito**: Cobranças de estabelecimentos premium
- **Scope**: B2B subscriptions, não pagamentos de consumo
- **Compliance**: PCI DSS através do Stripe
- **Webhooks**: Eventos de pagamento para atualizar status

---

## 4. Arquitetura de Contexto

### 4.1 Bounded Contexts Identification

#### **Core Domain: Discovery & Recommendation**
```
┌─────────────────────────────────────────┐
│        RECOMMENDATION CONTEXT           │
│                                         │
│ Entities:                               │
│ • UserContext                           │
│ • Suggestion                            │
│ • RecommendationRequest                 │
│ • AIResponse                            │
│                                         │
│ Services:                               │
│ • ContextProcessor                      │
│ • RecommendationEngine                  │
│ • PersonalizationService                │
│                                         │
│ External Dependencies:                  │
│ • OpenAI API                            │
│ • User Preferences                      │
│ • Venue Data                            │
└─────────────────────────────────────────┘
```

#### **Supporting Domain: Social Coordination**
```
┌─────────────────────────────────────────┐
│         SOCIAL CONTEXT                  │
│                                         │
│ Entities:                               │
│ • SocialEvent                           │
│ • Invitation                            │
│ • RSVP                                  │
│ • SocialGroup                           │
│                                         │
│ Services:                               │
│ • EventManager                          │
│ • NotificationService                   │
│ • GroupCoordinator                      │
│                                         │
│ External Dependencies:                  │
│ • Push Notification Services            │
│ • Email Service                         │
│ • User Directory                        │
└─────────────────────────────────────────┘
```

#### **Supporting Domain: Venue Management**
```
┌─────────────────────────────────────────┐
│          VENUE CONTEXT                  │
│                                         │
│ Entities:                               │
│ • Venue                                 │
│ • VenueProfile                          │
│ • OperatingHours                        │
│ • VibeTag                               │
│                                         │
│ Services:                               │
│ • VenueEnrichmentService                │
│ • DataSyncService                       │
│ • QualityAssuranceService               │
│                                         │
│ External Dependencies:                  │
│ • Google Places API                     │
│ • Venue Owner Portal                    │
│ • Photo Storage Service                 │
└─────────────────────────────────────────┘
```

### 4.2 Context Integration Patterns

#### **Anti-Corruption Layer for External APIs**
```python
class GooglePlacesAdapter:
    """Anti-corruption layer for Google Places API"""
    
    async def get_venue_data(self, place_id: str) -> VenueData:
        # Get data from Google
        google_data = await self.google_client.get_place_details(place_id)
        
        # Transform to internal model
        return VenueData(
            id=self.generate_internal_id(place_id),
            name=google_data.get("name"),
            address=self.parse_address(google_data.get("formatted_address")),
            category=self.map_google_types(google_data.get("types", [])),
            coordinates=self.extract_coordinates(google_data.get("geometry")),
            rating=google_data.get("rating"),
            # Filter and transform other fields
        )
    
    def map_google_types(self, google_types: List[str]) -> VenueCategory:
        """Map Google's types to internal categories"""
        type_mapping = {
            "restaurant": VenueCategory.RESTAURANT,
            "bar": VenueCategory.BAR,
            "cafe": VenueCategory.CAFE,
            "night_club": VenueCategory.NIGHTCLUB,
        }
        
        for google_type in google_types:
            if google_type in type_mapping:
                return type_mapping[google_type]
        
        return VenueCategory.OTHER
```

#### **Event-Driven Communication Between Contexts**
```python
class DomainEventBus:
    def __init__(self):
        self.handlers = defaultdict(list)
    
    def subscribe(self, event_type: Type[DomainEvent], handler: Callable):
        self.handlers[event_type].append(handler)
    
    async def publish(self, event: DomainEvent):
        for handler in self.handlers[type(event)]:
            try:
                await handler(event)
            except Exception as e:
                logger.error(f"Error handling event {event}: {e}")

# Usage across contexts
@event_handler
async def update_recommendation_model(event: UserCheckInCompleted):
    """Update recommendation model when user checks in"""
    await recommendation_service.update_user_preferences(
        event.user_id, 
        event.venue_preferences
    )

@event_handler
async def send_social_notification(event: EventCreated):
    """Send notifications when social event is created"""
    await notification_service.notify_invitees(
        event.event_id,
        event.invited_users
    )
```

---

## 5. Contexto de Segurança e Compliance

### 5.1 LGPD Compliance Context

#### **Data Protection Mapping**:
```
┌─────────────────────────────────────────┐
│           USER DATA CONTEXT             │
│                                         │
│ Personal Data (LGPD Article 5):        │
│ • Name, email (identification)          │
│ • Location data (sensitive)             │
│ • Behavioral patterns (profiling)       │
│ • Social connections (relationship)     │
│                                         │
│ Legal Basis (LGPD Article 7):          │
│ • Consent for location tracking         │
│ • Legitimate interest for service       │
│ • Contract performance for premium      │
│                                         │
│ Rights Implementation:                  │
│ • Access: User dashboard               │
│ • Portability: Data export             │
│ • Deletion: Account deletion           │
│ • Correction: Profile editing          │
└─────────────────────────────────────────┘
```

#### **Privacy by Design Implementation**:
```python
class PrivacyManager:
    async def collect_user_data(self, user: User, data: UserData) -> bool:
        # Check consent for data type
        if not await self.has_consent(user.id, data.type):
            raise ConsentRequiredError(f"Consent required for {data.type}")
        
        # Minimize data collection
        minimized_data = self.minimize_data(data)
        
        # Encrypt sensitive data
        if data.is_sensitive():
            minimized_data = await self.encrypt_data(minimized_data)
        
        # Log data collection for audit
        await self.audit_log.record_data_collection(user.id, data.type)
        
        return await self.store_data(user.id, minimized_data)
    
    async def delete_user_data(self, user_id: str) -> bool:
        """LGPD Article 18 - Right to deletion"""
        try:
            # Delete from all systems
            await self.delete_from_primary_db(user_id)
            await self.delete_from_cache(user_id)
            await self.delete_from_analytics(user_id)
            await self.delete_from_logs(user_id)
            
            # Verify deletion
            return await self.verify_complete_deletion(user_id)
        except Exception as e:
            await self.audit_log.record_deletion_failure(user_id, str(e))
            raise
```

### 5.2 Security Context

#### **Authentication & Authorization Context**:
```python
class SecurityContext:
    def __init__(self, user: User, permissions: List[Permission]):
        self.user = user
        self.permissions = permissions
        self.session_id = str(uuid4())
        self.created_at = datetime.utcnow()
    
    def can_access_venue_data(self, venue_id: str) -> bool:
        """Check if user can access venue data"""
        if self.user.is_venue_owner(venue_id):
            return True
        
        if Permission.VIEW_PUBLIC_VENUES in self.permissions:
            return True
        
        return False
    
    def can_see_user_location(self, target_user_id: str) -> bool:
        """Check if user can see another user's location"""
        if self.user.id == target_user_id:
            return True
        
        # Check friendship and privacy settings
        return (
            self.user.is_friend_with(target_user_id) and
            self.user.allows_location_sharing_with_friends()
        )
```

---

## 6. Contexto de Performance

### 6.1 Performance Context per User Type

#### **High-Frequency Users (Julia-type)**:
- **Response time requirement**: < 2 seconds for recommendations
- **Caching strategy**: Aggressive caching of user preferences
- **Optimization**: Pre-computed recommendations for common contexts

#### **Family Users (Carlos-type)**:
- **Response time requirement**: < 5 seconds (more tolerance)
- **Caching strategy**: Cache family-friendly venues by location
- **Optimization**: Batch processing for group coordination

#### **Business Users (Establishment owners)**:
- **Response time requirement**: < 10 seconds for analytics
- **Caching strategy**: Dashboard data cached for 1 hour
- **Optimization**: Background processing for heavy analytics

### 6.2 Scalability Context

#### **Geographic Scaling**:
```python
class GeographicContext:
    def __init__(self, region: str):
        self.region = region
        self.db_shard = self.get_db_shard(region)
        self.cache_cluster = self.get_cache_cluster(region)
        self.ai_endpoint = self.get_regional_ai_endpoint(region)
    
    async def get_recommendations(self, context: UserContext) -> List[Suggestion]:
        # Route to appropriate regional services
        venue_service = VenueService(self.db_shard)
        ai_service = AIService(self.ai_endpoint)
        
        venues = await venue_service.find_nearby(context.location)
        return await ai_service.generate_recommendations(context, venues)
```

---

## 7. Contexto de Monitoramento

### 7.1 Business Context Monitoring

#### **User Experience Metrics**:
```python
class BusinessMetrics:
    async def track_recommendation_success(
        self, 
        user_id: str, 
        context: UserContext,
        suggestions: List[Suggestion],
        user_action: UserAction
    ):
        metrics = {
            "user_id": user_id,
            "context_type": context.companion_type,
            "time_of_day": context.time.hour,
            "location_type": context.location.area_type,
            "suggestions_count": len(suggestions),
            "user_action": user_action.value,
            "response_time": user_action.response_time_seconds,
            "satisfaction_score": user_action.satisfaction_score
        }
        
        await self.analytics.track("recommendation_interaction", metrics)
        
        # Business intelligence aggregation
        if user_action == UserAction.ACCEPTED:
            await self.bi_service.increment_conversion_rate(context.companion_type)
        elif user_action == UserAction.REJECTED:
            await self.bi_service.analyze_rejection_pattern(context, suggestions)
```

### 7.2 Technical Context Monitoring

#### **System Health per Context**:
```python
class SystemHealthMonitor:
    async def check_context_health(self) -> ContextHealthReport:
        return ContextHealthReport(
            recommendation_context=await self.check_recommendation_health(),
            social_context=await self.check_social_health(),
            venue_context=await self.check_venue_health(),
            external_integrations=await self.check_external_health()
        )
    
    async def check_recommendation_health(self) -> ContextHealth:
        return ContextHealth(
            response_time=await self.measure_avg_response_time("recommendations"),
            error_rate=await self.calculate_error_rate("recommendations"),
            ai_api_health=await self.check_openai_health(),
            cache_hit_rate=await self.get_cache_metrics("recommendations")
        )
```

---

## 8. Contexto de Evolução

### 8.1 Context Evolution Roadmap

#### **Current Context (MVP)**:
- Core recommendation engine
- Basic social coordination
- Simple venue management
- Manual data entry heavy

#### **6-Month Context**:
- Machine learning enhanced recommendations
- Advanced social features (groups, events)
- Automated venue data sync
- Basic business analytics

#### **12-Month Context**:
- Multi-city expansion
- Predictive recommendations
- Advanced business intelligence
- API ecosystem for partners

#### **24-Month Context**:
- International expansion
- AI-powered venue matching
- Full business automation
- Platform for third-party developers

### 8.2 Context Migration Strategy

#### **Data Context Migration**:
```python
class ContextMigrationManager:
    async def migrate_user_context(self, from_version: str, to_version: str):
        """Migrate user context between system versions"""
        
        migration_plan = self.get_migration_plan(from_version, to_version)
        
        for step in migration_plan.steps:
            try:
                await self.execute_migration_step(step)
                await self.verify_migration_step(step)
            except MigrationError as e:
                await self.rollback_migration_step(step)
                raise MigrationFailedError(f"Migration failed at step {step.name}: {e}")
        
        await self.finalize_migration(to_version)
```

---

