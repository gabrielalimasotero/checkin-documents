# Canvas de Planejamento de Expansão Geográfica e de Domínio - CheckIn

## Visão Geral
Este canvas define a estratégia detalhada de expansão do CheckIn tanto geograficamente (novas cidades/países) quanto por domínio (novos segmentos e verticais), estabelecendo frameworks de execução, métricas de sucesso e cronogramas realistas.

---

## 1. Análise de Expansão Geográfica

### 1.1 Framework de Avaliação de Mercados

#### **Critérios de Seleção de Cidades**
```python
class MarketEvaluationFramework:
    def __init__(self):
        self.evaluation_criteria = {
            'market_size': {
                'population': 0.25,           # 25% peso
                'target_demographic': 0.15,   # 15% peso
                'disposable_income': 0.10     # 10% peso
            },
            'market_readiness': {
                'smartphone_penetration': 0.15,
                'food_delivery_adoption': 0.10,
                'social_media_usage': 0.10
            },
            'competition': {
                'existing_competitors': 0.05,
                'market_saturation': 0.05,
                'local_alternatives': 0.05
            }
        }
    
    def evaluate_market(self, city_data: CityData) -> MarketScore:
        """Avaliar potencial de mercado para uma cidade"""
        
        scores = {}
        weighted_score = 0.0
        
        for category, criteria in self.evaluation_criteria.items():
            category_score = 0.0
            
            for criterion, weight in criteria.items():
                raw_score = self.get_criterion_score(city_data, criterion)
                weighted_criterion_score = raw_score * weight
                category_score += weighted_criterion_score
                weighted_score += weighted_criterion_score
            
            scores[category] = category_score
        
        return MarketScore(
            city=city_data.name,
            overall_score=weighted_score,
            category_scores=scores,
            recommendation=self.get_recommendation(weighted_score),
            risk_factors=self.identify_risk_factors(city_data),
            opportunity_factors=self.identify_opportunities(city_data)
        )
```

### 1.2 Mercados Prioritários - Brasil

#### **Tier 1: Expansão Nacional Imediata (2024)**

**1. Rio de Janeiro**
- **População**: 6.7M (região metropolitana)
- **Target Demographic**: 1.2M usuários potenciais (18-40 anos, classe B/C)
- **Market Readiness**: 95% smartphone penetration, alta adoção de apps
- **Competition**: Baixa (sem player dominante)
- **Investment Required**: R$ 150k
- **Timeline**: Q2 2024
- **Expected ROI**: 200% em 12 meses

**Estratégia de Entrada**:
```yaml
Fase 1 (Mês 1-2): Pesquisa e Setup
  - Mapeamento de 500 estabelecimentos
  - Parcerias com 3 influenciadores cariocas
  - Adaptação linguística ("rolê" vs "programa")
  - Setup de infraestrutura regional

Fase 2 (Mês 3): Beta Launch
  - 100 beta testers recrutados via Instagram
  - 25 estabelecimentos piloto onboarded
  - Teste de precisão da IA com contexto carioca
  
Fase 3 (Mês 4-6): Public Launch
  - Marketing digital focado em Zona Sul/Barra
  - Eventos de lançamento em 5 bairros principais
  - Meta: 2.000 MAU em 6 meses
```

**2. Belo Horizonte**
- **População**: 2.7M (região metropolitana)
- **Target Demographic**: 450k usuários potenciais
- **Market Readiness**: 90% smartphone, forte cultura gastronômica
- **Competition**: Muito baixa
- **Investment Required**: R$ 100k
- **Timeline**: Q3 2024
- **Expected ROI**: 250% em 12 meses

**3. Brasília**
- **População**: 3.1M (DF)
- **Target Demographic**: 600k usuários potenciais
- **Market Readiness**: Alta renda, tech-friendly
- **Competition**: Baixa
- **Investment Required**: R$ 120k
- **Timeline**: Q4 2024
- **Expected ROI**: 180% em 12 meses

