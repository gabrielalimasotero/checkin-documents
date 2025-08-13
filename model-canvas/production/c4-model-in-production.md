# C4 Model em Produção - CheckIn

## Visão Geral
Este canvas implementa o modelo C4 (Context, Containers, Components, Code) em ambiente de produção para o CheckIn, fornecendo diferentes níveis de abstração arquitetural com foco em operação real e escalabilidade.

---

## 1. Nível 1: Context Diagram (Sistema em Produção)

### 1.1 System Context - Ambiente de Produção
```
                    ┌──────────────────────────────────────────────────┐
                    │                                                  │
         ┌─────────────────┐                                ┌─────────────────┐
         │   Mobile Users  │                                │  Venue Owners   │
         │                 │                                │                 │
         │ • iOS/Android   │                                │ • Web Dashboard │
         │ • 10k+ users    │                                │ • 200+ venues   │
         │ • Global access │                                │ • Business tools│
         └─────────────────┘                                └─────────────────┘
                    │                                                  │
                    │ HTTPS/Mobile APIs                               │ HTTPS/Web APIs
                    │                                                  │
                    ▼                                                  ▼
         ┌─────────────────────────────────────────────────────────────────────────┐
         │                                                                         │
         │                        CHECKIN PLATFORM                                │
         │                                                                         │
         │ • AI-Powered Recommendations                                           │
         │ • Social Coordination                                                  │
         │ • Real-time Check-ins                                                  │
         │ • Business Analytics                                                   │
         │ • 99.9% Uptime SLA                                                     │
         │ • Auto-scaling Infrastructure                                          │
         │                                                                         │
         └─────────────────────────────────────────────────────────────────────────┘
                    │                                                  │
                    │ API Calls                                        │ Webhook/API
                    │                                                  │
                    ▼                                                  ▼
         ┌─────────────────┐                                ┌─────────────────┐
         │ External APIs   │                                │ Infrastructure  │
         │                 │                                │   Services      │
         │ • OpenAI GPT-4  │                                │ • AWS/GCP       │
         │ • Google Places │                                │ • CDN/Cloudflare│
         │ • Weather API   │                                │ • Monitoring    │
         │ • Payment APIs  │                                │ • Backup Systems│
         └─────────────────┘                                └─────────────────┘
```

### 1.2 Production Context Specifications

#### **User Base Production Stats**
- **Peak Concurrent Users**: 2,500 (Friday 19h-22h)
- **Daily Active Users**: 8,000-12,000
- **Geographic Distribution**: São Paulo (70%), RJ (20%), Other (10%)
- **Device Split**: iOS (60%), Android (40%)
- **Network Conditions**: 4G (70%), WiFi (25%), 3G (5%)

#### **Business Context**
- **Venue Partners**: 200+ establishments
- **Premium Subscribers**: 50+ venues paying R$ 99/month
- **Revenue**: R$ 25k MRR growing 15% month-over-month
- **Support Load**: 50-80 tickets/week, 4.8/5 satisfaction

---

## 2. Nível 2: Container Diagram (Produção)

