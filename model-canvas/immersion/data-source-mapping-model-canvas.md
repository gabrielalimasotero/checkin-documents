# Canvas de Mapeamento de Fontes de Dados - CheckIn

## Visão Geral
Este canvas mapeia todas as fontes de dados necessárias para o funcionamento do CheckIn, identificando origens, qualidade, disponibilidade e estratégias de coleta para alimentar o sistema de recomendações por IA e funcionalidades sociais.

---

## 1. Categorização de Dados Necessários

### 1.1 Dados de Usuários
**Dados Primários** (coletados diretamente):
- Perfil básico (nome, idade, localização)
- Preferências alimentares e restrições
- Histórico de check-ins e avaliações
- Padrões de comportamento (horários, tipos de local)
- Conexões sociais (amigos, grupos)

**Dados Comportamentais** (coletados por uso):
- Tempo de permanência em locais
- Frequência de uso do app
- Padrões de navegação e interação
- Feedback sobre sugestões da IA
- Interações sociais (convites enviados/aceitos)

### 1.2 Dados de Estabelecimentos
**Dados Básicos**:
- Nome, endereço, categoria, horário funcionamento
- Cardápio e faixa de preço
- Capacidade, tipo de ambiente
- Contatos e informações de reserva

**Dados Dinâmicos**:
- Lotação em tempo real
- Status operacional (aberto/fechado)
- Promoções e eventos especiais
- Avaliações e feedback recentes

### 1.3 Dados Contextuais
**Temporais**:
- Horário atual, dia da semana
- Feriados e eventos locais
- Sazonalidade e tendências

**Ambientais**:
- Clima atual e previsão
- Trânsito e tempo de deslocamento
- Eventos próximos que podem afetar movimento

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

### 2.2 Fontes Externas - APIs Públicas
| Fonte | Tipo de Dado | API/Serviço | Custo | Limitações |
|-------|--------------|-------------|-------|------------|
| **Google Places** | Info estabelecimentos | Google Places API | $17/1000 requests | Rate limiting |
| **Foursquare** | Check-ins, popularidade | Foursquare API | Freemium | Dados limitados |
| **OpenWeather** | Clima atual/previsão | OpenWeather API | $1/1000 calls | Precisão regional |
| **Google Maps** | Trânsito, distâncias | Google Maps API | $5/1000 requests | Cota diária |
| **Eventbrite** | Eventos locais | Eventbrite API | Gratuito | Apenas eventos cadastrados |

### 2.3 Fontes Externas - Parcerias
| Fonte | Tipo de Dado | Estratégia de Aquisição | Benefício Mútuo |
|-------|--------------|------------------------|-----------------|
| **Restaurantes Locais** | Cardápio, disponibilidade | Parceria direta | Visibilidade e reservas |
| **Influenciadores Locais** | Recomendações curadas | Programa de embaixadores | Exposição e engajamento |
| **Universidades** | Eventos acadêmicos | Parcerias institucionais | Engajamento estudantil |
| **Centros Comerciais** | Eventos e promoções | Acordos comerciais | Marketing conjunto |

### 2.4 Fontes Externas - Web Scraping
| Fonte | Tipo de Dado | Método | Frequência | Riscos |
|-------|--------------|--------|------------|--------|
| **Instagram** | Posts geolocalizados | Hashtag scraping | Diária | Mudanças na API |
| **Facebook Events** | Eventos públicos | Graph API | Semanal | Restrições de acesso |
| **Sites de Estabelecimentos** | Cardápios atualizados | Web scraping | Semanal | Bloqueios, mudanças |
| **TripAdvisor/Google Reviews** | Reviews recentes | Scraping cauteloso | Mensal | Violação de ToS |

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

### 3.3 Parcerias - Estratégia de Relacionamento
#### **Programa de Parceiros Estabelecimentos**
- **Tier Gratuito**: Listagem básica + analytics simples
- **Tier Premium**: Dashboard completo + reservas + promoções
- **Incentivos**: Primeiros 50 estabelecimentos gratuitos por 6 meses

#### **Influenciadores e Curadores**
- Seleção baseada em engajamento e relevância local
- Compensação: Visibilidade + experiências gratuitas
- KPIs: Qualidade das recomendações + engajamento gerado

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

---

**Responsável**: Lucas Emmanuel Gomes de Lucena (Modeling e Infraestrutura)  
**Colaboração**: Henrique Fontaine (Arquitetura Técnica)  
**Revisão**: Mensal com base em qualidade e performance  
**Última atualização**: Dezembro 2024