#### **Tier 2: Expansão Regional (2025)**

**4. Porto Alegre**
- **Rationale**: Forte cultura gastronômica, público jovem universitário
- **Timeline**: Q1 2025
- **Investment**: R$ 100k

**5. Recife**
- **Rationale**: Hub tecnológico do Nordeste, vida noturna intensa
- **Timeline**: Q2 2025
- **Investment**: R$ 110k

**6. Salvador**
- **Rationale**: Turismo + população local, cultura social forte
- **Timeline**: Q3 2025
- **Investment**: R$ 120k

### 1.3 Expansão Internacional

#### **Tier 1: América Latina (2025-2026)**

**1. Buenos Aires, Argentina**
- **Market Size**: 15M (região metropolitana)
- **Target Demographic**: 2.5M usuários potenciais
- **Market Readiness**: 88% smartphone, alta adoção de apps
- **Cultural Fit**: Muito alta (cultura similar, horários noturnos)
- **Investment Required**: R$ 300k
- **Timeline**: Q3 2025

**Adaptações Necessárias**:
```yaml
Technical:
  - Spanish language AI training
  - Argentine Spanish specific expressions
  - Local payment integration (MercadoPago)
  - Currency handling (ARS)

Business:
  - Local partnership for establishment onboarding
  - Argentina tax compliance
  - Local marketing team or agency
  - Banking/payment processing setup

Cultural:
  - Later dining times (22h-24h normal)
  - Different tipping culture
  - Mate culture integration
  - Local slang and expressions
```

**2. Santiago, Chile**
- **Market Size**: 7M (região metropolitana)
- **Target Demographic**: 1.1M usuários potenciais
- **Market Readiness**: 92% smartphone, alta renda per capita
- **Investment Required**: R$ 280k
- **Timeline**: Q1 2026

**3. Cidade do México, México**
- **Market Size**: 22M (região metropolitana)
- **Target Demographic**: 3.5M usuários potenciais
- **Market Readiness**: 85% smartphone, mercado massive
- **Investment Required**: R$ 400k
- **Timeline**: Q3 2026

---

## 2. Expansão de Domínio

### 2.1 Análise de Segmentos Adjacentes

#### **Matriz de Proximidade de Domínio**
```
                    Proximidade ao Core Business
                    Baixa    Média    Alta
Complexidade   Alta │  ?   │  ?   │ Travel  │
de Execução  Média │  B2B │ Work │ Events  │
              Baixa │ Groc │ Shop │ Coffee  │
```

**Legenda**:
- **Events**: Eventos culturais e entretenimento
- **Travel**: Experiências para turistas
- **Work**: Networking profissional
- **Coffee**: Cafeterias e coworkings
- **Shop**: Shopping e varejo
- **Groc**: Groceries e mercados
- **B2B**: Soluções corporativas

### 2.2 Domínios Prioritários

#### **1. CheckIn Events - Entretenimento Cultural**

**Market Opportunity**:
- **TAM**: R$ 2.5B (mercado de eventos culturais Brasil)
- **SAM**: R$ 150M (eventos em SP/RJ/BH)
- **SOM**: R$ 15M (nossa fatia estimada em 3 anos)

**Value Proposition**: 
"Descubra e organize experiências culturais únicas com seus amigos"

**Diferenciação**:
- IA especializada em perfil cultural/entretenimento
- Coordenação social para grupos
- Curadoria de eventos não-mainstream
- Integração com venues de cultura

**Monetização**:
```yaml
Revenue Streams:
  ticket_commissions: 8-12% por ingresso vendido
  venue_partnerships: R$ 200-500/mês por venue cultural
  premium_curation: R$ 29/mês para usuários premium
  corporate_events: R$ 1000-5000 por evento corporativo

Target Metrics (12 meses):
  events_per_month: 500
  tickets_sold_monthly: 2000
  revenue_target: R$ 25k MRR
```