### 2.1 Production Container Architecture
```
                    ┌─────────────────┐
                    │   CloudFlare    │
                    │   CDN + WAF     │
                    └─────────────────┘
                              │
                    ┌─────────────────┐
                    │  Load Balancer  │
                    │  (AWS ALB)      │
                    └─────────────────┘
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                    │
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│  Mobile Apps    │  │   Admin Panel   │  │  Monitoring     │
│                 │  │                 │  │  Dashboard      │
│ React Native    │  │ React SPA       │  │ Grafana         │
│ 50MB bundle     │  │ Hosted on S3    │  │ Public metrics  │
│ Auto-updates    │  │ CloudFront CDN  │  │ Status page     │
└─────────────────┘  └─────────────────┘  └─────────────────┘
         │                    │                    │
         └────────────────────┼────────────────────┘
                              │ HTTPS/REST
                    ┌─────────────────┐
                    │   API Gateway   │
                    │   (Kong)        │
                    │ Rate Limiting   │
                    │ Authentication  │
                    │ Request Logging │
                    └─────────────────┘
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                    │
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│ Recommendation  │  │   Social API    │  │   Venue API     │
│   Service       │  │                 │  │                 │
│                 │  │ FastAPI         │  │ FastAPI         │
│ FastAPI + AI    │  │ WebSocket       │  │ Background Jobs │
│ OpenAI API      │  │ 3 replicas      │  │ 2 replicas      │
│ 5 replicas      │  │ 1GB RAM each    │  │ 512MB RAM each  │
│ 2GB RAM each    │  │                 │  │                 │
└─────────────────┘  └─────────────────┘  └─────────────────┘
         │                    │                    │
         └────────────────────┼────────────────────┘
                              │
                    ┌─────────────────┐
                    │  Message Queue  │
                    │    (Redis)      │
                    │  Pub/Sub + Jobs │
                    │  3 replicas     │
                    │  HA Cluster     │
                    └─────────────────┘
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                    │
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│   PostgreSQL    │  │     Redis       │  │  File Storage   │
│   (Primary)     │  │   (Cache)       │  │    (AWS S3)     │
│                 │  │                 │  │                 │
│ Multi-AZ RDS    │  │ ElastiCache     │  │ Images/Assets   │
│ 16GB RAM        │  │ 3 nodes         │  │ CloudFront CDN  │
│ Read Replicas   │  │ 4GB RAM each    │  │ 99.9% durability│
│ Auto-backup     │  │ Auto-failover   │  │                 │
└─────────────────┘  └─────────────────┘  └─────────────────┘
```

### 2.2 Production Container Specifications

#### **API Gateway (Kong)**
```yaml
Production Specs:
  instances: 3 (multi-AZ)
  cpu: 1 vCPU
  memory: 2GB
  
Key Configurations:
  rate_limiting: 1000 req/min per user
  cors_enabled: true
  jwt_validation: true
  request_logging: stdout
  response_caching: 5min for static data
  
Health Checks:
  path: /health
  interval: 30s
  timeout: 5s
  healthy_threshold: 2
```

#### **Recommendation Service**
```yaml
Production Specs:
  instances: 5 (auto-scaling 3-10)
  cpu: 2 vCPU
  memory: 4GB
  
Environment:
  NODE_ENV: production
  OPENAI_API_KEY: encrypted
  DATABASE_URL: rds_primary
  REDIS_URL: elasticache_cluster
  
Scaling Policy:
  target_cpu: 70%
  target_memory: 80%
  scale_up_cooldown: 300s
  scale_down_cooldown: 600s
```

#### **PostgreSQL (RDS)**
```yaml
Production Specs:
  instance_class: db.r5.2xlarge
  engine_version: 15.4
  allocated_storage: 500GB
  
High Availability:
  multi_az: true
  read_replicas: 2
  backup_retention: 30 days
  
Performance:
  max_connections: 200
  shared_buffers: 4GB
  effective_cache_size: 12GB
  
Monitoring:
  performance_insights: enabled
  enhanced_monitoring: 60s interval
```

---

## 3. Nível 3: Component Diagram (Produção)

