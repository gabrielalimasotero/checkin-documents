# Canvas de Mapeamento de Fontes de Dados

Nota: Estruturado por fonte, seguindo o template (itens 1–12).

### Fonte A: Perfis de Usuário (Interna)
1. Nome da Fonte de Dados: Perfis de Usuário
2. Descrição da Fonte de Dados: Dados cadastrais e preferências usadas para personalização e privacidade.
3. Origem dos Dados: App CheckIN (onboarding e edição de perfil).
4. Tipo de Dados: Textual, categórico, temporal (criação/atualização).
5. Formato dos Dados: Tabelas SQL (`users`, `user_preferences`) em PostgreSQL (JSONB para preferências).
6. Frequência de Atualização: Event-driven (onboarding/edições) e esporádica.
7. Qualidade dos Dados: Alta; campos obrigatórios validados; possíveis lacunas em preferências.
8. Métodos de Coleta: Formulários no app; coleta progressiva.
9. Acesso aos Dados: API REST autenticada (`/users/me`, `/users/preferences`).
10. Proprietário dos Dados: Produto/Aplicativo CheckIN (DPO indicado pela equipe).
11. Restrições de Privacidade e Segurança: LGPD; consentimento; criptografia em trânsito/repouso; controle de acesso por escopos.
12. Requisitos de Integração: DTOs tipados; mapeamento JSONB→features; versionamento de schema (migrations).

### Fonte B: Check-ins e Avaliações (Interna)
1. Nome da Fonte de Dados: Check-ins e Avaliações
2. Descrição da Fonte de Dados: Registro de presença e feedback leve (rating, tags de vibe).
3. Origem dos Dados: App CheckIN (tela de check-in; pós-visita).
4. Tipo de Dados: Temporal, geoespacial, categórico, numérico.
5. Formato dos Dados: Tabelas SQL (`checkins`, `reviews`, `review_tags`) em PostgreSQL.
6. Frequência de Atualização: Tempo real por evento de usuário.
7. Qualidade dos Dados: Alta para check-ins; média para avaliações (subjetivas); mitigada por volume/decay.
8. Métodos de Coleta: Ações no app; QR code opcional; prompts pós-check-in.
9. Acesso aos Dados: API REST (`/checkins`, `/venues/{id}/checkins`, `/reviews`).
10. Proprietário dos Dados: Produto/Aplicativo CheckIN.
11. Restrições de Privacidade e Segurança: Visibilidade configurável (público/amigos/privado); retenção mínima 6 meses.
12. Requisitos de Integração: Normalização de tags; geofencing; agregações por janela temporal.

### Fonte C: Interações Sociais e Convites (Interna)
1. Nome da Fonte de Dados: Interações Sociais e Convites/RSVP
2. Descrição da Fonte de Dados: Convites, confirmações de presença, mensagens e relacionamentos.
3. Origem dos Dados: App CheckIN (abas Convites, Mensagens, Network).
4. Tipo de Dados: Relacional, temporal, textual.
5. Formato dos Dados: Tabelas SQL (`events`, `rsvps`, `friendships`, `messages`).
6. Frequência de Atualização: Contínua, tempo real.
7. Qualidade dos Dados: Alta; fluxos guiados; validações de estado.
8. Métodos de Coleta: Ações do usuário; notificações contextuais.
9. Acesso aos Dados: API REST (`/events`, `/events/{id}/attendees`, `/friendships`, `/messages`).
10. Proprietário dos Dados: Produto/Aplicativo CheckIN.
11. Restrições de Privacidade e Segurança: Consentimento; retenção mínima; opt-out de mensagens; criptografia.
12. Requisitos de Integração: Webhooks para notificações; filas para fan-out; idempotência.

---

## 2. Fontes de Dados Identificadas

### 2.1 Fontes Internas (App CheckIn)
| Fonte | Tipo de Dado | Frequência | Qualidade Esperada |
|-------|--------------|------------|-------------------|
| **Perfis de Usuário** | Dados demográficos, preferências | Uma vez | Alta |
| **Check-ins** | Localização, horário, duração | Por visita | Alta |
| **Avaliações** | Rating, comentários, tags de vibe | Pós-check-in | Média-Alta |
| **Interações Sociais** | Convites, RSVPs, mensagens | Contínua | Alta |
| **Comportamento no App** | Cliques, tempo, fluxos | Contínua | Alta |