**Development Roadmap**:
```yaml
Q1 2024 - MVP Development:
  - Event discovery and recommendation engine
  - Basic social coordination features
  - Integration with Sympla/Eventbrite
  - SP market focus

Q2 2024 - Beta Launch:
  - 50 cultural venues onboarded
  - 100 beta users testing
  - Feedback integration and iteration

Q3 2024 - Public Launch:
  - Marketing campaign focused on cultural events
  - Expansion to RJ market
  - Partnership with major cultural centers

Q4 2024 - Scaling:
  - 200 venues across SP/RJ
  - Premium features launch
  - Corporate events pilot
```

#### **2. CheckIn Travel - Experiências Autênticas**

**Market Opportunity**:
- **TAM**: R$ 8B (turismo doméstico Brasil)
- **SAM**: R$ 400M (turismo experiencial)
- **SOM**: R$ 40M (fatia em 5 anos)

**Value Proposition**: 
"Viva como um local, não como turista"

**Target Audience**:
- Turistas nacionais em SP/RJ (70%)
- Turistas internacionais (20%)
- Locais explorando própria cidade (10%)

**Diferenciação**:
- IA treinada em preferências local vs. turista
- Experiências curadas por moradores locais
- Roteiros personalizados por tempo disponível
- Anti-tourist trap guarantee

**Monetização**:
```yaml
Revenue Streams:
  experience_commissions: 15-20% por experiência bookada
  hotel_partnerships: R$ 50-100 por conversão
  premium_guides: R$ 99 por roteiro premium personalizado
  local_guide_network: 30% comissão de guias locais

Target Metrics (18 meses):
  experiences_monthly: 300
  tourists_served: 1500/mês
  revenue_target: R$ 35k MRR
```

#### **3. CheckIn Work - Networking Profissional**

**Market Opportunity**:
- **TAM**: R$ 500M (networking profissional Brasil)
- **SAM**: R$ 50M (networking em grandes centros)
- **SOM**: R$ 8M (nossa oportunidade)

**Value Proposition**: 
"Networking autêntico em contextos sociais descontraídos"

**Target Audience**:
- Profissionais 25-40 anos (80%)
- Freelancers e empreendedores (15%)
- Executivos seniores (5%)

**Diferenciação**:
- Matching baseado em objetivos profissionais + chemistry
- Eventos em venues descontraídos (não salas de reunião)
- IA que facilita conversas e conexões
- Métricas de networking success

**Monetização**:
```yaml
Revenue Streams:
  professional_subscription: R$ 49/mês para profissionais
  corporate_packages: R$ 500-2000/mês para empresas
  event_organization: R$ 200-500 por evento organizado
  premium_matching: R$ 99 por match premium

Target Metrics (24 meses):
  active_professionals: 2000
  monthly_connections: 1500
  revenue_target: R$ 20k MRR
```

---

## 3. Framework de Execução

### 3.1 Modelo de Entrada em Novos Mercados