### 3.1 Recommendation Service Components (Production)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    RECOMMENDATION SERVICE                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐     │
│  │  Load Balancer  │    │  Health Check   │    │   Metrics       │     │
│  │   (Internal)    │    │   Endpoint      │    │  Prometheus     │     │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘     │
│           │                       │                       │             │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                    API LAYER                                    │   │
│  │                                                                 │   │
│  │  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────┐ │   │
│  │  │ Recommendation  │    │   Context       │    │ Feedback    │ │   │
│  │  │   Controller    │    │  Controller     │    │ Controller  │ │   │
│  │  └─────────────────┘    └─────────────────┘    └─────────────┘ │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│           │                       │                       │             │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                  BUSINESS LAYER                                 │   │
│  │                                                                 │   │
│  │  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────┐ │   │
│  │  │Recommendation   │    │   AI Service    │    │ Context     │ │   │
│  │  │   Service       │    │   Manager       │    │ Processor   │ │   │
│  │  │                 │    │                 │    │             │ │   │
│  │  │ • Generate      │    │ • OpenAI Client │    │ • Parse     │ │   │
│  │  │ • Rank          │    │ • Prompt Mgmt   │    │ • Validate  │ │   │
│  │  │ • Personalize   │    │ • Cost Tracking │    │ • Enrich    │ │   │
│  │  └─────────────────┘    └─────────────────┘    └─────────────┘ │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│           │                       │                       │             │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                    DATA LAYER                                   │   │
│  │                                                                 │   │
│  │  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────┐ │   │
│  │  │     Cache       │    │   Database      │    │ External    │ │   │
│  │  │   Manager       │    │   Repository    │    │ API Client  │ │   │
│  │  │                 │    │                 │    │             │ │   │
│  │  │ • Redis Ops     │    │ • User Data     │    │ • OpenAI    │ │   │
│  │  │ • TTL Mgmt      │    │ • Venue Data    │    │ • Google    │ │   │
│  │  │ • Invalidation  │    │ • Preferences   │    │ • Weather   │ │   │
│  │  └─────────────────┘    └─────────────────┘    └─────────────┘ │   │
│  └─────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Component Production Configurations

#### **AI Service Manager**
```python
class ProductionAIService:
    def __init__(self):
        self.openai_client = AsyncOpenAI(
            api_key=os.getenv('OPENAI_API_KEY'),
            timeout=30.0,
            max_retries=3
        )
        self.cost_tracker = CostTracker()
        self.circuit_breaker = CircuitBreaker(
            failure_threshold=5,
            recovery_timeout=60
        )
        
    @circuit_breaker
    async def generate_recommendations(
        self, 
        context: UserContext
    ) -> List[Suggestion]:
        
        # Cost check
        if await self.cost_tracker.would_exceed_budget():
            return await self.fallback_recommendations(context)
        
        # Rate limiting
        await self.rate_limiter.acquire(context.user_id)
        
        try:
            response = await self.openai_client.chat.completions.create(
                model="gpt-4-turbo-preview",
                messages=self.build_prompt(context),
                max_tokens=1000,
                temperature=0.7
            )
            
            # Track usage and costs
            await self.cost_tracker.record_usage(response.usage)
            
            return self.parse_ai_response(response)
            
        except OpenAIError as e:
            logger.error(f"OpenAI API error: {e}")
            return await self.fallback_recommendations(context)
```

#### **Cache Manager with Production Optimization**
```python
class ProductionCacheManager:
    def __init__(self):
        # Redis cluster for HA
        self.redis_cluster = RedisCluster(
            startup_nodes=[
                {"host": "cache-1.elasticache.amazonaws.com", "port": 6379},
                {"host": "cache-2.elasticache.amazonaws.com", "port": 6379},
                {"host": "cache-3.elasticache.amazonaws.com", "port": 6379}
            ],
            decode_responses=True,
            health_check_interval=30,
            retry_on_timeout=True
        )
        
        # Different TTLs for different data types
        self.ttl_config = {
            'user_preferences': 3600,      # 1 hour
            'venue_data': 1800,            # 30 minutes
            'recommendations': 300,         # 5 minutes
            'ai_responses': 600,           # 10 minutes
            'static_data': 86400           # 24 hours
        }
    
    async def get_recommendations(
        self, 
        cache_key: str
    ) -> Optional[List[Suggestion]]:
        try:
            cached_data = await self.redis_cluster.get(cache_key)
            if cached_data:
                # Track cache hit
                await self.metrics.increment('cache.hit', tags=['type:recommendations'])
                return json.loads(cached_data)
        except RedisError as e:
            logger.warning(f"Cache read error: {e}")
            await self.metrics.increment('cache.error', tags=['operation:read'])
        
        return None
    
    async def set_recommendations(
        self, 
        cache_key: str, 
        suggestions: List[Suggestion]
    ):
        try:
            await self.redis_cluster.setex(
                cache_key,
                self.ttl_config['recommendations'],
                json.dumps([s.dict() for s in suggestions])
            )
            await self.metrics.increment('cache.write', tags=['type:recommendations'])
        except RedisError as e:
            logger.warning(f"Cache write error: {e}")
            await self.metrics.increment('cache.error', tags=['operation:write'])
```

