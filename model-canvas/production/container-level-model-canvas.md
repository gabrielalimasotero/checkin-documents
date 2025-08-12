# Canvas de Nível de Container - CheckIn

## Visão Geral
Este canvas define a arquitetura de containers do CheckIn, especificando como os diferentes sistemas se comunicam, suas responsabilidades, tecnologias utilizadas e estratégias de deployment em ambiente de produção.

---

## 1. Arquitetura de Containers Overview

### 1.1 Diagrama de Containers
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Mobile App    │    │   Web Browser   │    │  Admin Panel    │
│   (React       │    │   (React SPA)   │    │  (React Admin)  │
│   Native)       │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────┐
                    │   API Gateway   │
                    │   (Kong/Nginx)  │
                    └─────────────────┘
                                 │
         ┌───────────────────────┼───────────────────────┐
         │                       │                       │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Recommendation  │    │   Social API    │    │  Venue API      │
│ Service         │    │   (FastAPI)     │    │  (FastAPI)      │
│ (FastAPI + AI)  │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────┐
                    │   Message Bus   │
                    │   (Redis/RabbitMQ) │
                    └─────────────────┘
                                 │
         ┌───────────────────────┼───────────────────────┐
         │                       │                       │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   PostgreSQL    │    │     Redis       │    │  External APIs  │
│   (Primary DB)  │    │   (Cache/Queue) │    │ (Google, OpenAI)│
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### 1.2 Container Responsibilities Matrix
| Container | Primary Responsibility | Secondary Responsibilities |
|-----------|----------------------|---------------------------|
| **Mobile App** | User interface mobile | Offline caching, push notifications |
| **Web Browser** | Desktop interface | Admin functions, analytics |
| **API Gateway** | Routing, auth, rate limiting | Load balancing, SSL termination |
| **Recommendation Service** | AI-powered suggestions | User preference learning |
| **Social API** | Social features | Event management, messaging |
| **Venue API** | Venue data management | Check-in processing |
| **PostgreSQL** | Primary data storage | ACID transactions |
| **Redis** | Caching, sessions | Message queuing |
| **External APIs** | Third-party integrations | Data enrichment |

---

## 2. Container Specifications

### 2.1 Frontend Containers

#### **Mobile App Container**
**Technology Stack**:
- Framework: React Native + Expo
- State Management: Redux Toolkit + RTK Query
- Navigation: React Navigation v6
- Offline Storage: AsyncStorage + SQLite
- Push Notifications: Expo Notifications

**Container Specifications**:
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build:mobile
EXPOSE 19000 19001 19002
CMD ["npm", "start"]
```

**Environment Variables**:
```bash
EXPO_PUBLIC_API_URL=https://api.checkin.app
EXPO_PUBLIC_ENVIRONMENT=production
EXPO_PUBLIC_SENTRY_DSN=https://...
```

**Resource Requirements**:
- Development: 2GB RAM, 1 CPU
- Build: 4GB RAM, 2 CPU
- Disk: 500MB

#### **Web Browser Container**
**Technology Stack**:
- Framework: React 18 + TypeScript
- Build Tool: Vite
- State Management: Zustand
- HTTP Client: Axios with interceptors
- UI Framework: Shadcn/ui + Tailwind CSS

**Container Specifications**:
```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Environment Variables**:
```bash
VITE_API_URL=https://api.checkin.app
VITE_ENVIRONMENT=production
VITE_SENTRY_DSN=https://...
```

### 2.2 API Gateway Container

#### **Kong API Gateway**
**Technology Stack**:
- Kong Gateway (Open Source)
- Kong Admin API
- Plugins: Rate Limiting, JWT, CORS, Logging

**Container Specifications**:
```dockerfile
FROM kong:3.4-alpine
COPY kong.yml /opt/kong/kong.yml
ENV KONG_DATABASE=off
ENV KONG_DECLARATIVE_CONFIG=/opt/kong/kong.yml
ENV KONG_PROXY_ACCESS_LOG=/dev/stdout
ENV KONG_ADMIN_ACCESS_LOG=/dev/stdout
ENV KONG_PROXY_ERROR_LOG=/dev/stderr
ENV KONG_ADMIN_ERROR_LOG=/dev/stderr
EXPOSE 8000 8001 8443 8444
```