### Fonte D: Comportamento no App/Analytics (Interna)
1. Nome da Fonte de Dados: Eventos de Uso/Analytics
2. Descrição da Fonte de Dados: Cliques, tempo até decisão, uso de sugestões, salvamentos/rejeições.
3. Origem dos Dados: SDK de analytics no app.
4. Tipo de Dados: Temporal, categórico, numérico.
5. Formato dos Dados: Eventos JSON (batch/stream) em data store (S3/BigQuery futuro).
6. Frequência de Atualização: Quase tempo real (batch a cada 1–5 min).
7. Qualidade dos Dados: Alta com esquema versionado; risco de perda em offline mitigado por fila local.
8. Métodos de Coleta: Instrumentação de eventos; buffer e envio.
9. Acesso aos Dados: Dashboard interno; exportação parquet/CSV.
10. Proprietário dos Dados: Produto/Analytics.
11. Restrições de Privacidade e Segurança: Pseudonimização; consentimento de tracking; retenção por finalidade.
12. Requisitos de Integração: Dicionário de eventos; schema registry; jobs de transformação para métricas.

### Fonte E: TripAdvisor (Externa)
1. Nome da Fonte de Dados: TripAdvisor Venues
2. Descrição da Fonte de Dados: Informações de estabelecimentos (nome, endereço, categoria, fotos, rating, reviews resumidas) usadas para catálogo inicial de venues.
3. Origem dos Dados: TripAdvisor (APIs públicas limitadas/parcerias; onde necessário, coleta assistida respeitando ToS).
4. Tipo de Dados: Textual, categórico, geoespacial.
5. Formato dos Dados: JSON via API quando disponível; CSV/planilhas para importações; eventualmente HTML parseado sob conformidade.
6. Frequência de Atualização: Importações periódicas (semanal/mensal); cache 30 dias para atributos estáticos.
7. Qualidade dos Dados: Alta para metadados básicos; reviews podem estar desatualizados; complementar com dados internos.
8. Métodos de Coleta: API/feeds oficiais; importação manual assistida; nunca scraping agressivo; respeito a robots/ToS.
9. Acesso aos Dados: Serviço backend com normalização e cache Redis.
10. Proprietário dos Dados: TripAdvisor; uso condicionado a licenças/ToS e atribuição.
11. Restrições de Privacidade e Segurança: Sem PII; somente dados públicos/licenciados; auditoria de uso e retenção.
12. Requisitos de Integração: Deduplicação com dados internos; normalização de categorias; mapeamento de IDs; controle de atualizações.

### Fonte F: OpenWeather (Externa)
1. Nome da Fonte de Dados: OpenWeather
2. Descrição da Fonte de Dados: Clima atual e previsão para contexto de recomendação.
3. Origem dos Dados: API pública OpenWeather.
4. Tipo de Dados: Numérico, temporal, categórico.
5. Formato dos Dados: JSON via REST.
6. Frequência de Atualização: 5–15 min; cache 15 min.
7. Qualidade dos Dados: Alta; precisão varia por região.
8. Métodos de Coleta: Chamadas REST programáticas.
9. Acesso aos Dados: Serviço backend com cache Redis.
10. Proprietário dos Dados: OpenWeather.
11. Restrições de Privacidade e Segurança: Sem PII; chaves seguras; limites de uso.
12. Requisitos de Integração: Normalização de códigos meteorológicos; fallback.

---

## 3. Estratégia de Coleta por Fonte

### 3.1 Dados Internos - Estratégia de Captura
#### **Onboarding Inteligente**
- Questionário inicial não invasivo (5-7 perguntas)
- Coleta progressiva baseada em uso
- Incentivos para preenchimento completo (sugestões melhores)

#### **Coleta Implícita**
- Tracking de comportamento com consentimento
- Análise de padrões de check-in para inferir preferências
- Machine learning para categorizar automaticamente