---

## 4. Nível 4: Code Level (Production Critical Paths)

### 4.1 Production-Grade Recommendation Algorithm

```python
class ProductionRecommendationEngine:
    """Production-optimized recommendation engine with full observability"""
    
    def __init__(self):
        self.ai_service = ProductionAIService()
        self.cache_manager = ProductionCacheManager()
        self.venue_repository = VenueRepository()
        self.user_service = UserService()
        self.metrics = PrometheusMetrics()
        
    @observe_performance("recommendation_generation")
    @retry(max_attempts=3, backoff_factor=1.5)
    async def generate_recommendations(
        self, 
        user_context: UserContext
    ) -> RecommendationResponse:
        """
        Generate personalized recommendations with full production resilience
        """
        start_time = time.time()
        request_id = str(uuid4())
        
        try:
            # 1. Input validation
            validated_context = await self.validate_context(user_context)
            
            # 2. Check cache first
            cache_key = self.build_cache_key(validated_context)
            cached_suggestions = await self.cache_manager.get_recommendations(cache_key)
            
            if cached_suggestions:
                await self.metrics.increment('recommendations.cache_hit')
                return RecommendationResponse(
                    suggestions=cached_suggestions,
                    source='cache',
                    request_id=request_id,
                    response_time_ms=int((time.time() - start_time) * 1000)
                )
            
            # 3. Get candidate venues
            candidate_venues = await self.get_candidate_venues(validated_context)
            
            if not candidate_venues:
                await self.metrics.increment('recommendations.no_candidates')
                return await self.handle_no_venues_available(validated_context)
            
            # 4. Generate AI recommendations
            ai_suggestions = await self.ai_service.generate_recommendations(
                validated_context, 
                candidate_venues
            )
            
            # 5. Post-process and rank
            final_suggestions = await self.post_process_suggestions(
                ai_suggestions, 
                validated_context
            )
            
            # 6. Cache results
            await self.cache_manager.set_recommendations(cache_key, final_suggestions)
            
            # 7. Track metrics
            await self.track_recommendation_metrics(
                validated_context, 
                final_suggestions, 
                time.time() - start_time
            )
            
            return RecommendationResponse(
                suggestions=final_suggestions,
                source='ai_generated',
                request_id=request_id,
                response_time_ms=int((time.time() - start_time) * 1000)
            )
            
        except ValidationError as e:
            await self.metrics.increment('recommendations.validation_error')
            logger.warning(f"Invalid context for recommendation: {e}")
            raise BadRequestError(f"Invalid request: {e}")
            
        except ExternalServiceError as e:
            await self.metrics.increment('recommendations.external_service_error')
            logger.error(f"External service error in recommendations: {e}")
            
            # Fallback to rule-based recommendations
            fallback_suggestions = await self.generate_fallback_recommendations(
                validated_context
            )
            
            return RecommendationResponse(
                suggestions=fallback_suggestions,
                source='fallback_rules',
                request_id=request_id,
                response_time_ms=int((time.time() - start_time) * 1000)
            )
            
        except Exception as e:
            await self.metrics.increment('recommendations.unexpected_error')
            logger.error(f"Unexpected error in recommendations: {e}", exc_info=True)
            
            # Emergency fallback
            emergency_suggestions = await self.generate_emergency_fallback(
                validated_context
            )
            
            return RecommendationResponse(
                suggestions=emergency_suggestions,
                source='emergency_fallback',
                request_id=request_id,
                response_time_ms=int((time.time() - start_time) * 1000)
            )
    
    async def validate_context(self, context: UserContext) -> UserContext:
        """Validate and enrich user context"""
        
        # Required fields validation
        if not context.user_id:
            raise ValidationError("user_id is required")
        
        if not context.location or not context.location.is_valid():
            raise ValidationError("valid location is required")
        
        # Enrich with user preferences
        user_preferences = await self.user_service.get_preferences(context.user_id)
        context.preferences = user_preferences
        
        # Validate business hours
        if context.time:
            context.is_business_hours = self.is_business_hours(context.time)
        
        return context
    
    async def get_candidate_venues(
        self, 
        context: UserContext
    ) -> List[Venue]:
        """Get candidate venues based on context"""
        
        search_criteria = VenueSearchCriteria(
            location=context.location,
            radius_km=context.max_distance_km or 5,
            categories=context.preferred_categories,
            min_rating=context.min_rating or 3.5,
            open_at_time=context.time,
            price_range=context.price_range
        )
        
        # Get venues from repository with caching
        venues = await self.venue_repository.find_venues(search_criteria)
        
        # Filter based on user history (avoid recently rejected)
        filtered_venues = await self.filter_by_user_history(
            venues, 
            context.user_id
        )
        
        return filtered_venues[:20]  # Limit to top 20 candidates
    
    async def post_process_suggestions(
        self, 
        ai_suggestions: List[Suggestion],
        context: UserContext
    ) -> List[Suggestion]:
        """Post-process AI suggestions for production quality"""
        
        processed_suggestions = []
        
        for suggestion in ai_suggestions:
            # Enrich with real-time data
            suggestion = await self.enrich_with_realtime_data(suggestion)
            
            # Validate suggestion quality
            if await self.is_valid_suggestion(suggestion, context):
                processed_suggestions.append(suggestion)
        
        # Re-rank based on user preferences and real-time factors
        ranked_suggestions = await self.rank_suggestions(
            processed_suggestions, 
            context
        )
        
        return ranked_suggestions[:3]  # Return top 3
    
    async def track_recommendation_metrics(
        self, 
        context: UserContext,
        suggestions: List[Suggestion],
        response_time: float
    ):
        """Track detailed metrics for monitoring and optimization"""
        
        await self.metrics.record('recommendations.response_time', response_time)
        await self.metrics.increment('recommendations.generated')
        await self.metrics.record('recommendations.count', len(suggestions))
        
        # Track by context type
        context_tags = [
            f'companion_type:{context.companion_type}',
            f'time_of_day:{context.time.hour if context.time else "unknown"}',
            f'day_of_week:{context.time.weekday() if context.time else "unknown"}'
        ]
        
        await self.metrics.increment(
            'recommendations.by_context', 
            tags=context_tags
        )
```