**Configuration**:
```yaml
# kong.yml
_format_version: "3.0"
services:
  - name: recommendation-service
    url: http://recommendation-service:8001
    routes:
      - name: recommendations
        paths: ["/api/recommendations"]
        methods: ["GET", "POST"]
    plugins:
      - name: jwt
      - name: rate-limiting
        config:
          minute: 100
          hour: 1000

  - name: social-api
    url: http://social-api:8002
    routes:
      - name: social
        paths: ["/api/social"]
        methods: ["GET", "POST", "PUT", "DELETE"]
```

**Resource Requirements**:
- Production: 1GB RAM, 0.5 CPU
- High Load: 2GB RAM, 1 CPU

### 2.3 Backend Service Containers

#### **Recommendation Service Container**
**Technology Stack**:
- Framework: FastAPI + Uvicorn
- AI Integration: OpenAI Python SDK
- Database: AsyncPG + SQLAlchemy
- Caching: Redis-py

**Container Specifications**:
```dockerfile
FROM python:3.11-slim
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application
COPY . .

# Create non-root user
RUN useradd --create-home --shell /bin/bash checkin
USER checkin

EXPOSE 8001
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8001"]
```

**Environment Variables**:
```bash
DATABASE_URL=postgresql+asyncpg://user:pass@postgres:5432/checkin
REDIS_URL=redis://redis:6379/0
OPENAI_API_KEY=sk-...
SENTRY_DSN=https://...
LOG_LEVEL=INFO
```

**Health Check**:
```dockerfile
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8001/health || exit 1
```

#### **Social API Container**
**Technology Stack**:
- Framework: FastAPI + Uvicorn
- WebSockets: FastAPI WebSocket support
- Background Tasks: Celery + Redis
- Email: SendGrid integration

**Container Specifications**:
```dockerfile
FROM python:3.11-slim
WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

RUN useradd --create-home --shell /bin/bash social
USER social

EXPOSE 8002
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8002"]
```

#### **Venue API Container**
**Technology Stack**:
- Framework: FastAPI + Uvicorn
- External APIs: Google Places, Foursquare
- Background Jobs: Celery for data sync
- File Storage: AWS S3 for images

**Container Specifications**:
```dockerfile
FROM python:3.11-slim
WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

RUN useradd --create-home --shell /bin/bash venue
USER venue

EXPOSE 8003
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8003"]
```

### 2.4 Data Store Containers

#### **PostgreSQL Container**
**Technology Stack**:
- PostgreSQL 15
- Extensions: PostGIS, pg_stat_statements
- Backup: pg_dump automated

**Container Specifications**:
```dockerfile
FROM postgres:15-alpine
RUN apk add --no-cache postgis

# Copy initialization scripts
COPY init-scripts/ /docker-entrypoint-initdb.d/

# Set up backup script
COPY backup.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/backup.sh
```

**Environment Variables**:
```bash
POSTGRES_DB=checkin
POSTGRES_USER=checkin_user
POSTGRES_PASSWORD=secure_password
POSTGRES_INITDB_ARGS=--encoding=UTF-8 --lc-collate=C --lc-ctype=C
```

**Persistent Volumes**:
```yaml
volumes:
  postgres_data:
    driver: local
  postgres_backups:
    driver: local
```

#### **Redis Container**
**Technology Stack**:
- Redis 7
- Redis Modules: RedisJSON, RedisSearch
- Persistence: RDB + AOF

**Container Specifications**:
```dockerfile
FROM redis:7-alpine
COPY redis.conf /usr/local/etc/redis/redis.conf
CMD ["redis-server", "/usr/local/etc/redis/redis.conf"]
```