#### **Gamificação da Contribuição**
- Pontos por avaliar locais visitados
- Badges por contribuir com dados de qualidade
- Ranking de "exploradores" da cidade

### 3.2 APIs Externas - Implementação
#### **Arquitetura de Consumo**
```
API Gateway → Rate Limiter → Cache Layer → Data Processor → Database
```

#### **Estratégia de Cache**
- **Dados estáticos** (info estabelecimentos): Cache 30 dias
- **Dados dinâmicos** (clima, trânsito): Cache 15 minutos
- **Dados de eventos**: Cache 6 horas
- **Invalidação inteligente** baseada em mudanças

#### **Fallback e Redundância**
- Múltiplas fontes para dados críticos
- Graceful degradation quando APIs falham
- Dados históricos como backup

### Fonte G: Google Maps Distance/Routes (Externa)
1. Nome da Fonte de Dados: Google Maps Distance Matrix/Routes
2. Descrição da Fonte de Dados: Distâncias, tempo de deslocamento e trânsito.
3. Origem dos Dados: Google Maps Platform.
4. Tipo de Dados: Numérico, temporal, geoespacial.
5. Formato dos Dados: JSON via REST.
6. Frequência de Atualização: Sob demanda; cache 5–15 min.
7. Qualidade dos Dados: Alta.
8. Métodos de Coleta: Chamadas REST; batching quando possível.
9. Acesso aos Dados: Serviço backend; chaves geridas por vault.
10. Proprietário dos Dados: Google.
11. Restrições de Privacidade e Segurança: ToS; sem armazenar trajetos pessoais persistentemente.
12. Requisitos de Integração: Geocoding consistente; quotas; tratamento de erros.

---

## 4. Qualidade e Confiabilidade dos Dados

### 4.1 Métricas de Qualidade por Fonte
| Fonte | Completude | Atualidade | Precisão | Relevância |
|-------|------------|------------|----------|------------|
| **Check-ins Usuários** | 95% | Tempo real | 90% | Alta |
| **Google Places** | 80% | Variável | 95% | Alta |
| **Reviews Scraping** | 60% | Desatualizada | 70% | Média |
| **Parcerias Diretas** | 100% | Tempo real | 95% | Alta |
| **Dados Climáticos** | 100% | Tempo real | 85% | Média |

### 4.2 Sistema de Validação
#### **Validação Cruzada**
- Comparar dados de múltiplas fontes
- Flagging automático de discrepâncias
- Confiança baseada em consenso

#### **Validação por Comunidade**
- Usuários podem reportar informações incorretas
- Sistema de votação para validar correções
- Moderação automatizada + manual

#### **Machine Learning para Qualidade**
- Modelos que detectam dados suspeitos
- Análise de padrões para identificar outliers
- Scoring automático de confiabilidade

---

## 5. Estratégia de Dados para IA

### 5.1 Alimentação do Sistema de Recomendações
#### **Features Principais**
- **Contextuais**: Horário, localização, clima, companhia
- **Comportamentais**: Histórico, preferências, padrões
- **Sociais**: Amigos presentes, eventos, tendências
- **Estabelecimento**: Tipo, vibe, preço, disponibilidade

#### **Pipeline de Dados**
```
Raw Data → Feature Engineering → ML Pipeline → Recommendation Engine → User Interface
```

### 5.2 Treinamento e Melhoria Contínua
#### **Feedback Loop**
- Sugestões aceitas/rejeitadas alimentam modelo
- A/B testing de diferentes algoritmos
- Retreino semanal com novos dados

#### **Dados de Treinamento**
- Histórico de decisões de usuários similares
- Padrões sazonais e temporais
- Feedback explícito sobre recomendações

---

## 6. Compliance e Privacidade

### 6.1 LGPD e Proteção de Dados
#### **Dados Pessoais**
- **Consentimento explícito** para coleta
- **Minimização**: Coletar apenas o necessário
- **Transparência**: Explicar uso claro dos dados
- **Direito ao esquecimento**: Processo de deleção

#### **Anonimização e Agregação**
- Dados comportamentais agregados para insights
- Remoção de identificadores pessoais em análises
- Pseudonimização para dados de treinamento de IA