#### **Playbook Padronizado de Expansão**
```python
class MarketExpansionPlaybook:
    def __init__(self):
        self.phases = {
            'market_research': {
                'duration_weeks': 4,
                'activities': [
                    'local_market_analysis',
                    'competitor_landscape',
                    'customer_interviews',
                    'partnership_opportunities'
                ],
                'success_criteria': ['market_size_validated', 'competition_mapped']
            },
            'technical_setup': {
                'duration_weeks': 3,
                'activities': [
                    'local_api_integrations',
                    'payment_gateway_setup',
                    'language_localization',
                    'infrastructure_deployment'
                ],
                'success_criteria': ['platform_localized', 'payments_working']
            },
            'content_and_partnerships': {
                'duration_weeks': 6,
                'activities': [
                    'venue_onboarding',
                    'local_influencer_partnerships',
                    'content_creation',
                    'team_hiring'
                ],
                'success_criteria': ['50_venues_onboarded', 'local_team_ready']
            },
            'beta_launch': {
                'duration_weeks': 4,
                'activities': [
                    'beta_user_recruitment',
                    'product_testing',
                    'feedback_integration',
                    'metrics_optimization'
                ],
                'success_criteria': ['nps_above_60', 'retention_above_40_percent']
            },
            'public_launch': {
                'duration_weeks': 8,
                'activities': [
                    'marketing_campaign',
                    'pr_and_media',
                    'user_acquisition',
                    'scaling_operations'
                ],
                'success_criteria': ['target_mau_reached', 'unit_economics_positive']
            }
        }
    
    def execute_expansion(self, market: Market) -> ExpansionPlan:
        """Generate detailed expansion plan for specific market"""
        
        plan = ExpansionPlan(market=market)
        
        for phase_name, phase_config in self.phases.items():
            phase_plan = self.create_phase_plan(phase_name, phase_config, market)
            plan.add_phase(phase_plan)
        
        return plan
```

### 3.2 Adaptações por Mercado

#### **Matriz de Adaptações Necessárias**
| Aspecto | São Paulo (Base) | Rio de Janeiro | Buenos Aires | Belo Horizonte |
|---------|------------------|----------------|--------------|----------------|
| **Linguagem** | PT-BR padrão | Carioquês, gírias | ES-AR | Mineirês |
| **Horários** | 19h-23h | 20h-01h | 22h-02h | 19h-22h |
| **Cultura** | Diversa | Praia/outdoor | Noturna intensa | Tradicional |
| **Pagamentos** | PIX, cartão | PIX, cartão | MercadoPago | PIX, cartão |
| **Parcerias** | Influencers tech | Influencers lifestyle | Bloggers gastro | Influencers locais |

#### **Template de Adaptação Cultural**
```yaml
Cultural_Adaptation_Template:
  language_localization:
    - local_slang_integration
    - region_specific_expressions
    - cultural_context_awareness
    
  timing_preferences:
    - meal_times_adjustment
    - nightlife_hours_mapping
    - weekend_vs_weekday_patterns
    
  social_behaviors:
    - group_size_preferences
    - formality_levels
    - invitation_etiquette
    
  local_partnerships:
    - influencer_selection_criteria
    - media_outlet_preferences
    - community_leader_engagement
```

---

## 4. Gestão de Portfolio de Expansão

### 4.1 Alocação de Recursos

#### **Resource Allocation Matrix**
```yaml
Total Investment Budget: R$ 2M (24 meses)

Geographic Expansion (60% - R$ 1.2M):
  Rio de Janeiro: R$ 200k (Q2 2024)
  Belo Horizonte: R$ 150k (Q3 2024)
  Brasília: R$ 180k (Q4 2024)
  Buenos Aires: R$ 400k (Q3 2025)
  Santiago: R$ 270k (Q1 2026)

Domain Expansion (30% - R$ 600k):
  CheckIn Events: R$ 250k (Q1-Q3 2024)
  CheckIn Travel: R$ 200k (Q4 2024-Q2 2025)
  CheckIn Work: R$ 150k (Q3-Q4 2025)

Infrastructure & Support (10% - R$ 200k):
  Multi-region infrastructure: R$ 100k
  Team expansion: R$ 80k
  Tools and systems: R$ 20k
```

### 4.2 Métricas de Sucesso por Iniciativa

#### **Geographic Expansion KPIs**
| Mercado | 6 Meses | 12 Meses | 18 Meses | Success Threshold |
|---------|---------|----------|----------|-------------------|
| **Rio de Janeiro** | 2k MAU | 6k MAU | 10k MAU | >5k MAU @ 12m |
| **Belo Horizonte** | 1k MAU | 3k MAU | 5k MAU | >2.5k MAU @ 12m |
| **Brasília** | 1.5k MAU | 4k MAU | 6k MAU | >3k MAU @ 12m |
| **Buenos Aires** | - | - | 2k MAU | >1.5k MAU @ 18m |