**Configuration**:
```conf
# redis.conf
maxmemory 1gb
maxmemory-policy allkeys-lru
save 900 1
save 300 10
save 60 10000
appendonly yes
appendfsync everysec
```

---

## 3. Container Communication Patterns

### 3.1 Synchronous Communication

#### **API-to-API Communication**
```python
# Recommendation Service calling Venue API
class VenueAPIClient:
    def __init__(self, base_url: str, timeout: int = 30):
        self.client = httpx.AsyncClient(
            base_url=base_url,
            timeout=timeout,
            headers={"Service-Name": "recommendation-service"}
        )
    
    async def get_venue_details(self, venue_id: str) -> VenueDetails:
        response = await self.client.get(f"/venues/{venue_id}")
        response.raise_for_status()
        return VenueDetails(**response.json())
```

#### **Circuit Breaker Pattern**
```python
from circuitbreaker import circuit

@circuit(failure_threshold=5, recovery_timeout=30)
async def call_external_service(url: str) -> dict:
    async with httpx.AsyncClient() as client:
        response = await client.get(url)
        response.raise_for_status()
        return response.json()
```

### 3.2 Asynchronous Communication

#### **Message Queue Integration**
```python
# Publisher (Social API)
class EventPublisher:
    def __init__(self, redis_client: Redis):
        self.redis = redis_client
    
    async def publish_user_checked_in(self, user_id: str, venue_id: str):
        event = {
            "type": "user_checked_in",
            "user_id": user_id,
            "venue_id": venue_id,
            "timestamp": datetime.utcnow().isoformat()
        }
        await self.redis.lpush("events:check_in", json.dumps(event))

# Consumer (Recommendation Service)
class EventConsumer:
    async def process_check_in_events(self):
        while True:
            event_data = await self.redis.brpop("events:check_in", timeout=30)
            if event_data:
                event = json.loads(event_data[1])
                await self.update_user_preferences(event)
```

### 3.3 Database Access Patterns

#### **Connection Pooling**
```python
from sqlalchemy.ext.asyncio import create_async_engine, async_sessionmaker

engine = create_async_engine(
    DATABASE_URL,
    pool_size=10,
    max_overflow=20,
    pool_pre_ping=True,
    pool_recycle=3600
)

SessionLocal = async_sessionmaker(
    engine,
    expire_on_commit=False
)
```

---

## 4. Container Orchestration

### 4.1 Docker Compose Development

#### **docker-compose.yml**
```yaml
version: '3.8'

services:
  # API Gateway
  api-gateway:
    image: kong:3.4-alpine
    environment:
      KONG_DATABASE: "off"
      KONG_DECLARATIVE_CONFIG: /opt/kong/kong.yml
    volumes:
      - ./kong.yml:/opt/kong/kong.yml
    ports:
      - "8000:8000"
      - "8001:8001"
    depends_on:
      - recommendation-service
      - social-api
      - venue-api

  # Backend Services
  recommendation-service:
    build:
      context: ./services/recommendation
      dockerfile: Dockerfile
    environment:
      DATABASE_URL: postgresql+asyncpg://checkin:password@postgres:5432/checkin
      REDIS_URL: redis://redis:6379/0
      OPENAI_API_KEY: ${OPENAI_API_KEY}
    depends_on:
      - postgres
      - redis
    ports:
      - "8001:8001"

  social-api:
    build:
      context: ./services/social
      dockerfile: Dockerfile
    environment:
      DATABASE_URL: postgresql+asyncpg://checkin:password@postgres:5432/checkin
      REDIS_URL: redis://redis:6379/1
    depends_on:
      - postgres
      - redis
    ports:
      - "8002:8002"

  venue-api:
    build:
      context: ./services/venue
      dockerfile: Dockerfile
    environment:
      DATABASE_URL: postgresql+asyncpg://checkin:password@postgres:5432/checkin
      GOOGLE_PLACES_API_KEY: ${GOOGLE_PLACES_API_KEY}
    depends_on:
      - postgres
    ports:
      - "8003:8003"

  # Databases
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: checkin
      POSTGRES_USER: checkin
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init-scripts:/docker-entrypoint-initdb.d
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    command: redis-server --appendonly yes
    volumes:
      - redis_data:/data
    ports:
      - "6379:6379"

  # Frontend
  web-app:
    build:
      context: ./frontend
      dockerfile: Dockerfile
      target: development
    environment:
      VITE_API_URL: http://localhost:8000
    volumes:
      - ./frontend:/app
      - /app/node_modules
    ports:
      - "3000:3000"

volumes:
  postgres_data:
  redis_data:

networks:
  default:
    name: checkin-network
```