### 6.2 Termos de Uso de APIs
#### **Compliance com ToS**
- Respeitar rate limits rigorosamente
- Não revender dados obtidos via APIs
- Atribuição adequada quando exigida
- Monitoramento de mudanças nos termos

---

## 7. Infraestrutura de Dados

### 7.1 Arquitetura de Armazenamento
#### **Banco Principal** (PostgreSQL)
- Dados estruturados de usuários e estabelecimentos
- Transações e relacionamentos
- Backup diário automatizado

#### **Cache** (Redis)
- Dados de APIs externas com TTL
- Sessões de usuário
- Resultados de queries frequentes

#### **Data Lake** (futuramente)
- Dados brutos de múltiplas fontes
- Analytics e machine learning
- Retenção de longo prazo

### 7.2 ETL e Processamento
#### **Pipelines Automatizados**
- Ingestão noturna de dados estáticos
- Streaming para dados em tempo real
- Monitoramento e alertas de falhas

#### **Processamento**
- Limpeza e normalização automática
- Detecção de anomalias
- Enriquecimento com dados contextuais

---

## 8. Roadmap de Implementação

### 8.1 Fase 1 - MVP (Primeiros 3 meses)
**Fontes Prioritárias**:
- ✅ Dados internos (perfis, check-ins, avaliações)
- ✅ Google Places API (informações básicas)
- ✅ OpenWeather API (contexto climático)
- ✅ Dados manuais de 25 estabelecimentos piloto

**Infraestrutura**:
- PostgreSQL para dados principais
- Redis para cache básico
- Scripts Python para ETL simples

### 8.2 Fase 2 - Crescimento (3-6 meses)
**Expansão de Fontes**:
- 🔄 Parcerias com 100 estabelecimentos
- 🔄 Foursquare API para popularidade
- 🔄 Google Maps API para trânsito
- 🔄 Programa de influenciadores (10-15 pessoas)

**Melhorias**:
- Sistema de validação automática
- Dashboard de qualidade de dados
- APIs internas para parceiros

### 8.3 Fase 3 - Escala (6+ meses)
**Fontes Avançadas**:
- 🔄 Web scraping estruturado
- 🔄 Parcerias institucionais
- 🔄 Data enrichment services
- 🔄 IoT data (futuro - sensors de movimento)

**Infraestrutura Avançada**:
- Data lake para analytics
- Machine learning pipeline completo
- Real-time processing com Kafka

---

## 9. Métricas de Sucesso

### 9.1 KPIs de Qualidade dos Dados
| Métrica | Meta MVP | Meta 6 meses | Ferramenta |
|---------|----------|--------------|------------|
| **Cobertura de Estabelecimentos** | 80% da região | 95% da região | Dashboard interno |
| **Atualidade Média** | <24h | <6h | Monitoring sistema |
| **Precisão de Recomendações** | >70% | >85% | Feedback usuários |
| **Uptime de APIs** | >95% | >99% | Monitoring externo |

### 9.2 KPIs de Uso dos Dados
| Métrica | Meta MVP | Meta 6 meses | Impacto |
|---------|----------|--------------|---------|
| **Dados por Recomendação** | 5+ sources | 10+ sources | Qualidade sugestões |
| **Tempo de Resposta** | <2s | <1s | UX |
| **Cache Hit Rate** | >80% | >90% | Performance |
| **Contribuições Usuários** | 20% | 60% | Engajamento |

---

## 10. Riscos e Mitigações

### 10.1 Riscos Identificados
#### **Dependência de APIs Externas**
- **Risco**: Mudanças de pricing, deprecação
- **Mitigação**: Múltiplas fontes, cache extensivo, partnerships

#### **Qualidade de Dados Terceiros**
- **Risco**: Dados incorretos, desatualizados
- **Mitigação**: Validação cruzada, feedback comunidade

#### **Compliance e Privacidade**
- **Risco**: Violação LGPD, mudanças regulatórias
- **Mitigação**: Privacy by design, auditoria regular

### 10.2 Plano de Contingência
- **Fallback para dados históricos** quando APIs falham
- **Modo degradado** com funcionalidades limitadas
- **Partnerships de backup** para fontes críticas