#### **Domain Expansion KPIs**
| Domínio | 6 Meses | 12 Meses | Success Threshold |
|---------|---------|----------|-------------------|
| **Events** | 500 eventos | 2k eventos | >1.5k eventos @ 12m |
| **Travel** | 100 exp/mês | 400 exp/mês | >300 exp/mês @ 12m |
| **Work** | 200 profissionais | 1k profissionais | >800 prof @ 12m |

---

## 5. Gestão de Riscos na Expansão

### 5.1 Riscos Geográficos

#### **Risk Assessment Matrix**
| Risco | Probabilidade | Impacto | Mitigação |
|-------|---------------|---------|-----------|
| **Resistência cultural local** | Média | Alto | Pesquisa etnográfica + parcerias locais |
| **Competição local forte** | Baixa | Alto | Diferenciação via IA + entrada rápida |
| **Regulamentações locais** | Baixa | Médio | Due diligence legal + compliance |
| **Problemas de infraestrutura** | Baixa | Médio | Setup multi-região + contingência |
| **Taxa de câmbio (internacional)** | Alta | Médio | Hedge cambial + preços locais |

#### **Contingency Plans**
```python
class ExpansionRiskManagement:
    def __init__(self):
        self.contingency_plans = {
            'slow_adoption': {
                'triggers': ['mau_below_50_percent_target', 'retention_below_30_percent'],
                'actions': [
                    'increase_marketing_spend_50_percent',
                    'pivot_value_proposition',
                    'enhance_local_partnerships',
                    'consider_market_exit_if_no_improvement_3_months'
                ]
            },
            'strong_competition': {
                'triggers': ['competitor_launches_similar_product', 'market_share_drops'],
                'actions': [
                    'accelerate_feature_development',
                    'increase_ai_accuracy',
                    'exclusive_venue_partnerships',
                    'competitive_pricing_strategy'
                ]
            },
            'regulatory_issues': {
                'triggers': ['new_regulations_announced', 'compliance_violations'],
                'actions': [
                    'legal_consultation',
                    'product_modifications',
                    'government_relations',
                    'temporary_market_pause_if_needed'
                ]
            }
        }
```

### 5.2 Riscos de Domínio

#### **Domain-Specific Risks**
```yaml
CheckIn Events:
  market_saturation: "Eventbrite dominance"
  mitigation: "Focus on curated/unique events"
  
  low_commission_margins: "Venues want lower fees"
  mitigation: "Value-added services + premium tiers"

CheckIn Travel:
  tourism_seasonality: "Demand varies greatly"
  mitigation: "Focus on domestic + year-round experiences"
  
  quality_control: "Bad experiences damage brand"
  mitigation: "Strict vetting + insurance + guarantees"

CheckIn Work:
  networking_fatigue: "Professionals tired of events"
  mitigation: "Organic contexts + quality over quantity"
  
  corporate_sales_cycle: "Long B2B sales cycles"
  mitigation: "Freemium model + viral growth"
```

---

## 6. Cronograma de Execução Integrado

### 6.1 Master Timeline (2024-2026)

#### **2024: Ano da Expansão Nacional**
```
Q1 2024:
├── Core SP consolidation (maintain)
├── Rio de Janeiro research & setup
└── CheckIn Events MVP development

Q2 2024:
├── Rio de Janeiro beta launch
├── CheckIn Events public launch (SP)
└── Belo Horizonte research & setup

Q3 2024:
├── Rio de Janeiro public launch
├── Belo Horizonte beta launch
├── CheckIn Events expansion (RJ)
└── CheckIn Travel MVP development

Q4 2024:
├── Belo Horizonte public launch
├── Brasília research & setup
├── CheckIn Travel beta launch
└── International market research (Argentina)
```