### 4.2 Kubernetes Production

#### **namespace.yaml**
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: checkin-production
  labels:
    name: checkin-production
```

#### **deployment-recommendation-service.yaml**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: recommendation-service
  namespace: checkin-production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: recommendation-service
  template:
    metadata:
      labels:
        app: recommendation-service
    spec:
      containers:
      - name: recommendation-service
        image: checkin/recommendation-service:v1.2.3
        ports:
        - containerPort: 8001
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: recommendation-db-url
        - name: REDIS_URL
          valueFrom:
            configMapKeyRef:
              name: redis-config
              key: url
        - name: OPENAI_API_KEY
          valueFrom:
            secretKeyRef:
              name: openai-credentials
              key: api-key
        resources:
          requests:
            memory: "512Mi"
            cpu: "250m"
          limits:
            memory: "1Gi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8001
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8001
          initialDelaySeconds: 5
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: recommendation-service
  namespace: checkin-production
spec:
  selector:
    app: recommendation-service
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8001
  type: ClusterIP
```

#### **hpa.yaml (Horizontal Pod Autoscaler)**
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: recommendation-service-hpa
  namespace: checkin-production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: recommendation-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

---

## 5. Container Security

### 5.1 Security Best Practices

#### **Non-root User in Containers**
```dockerfile
# Create user in each service container
RUN groupadd -r checkin && useradd -r -g checkin checkin
USER checkin
```

#### **Secrets Management**
```yaml
# Kubernetes Secret
apiVersion: v1
kind: Secret
metadata:
  name: app-secrets
  namespace: checkin-production
type: Opaque
data:
  database-url: <base64-encoded-url>
  openai-api-key: <base64-encoded-key>
  jwt-secret: <base64-encoded-secret>
```

#### **Network Policies**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: recommendation-service-policy
  namespace: checkin-production
spec:
  podSelector:
    matchLabels:
      app: recommendation-service
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: api-gateway
    ports:
    - protocol: TCP
      port: 8001
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: postgres
    ports:
    - protocol: TCP
      port: 5432
  - to:
    - podSelector:
        matchLabels:
          app: redis
    ports:
    - protocol: TCP
      port: 6379
```

### 5.2 Container Scanning

#### **Security Scanning Pipeline**
```yaml
# .github/workflows/security-scan.yml
name: Container Security Scan
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  security-scan:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    
    - name: Build container
      run: docker build -t checkin/app:${{ github.sha }} .
    
    - name: Run Trivy vulnerability scanner
      uses: aquasecurity/trivy-action@master
      with:
        image-ref: 'checkin/app:${{ github.sha }}'
        format: 'sarif'
        output: 'trivy-results.sarif'
    
    - name: Upload Trivy scan results
      uses: github/codeql-action/upload-sarif@v1
      with:
        sarif_file: 'trivy-results.sarif'
```

---

## 6. Monitoring e Observabilidade

### 6.1 Metrics Collection

#### **Prometheus Configuration**
```yaml
# prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'recommendation-service'
    static_configs:
      - targets: ['recommendation-service:8001']
    metrics_path: '/metrics'
    
  - job_name: 'social-api'
    static_configs:
      - targets: ['social-api:8002']
    metrics_path: '/metrics'
    
  - job_name: 'venue-api'
    static_configs:
      - targets: ['venue-api:8003']
    metrics_path: '/metrics'