### 4.2 Production Error Handling & Circuit Breaker

```python
class ProductionCircuitBreaker:
    """Production-grade circuit breaker for external service calls"""
    
    def __init__(
        self, 
        failure_threshold: int = 5,
        recovery_timeout: int = 60,
        expected_exception: Type[Exception] = Exception
    ):
        self.failure_threshold = failure_threshold
        self.recovery_timeout = recovery_timeout
        self.expected_exception = expected_exception
        
        self.failure_count = 0
        self.last_failure_time = None
        self.state = 'CLOSED'  # CLOSED, OPEN, HALF_OPEN
        
    def __call__(self, func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            if self.state == 'OPEN':
                if self._should_attempt_reset():
                    self.state = 'HALF_OPEN'
                else:
                    raise CircuitBreakerOpenError(
                        f"Circuit breaker is OPEN for {func.__name__}"
                    )
            
            try:
                result = await func(*args, **kwargs)
                self._on_success()
                return result
                
            except self.expected_exception as e:
                self._on_failure()
                raise
                
        return wrapper
    
    def _should_attempt_reset(self) -> bool:
        return (
            self.last_failure_time and
            time.time() - self.last_failure_time >= self.recovery_timeout
        )
    
    def _on_success(self):
        self.failure_count = 0
        self.state = 'CLOSED'
    
    def _on_failure(self):
        self.failure_count += 1
        self.last_failure_time = time.time()
        
        if self.failure_count >= self.failure_threshold:
            self.state = 'OPEN'
```