#### **2025: Ano da Diversificação**
```
Q1 2025:
├── Brasília beta launch
├── CheckIn Travel public launch
├── Porto Alegre research
└── Buenos Aires detailed planning

Q2 2025:
├── Brasília public launch
├── Porto Alegre beta launch
├── CheckIn Work MVP development
└── Buenos Aires technical setup

Q3 2025:
├── Porto Alegre public launch
├── Buenos Aires beta launch
├── CheckIn Work beta launch
└── Santiago research

Q4 2025:
├── Buenos Aires public launch
├── CheckIn Work public launch
├── Recife research
└── 2026 planning
```

#### **2026: Ano da Consolidação**
```
Q1 2026:
├── Santiago beta launch
├── Recife beta launch
├── All products optimization
└── Series A preparation

Q2 2026:
├── Santiago public launch
├── Recife public launch
├── México research
└── Platform consolidation

Q3 2026:
├── México pilot
├── Advanced AI features
├── International expansion evaluation
└── Strategic partnerships

Q4 2026:
├── Portfolio optimization
├── Next phase planning
├── Market leadership consolidation
└── IPO readiness assessment
```

---

## 7. Métricas de Sucesso Consolidadas

### 7.1 Geographic Expansion Success Metrics

#### **Consolidated Geographic KPIs**
```yaml
Brazil Coverage (by end 2025):
  cities_active: 6 (SP, RJ, BH, BSB, POA, REC)
  total_mau_brazil: 50,000
  revenue_geographic_expansion: R$ 150k MRR
  market_share_major_cities: 8-12%

International Presence (by end 2026):
  countries_active: 3 (Brazil, Argentina, Chile)
  total_mau_latam: 75,000
  revenue_international: R$ 80k MRR
  brand_recognition_latam: Top 3 in category
```

### 7.2 Domain Expansion Success Metrics

#### **Domain Portfolio Performance**
```yaml
CheckIn Events (by end 2025):
  events_organized_monthly: 1,500
  tickets_facilitated_monthly: 8,000
  revenue_events: R$ 40k MRR
  nps_events: >75

CheckIn Travel (by end 2025):
  experiences_monthly: 800
  tourists_served_monthly: 2,500
  revenue_travel: R$ 50k MRR
  repeat_customer_rate: >60%

CheckIn Work (by end 2026):
  active_professionals: 5,000
  monthly_connections: 3,000
  revenue_work: R$ 30k MRR
  networking_success_rate: >70%
```

### 7.3 Overall Portfolio Health

#### **Portfolio Success Dashboard**
```python
class PortfolioHealthDashboard:
    def calculate_portfolio_health(self) -> PortfolioHealth:
        """Calculate overall health of expansion portfolio"""
        
        # Geographic diversification
        geographic_health = self.calculate_geographic_diversification()
        
        # Domain diversification
        domain_health = self.calculate_domain_diversification()
        
        # Revenue distribution
        revenue_health = self.calculate_revenue_distribution()
        
        # Risk distribution
        risk_health = self.calculate_risk_distribution()
        
        overall_score = (
            geographic_health * 0.3 +
            domain_health * 0.3 +
            revenue_health * 0.25 +
            risk_health * 0.15
        )
        
        return PortfolioHealth(
            overall_score=overall_score,
            geographic_diversification=geographic_health,
            domain_diversification=domain_health,
            revenue_distribution=revenue_health,
            risk_profile=risk_health,
            recommendations=self.generate_portfolio_recommendations()
        )
```

---

**Responsável Estratégico**: Gabriela Lima Sotero (CEO)  
**Execution Lead**: Henrique Fontaine (CTO)  
**International Expansion**: External consultant + internal team  
**Market Research**: Mixed internal/external approach  
**Review Cycle**: Mensal para operações, trimestral para estratégia  
**Decision Gates**: Go/No-Go a cada 6 meses por mercado  
**Última atualização**: Dezembro 2024