```

#### **Service Metrics Instrumentation**
```python
from prometheus_client import Counter, Histogram, generate_latest

# Metrics
request_count = Counter('http_requests_total', 'Total HTTP requests', ['method', 'endpoint'])
request_duration = Histogram('http_request_duration_seconds', 'HTTP request duration')

@app.middleware("http")
async def add_metrics(request: Request, call_next):
    start_time = time.time()
    response = await call_next(request)
    
    request_count.labels(
        method=request.method,
        endpoint=request.url.path
    ).inc()
    
    request_duration.observe(time.time() - start_time)
    return response

@app.get("/metrics")
async def metrics():
    return Response(generate_latest(), media_type="text/plain")
```

### 6.2 Logging Strategy

#### **Structured Logging**
```python
import structlog

logger = structlog.get_logger()

class LoggingMiddleware:
    async def __call__(self, request: Request, call_next):
        logger.info(
            "request_started",
            method=request.method,
            path=request.url.path,
            user_id=getattr(request.state, 'user_id', None)
        )
        
        try:
            response = await call_next(request)
            logger.info(
                "request_completed",
                status_code=response.status_code,
                duration=time.time() - start_time
            )
            return response
        except Exception as e:
            logger.error(
                "request_failed",
                error=str(e),
                exception_type=type(e).__name__
            )
            raise
```

#### **Log Aggregation with ELK Stack**
```yaml
# filebeat.yml
filebeat.inputs:
- type: container
  paths:
    - '/var/lib/docker/containers/*/*.log'
  processors:
    - add_docker_metadata:
        host: "unix:///var/run/docker.sock"

output.elasticsearch:
  hosts: ["elasticsearch:9200"]
  index: "checkin-logs-%{+yyyy.MM.dd}"

setup.template.name: "checkin"
setup.template.pattern: "checkin-*"
```

---

## 7. Performance e Scaling

### 7.1 Container Resource Management

#### **Resource Limits per Container**
```yaml
# Resource allocation strategy
resources:
  recommendation-service:
    requests:
      memory: "512Mi"
      cpu: "250m"
    limits:
      memory: "2Gi"
      cpu: "1000m"
  
  social-api:
    requests:
      memory: "256Mi"
      cpu: "100m"
    limits:
      memory: "1Gi"
      cpu: "500m"
  
  venue-api:
    requests:
      memory: "256Mi"
      cpu: "100m"
    limits:
      memory: "1Gi"
      cpu: "500m"
      
  postgres:
    requests:
      memory: "1Gi"
      cpu: "500m"
    limits:
      memory: "4Gi"
      cpu: "2000m"
      
  redis:
    requests:
      memory: "128Mi"
      cpu: "50m"
    limits:
      memory: "512Mi"
      cpu: "200m"
```

### 7.2 Auto-scaling Configuration

#### **Vertical Pod Autoscaler**
```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: recommendation-service-vpa
  namespace: checkin-production
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: recommendation-service
  updatePolicy:
    updateMode: "Auto"
  resourcePolicy:
    containerPolicies:
    - containerName: recommendation-service
      maxAllowed:
        cpu: 2
        memory: 4Gi
      minAllowed:
        cpu: 100m
        memory: 128Mi
```

### 7.3 Load Testing Strategy

#### **Container Load Testing**
```python
# locustfile.py
from locust import HttpUser, task, between

class CheckInUser(HttpUser):
    wait_time = between(1, 3)
    
    def on_start(self):
        # Login and get auth token
        response = self.client.post("/auth/login", json={
            "email": "test@example.com",
            "password": "testpass"
        })
        self.token = response.json()["access_token"]
        self.headers = {"Authorization": f"Bearer {self.token}"}
    
    @task(3)
    def get_recommendations(self):
        self.client.post("/api/recommendations", 
                        json={"context": {"location": {"lat": -23.5, "lng": -46.6}}},
                        headers=self.headers)
    
    @task(1)
    def create_event(self):
        self.client.post("/api/social/events",
                        json={"title": "Test Event", "venue_id": "123"},
                        headers=self.headers)