---

## 5. Production Monitoring & Observability

### 5.1 Application Performance Monitoring

```python
class ProductionAPM:
    """Application Performance Monitoring for production environment"""
    
    def __init__(self):
        self.prometheus = PrometheusClient()
        self.jaeger = JaegerClient()
        self.sentry = SentryClient()
        
    @contextmanager
    async def trace_request(self, operation_name: str, request_id: str):
        """Trace request through the entire system"""
        
        span = self.jaeger.start_span(operation_name)
        span.set_tag('request_id', request_id)
        
        start_time = time.time()
        
        try:
            yield span
            
            # Record success metrics
            duration = time.time() - start_time
            self.prometheus.histogram(
                f'{operation_name}.duration',
                duration,
                tags=['status:success']
            )
            
        except Exception as e:
            # Record error metrics
            duration = time.time() - start_time
            self.prometheus.histogram(
                f'{operation_name}.duration',
                duration,
                tags=['status:error']
            )
            
            # Send to error tracking
            self.sentry.capture_exception(e, extra={
                'request_id': request_id,
                'operation': operation_name,
                'duration': duration
            })
            
            span.set_tag('error', True)
            span.log_kv({'error.message': str(e)})
            
            raise
            
        finally:
            span.finish()
    
    async def track_business_metric(
        self, 
        metric_name: str, 
        value: float,
        tags: Dict[str, str] = None
    ):
        """Track business metrics for production insights"""
        
        await self.prometheus.gauge(metric_name, value, tags=tags or {})
        
        # Also send to business intelligence system
        await self.bi_system.record_metric(
            metric_name=metric_name,
            value=value,
            timestamp=datetime.utcnow(),
            tags=tags
        )
```

### 5.2 Production Health Checks

```python
class ProductionHealthCheck:
    """Comprehensive health checking for production deployment"""
    
    def __init__(self):
        self.db_pool = database_pool
        self.redis_client = redis_client
        self.external_apis = external_api_clients
        
    async def check_health(self) -> HealthStatus:
        """Comprehensive health check for load balancer"""
        
        checks = {
            'database': self._check_database(),
            'cache': self._check_redis(),
            'external_apis': self._check_external_apis(),
            'disk_space': self._check_disk_space(),
            'memory': self._check_memory_usage()
        }
        
        results = {}
        overall_healthy = True
        
        for check_name, check_coroutine in checks.items():
            try:
                result = await asyncio.wait_for(check_coroutine, timeout=5.0)
                results[check_name] = result
                
                if not result.healthy:
                    overall_healthy = False
                    
            except asyncio.TimeoutError:
                results[check_name] = HealthCheckResult(
                    healthy=False,
                    message="Health check timed out"
                )
                overall_healthy = False
                
            except Exception as e:
                results[check_name] = HealthCheckResult(
                    healthy=False,
                    message=f"Health check failed: {str(e)}"
                )
                overall_healthy = False
        
        return HealthStatus(
            healthy=overall_healthy,
            checks=results,
            timestamp=datetime.utcnow()
        )
    
    async def _check_database(self) -> HealthCheckResult:
        """Check database connectivity and performance"""
        try:
            start_time = time.time()
            
            async with self.db_pool.acquire() as conn:
                await conn.execute('SELECT 1')
            
            response_time = time.time() - start_time
            
            if response_time > 1.0:  # 1 second threshold
                return HealthCheckResult(
                    healthy=False,
                    message=f"Database response time too high: {response_time:.2f}s"
                )
            
            return HealthCheckResult(
                healthy=True,
                message=f"Database healthy (response: {response_time:.2f}s)"
            )
            
        except Exception as e:
            return HealthCheckResult(
                healthy=False,
                message=f"Database connection failed: {str(e)}"
            )
```

---