```

---

## 8. Disaster Recovery e Backup

### 8.1 Database Backup Strategy

#### **PostgreSQL Backup Container**
```dockerfile
FROM postgres:15-alpine
RUN apk add --no-cache aws-cli
COPY backup-script.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/backup-script.sh
CMD ["/usr/local/bin/backup-script.sh"]
```

#### **Backup Script**
```bash
#!/bin/bash
# backup-script.sh

DB_NAME=${POSTGRES_DB}
DB_USER=${POSTGRES_USER}
DB_HOST=${POSTGRES_HOST}
BACKUP_DIR="/backups"
S3_BUCKET=${BACKUP_S3_BUCKET}

# Create backup
pg_dump -h $DB_HOST -U $DB_USER -d $DB_NAME -F c -b -v -f $BACKUP_DIR/backup_$(date +%Y%m%d_%H%M%S).backup

# Upload to S3
aws s3 cp $BACKUP_DIR/*.backup s3://$S3_BUCKET/database-backups/

# Cleanup old local backups (keep 7 days)
find $BACKUP_DIR -name "*.backup" -mtime +7 -delete
```

### 8.2 Container Recovery Procedures

#### **Rolling Update Strategy**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: recommendation-service
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    spec:
      containers:
      - name: recommendation-service
        image: checkin/recommendation-service:v1.3.0
        readinessProbe:
          httpGet:
            path: /ready
            port: 8001
          initialDelaySeconds: 5
          periodSeconds: 5
        livenessProbe:
          httpGet:
            path: /health
            port: 8001
          initialDelaySeconds: 30
          periodSeconds: 10
```

---

## 9. Development Workflow

### 9.1 Local Development Environment

#### **Development docker-compose.override.yml**
```yaml
version: '3.8'

services:
  recommendation-service:
    build:
      target: development
    volumes:
      - ./services/recommendation:/app
      - /app/.venv
    environment:
      DEBUG: "true"
      RELOAD: "true"
    command: ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8001", "--reload"]

  social-api:
    build:
      target: development
    volumes:
      - ./services/social:/app
      - /app/.venv
    environment:
      DEBUG: "true"
      RELOAD: "true"
    command: ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8002", "--reload"]

  web-app:
    volumes:
      - ./frontend:/app
      - /app/node_modules
    environment:
      VITE_API_URL: http://localhost:8000
      VITE_HOT_RELOAD: "true"
```

### 9.2 CI/CD Pipeline for Containers

#### **Build and Deploy Pipeline**
```yaml
# .github/workflows/deploy.yml
name: Build and Deploy

on:
  push:
    branches: [main]
    
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        service: [recommendation-service, social-api, venue-api]
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v1
    
    - name: Login to DockerHub
      uses: docker/login-action@v1
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}
    
    - name: Build and push
      uses: docker/build-push-action@v2
      with:
        context: ./services/${{ matrix.service }}
        push: true
        tags: checkin/${{ matrix.service }}:${{ github.sha }}
        cache-from: type=gha
        cache-to: type=gha,mode=max
    
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
    - name: Deploy to Kubernetes
      run: |
        # Update deployment images
        kubectl set image deployment/recommendation-service \
          recommendation-service=checkin/recommendation-service:${{ github.sha }} \
          -n checkin-production
        
        kubectl set image deployment/social-api \
          social-api=checkin/social-api:${{ github.sha }} \
          -n checkin-production
        
        kubectl set image deployment/venue-api \
          venue-api=checkin/venue-api:${{ github.sha }} \
          -n checkin-production
```

---

**Responsável**: Lucas Emmanuel (Infrastructure) + Henrique Fontaine (Architecture)  
**Manutenção**: Revisão mensal da arquitetura de containers  
**Monitoramento**: 24/7 via Prometheus + Grafana  
**Última atualização**: Dezembro 2024
