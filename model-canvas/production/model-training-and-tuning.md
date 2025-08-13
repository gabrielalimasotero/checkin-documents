# Canvas de Treinamento e Ajuste de Modelos - CheckIn

## Visão Geral
Este canvas documenta a estratégia completa de treinamento, ajuste e melhoria contínua dos modelos de IA do CheckIn, incluindo dados, algoritmos, métricas de qualidade e pipelines de MLOps em produção.

---

## 1. Arquitetura de Machine Learning

### 1.1 ML Pipeline Overview
```
Data Collection → Feature Engineering → Model Training → Validation → Deployment → Monitoring
       ↓                 ↓                ↓             ↓           ↓            ↓
   User Events      Feature Store    Training Jobs    A/B Tests   Model API   Performance
   Venue Data       Transformations   Experiments     Validation  Serving     Monitoring
   External APIs    Data Quality      Hyperparams     Metrics     Inference   Drift Detection
```

### 1.2 Modelos em Produção

#### **Modelo 1: Recommendation Engine**
- **Objetivo**: Gerar sugestões personalizadas de locais
- **Tipo**: Hybrid (Collaborative Filtering + Content-Based + LLM)
- **Input**: User context, preferences, history, venue features
- **Output**: Ranked list of venue suggestions with explanations
- **Refresh**: Real-time inference + daily model updates

#### **Modelo 2: Context Understanding**
- **Objetivo**: Processar linguagem natural do usuário
- **Tipo**: Fine-tuned Large Language Model (OpenAI GPT-4)
- **Input**: User text input, conversation history
- **Output**: Structured context (mood, preferences, constraints)
- **Refresh**: Prompt engineering + monthly fine-tuning

#### **Modelo 3: Venue Matching**
- **Objetivo**: Score de compatibilidade venue-usuário
- **Tipo**: Gradient Boosting (XGBoost)
- **Input**: User features, venue features, contextual data
- **Output**: Compatibility score (0-1)
- **Refresh**: Weekly retraining

---

## 2. Estratégia de Dados

### 2.1 Data Collection Strategy

#### **Dados Primários (Coletados pelo App)**
```python
class DataCollectionService:
    def __init__(self):
        self.event_tracker = EventTracker()
        self.feature_store = FeatureStore()
        self.data_quality = DataQualityChecker()
    
    async def track_user_interaction(self, interaction: UserInteraction):
        """Track user interactions for model training"""
        
        # Core interaction data
        interaction_data = {
            'user_id': interaction.user_id,
            'session_id': interaction.session_id,
            'timestamp': interaction.timestamp,
            'interaction_type': interaction.type,  # view, click, accept, reject
            'venue_id': interaction.venue_id,
            'context': interaction.context,
            'response_time': interaction.response_time_ms
        }
        
        # Quality checks
        if await self.data_quality.is_valid_interaction(interaction_data):
            await self.event_tracker.record('user_interaction', interaction_data)
            
            # Real-time feature update
            await self.feature_store.update_user_features(
                interaction.user_id, 
                interaction_data
            )
    
    async def track_recommendation_feedback(self, feedback: RecommendationFeedback):
        """Track feedback on AI recommendations"""
        
        feedback_data = {
            'recommendation_id': feedback.recommendation_id,
            'user_id': feedback.user_id,
            'suggestions': feedback.original_suggestions,
            'user_action': feedback.action,  # accepted, rejected, ignored
            'acceptance_reason': feedback.reason,
            'satisfaction_score': feedback.satisfaction,
            'context_accuracy': feedback.context_accuracy
        }
        
        await self.event_tracker.record('recommendation_feedback', feedback_data)
```

#### **Dados de Treinamento**
| Tipo de Dado | Volume Semanal | Qualidade | Uso Principal |
|--------------|----------------|-----------|---------------|
| **User Interactions** | 50k events | Alta | Recommendation training |
| **Venue Check-ins** | 5k check-ins | Alta | Venue popularity scoring |
| **AI Conversations** | 20k conversations | Média | Prompt optimization |
| **Venue Updates** | 500 updates | Alta | Content-based features |
| **External API Data** | 1k API calls | Média | Venue enrichment |

### 2.2 Feature Engineering

#### **User Features**
```python
class UserFeatureEngineer:
    def extract_user_features(self, user_id: str, lookback_days: int = 30) -> UserFeatures:
        """Extract comprehensive user features for ML models"""
        
        # Behavioral features
        behavioral_features = {
            'avg_session_duration': self.get_avg_session_duration(user_id, lookback_days),
            'check_in_frequency': self.get_checkin_frequency(user_id, lookback_days),
            'venue_category_preferences': self.get_category_preferences(user_id),
            'time_of_day_patterns': self.get_temporal_patterns(user_id),
            'social_activity_level': self.get_social_activity(user_id),
            'exploration_vs_exploitation': self.get_exploration_ratio(user_id)
        }
        
        # Preference features
        preference_features = {
            'cuisine_preferences': self.get_cuisine_preferences(user_id),
            'price_sensitivity': self.calculate_price_sensitivity(user_id),
            'distance_tolerance': self.get_avg_travel_distance(user_id),
            'rating_threshold': self.get_min_rating_preference(user_id),
            'group_size_preference': self.get_typical_group_size(user_id)
        }
        
        # Contextual features
        contextual_features = {
            'recent_rejections': self.get_recent_rejections(user_id, 7),
            'current_mood_indicators': self.infer_current_mood(user_id),
            'seasonal_preferences': self.get_seasonal_patterns(user_id),
            'weather_sensitivity': self.get_weather_influence(user_id)
        }
        
        return UserFeatures(
            **behavioral_features,
            **preference_features,
            **contextual_features
        )
```

#### **Venue Features**
```python
class VenueFeatureEngineer:
    def extract_venue_features(self, venue_id: str) -> VenueFeatures:
        """Extract venue features for recommendation models"""
        
        # Static features
        static_features = {
            'category': self.get_venue_category(venue_id),
            'price_range': self.get_price_range(venue_id),
            'capacity': self.get_capacity(venue_id),
            'location_features': self.get_location_features(venue_id),
            'amenities': self.get_amenities_vector(venue_id)
        }
        
        # Dynamic features (updated regularly)
        dynamic_features = {
            'popularity_score': self.calculate_popularity_score(venue_id),
            'recent_rating_trend': self.get_rating_trend(venue_id, 30),
            'check_in_volume_trend': self.get_checkin_trend(venue_id, 30),
            'peak_hours': self.identify_peak_hours(venue_id),
            'typical_crowd_demographics': self.get_crowd_demographics(venue_id)
        }
        
        # Real-time features
        realtime_features = {
            'current_crowd_level': self.estimate_current_crowd(venue_id),
            'wait_time_estimate': self.estimate_wait_time(venue_id),
            'live_events': self.get_current_events(venue_id),
            'weather_impact': self.calculate_weather_impact(venue_id)
        }
        
        return VenueFeatures(
            **static_features,
            **dynamic_features,
            **realtime_features
        )
```

---

## 3. Modelo de Recomendação

### 3.1 Hybrid Recommendation System

#### **Arquitetura do Modelo**
```python
class HybridRecommendationModel:
    def __init__(self):
        # Sub-models
        self.collaborative_filter = CollaborativeFilteringModel()
        self.content_based = ContentBasedModel()
        self.popularity_model = PopularityModel()
        self.llm_explainer = LLMExplainerModel()
        
        # Ensemble weights (learned)
        self.model_weights = ModelWeights()
        
    async def predict(
        self, 
        user_features: UserFeatures,
        candidate_venues: List[VenueFeatures],
        context: RequestContext
    ) -> List[ScoredRecommendation]:
        """Generate hybrid recommendations"""
        
        # Get predictions from each sub-model
        cf_scores = await self.collaborative_filter.predict(
            user_features, candidate_venues
        )
        
        content_scores = await self.content_based.predict(
            user_features, candidate_venues
        )
        
        popularity_scores = await self.popularity_model.predict(
            candidate_venues, context
        )
        
        # Ensemble combination
        final_scores = self.combine_scores(
            cf_scores, content_scores, popularity_scores,
            user_features, context
        )
        
        # Generate explanations
        explanations = await self.llm_explainer.explain_recommendations(
            user_features, candidate_venues, final_scores, context
        )
        
        # Create final recommendations
        recommendations = []
        for venue, score, explanation in zip(candidate_venues, final_scores, explanations):
            recommendations.append(ScoredRecommendation(
                venue=venue,
                score=score,
                explanation=explanation,
                confidence=self.calculate_confidence(score, user_features),
                reason_codes=self.extract_reason_codes(venue, user_features)
            ))
        
        # Sort by score and return top N
        return sorted(recommendations, key=lambda x: x.score, reverse=True)[:10]
    
    def combine_scores(
        self,
        cf_scores: List[float],
        content_scores: List[float], 
        popularity_scores: List[float],
        user_features: UserFeatures,
        context: RequestContext
    ) -> List[float]:
        """Dynamically combine scores based on context"""
        
        # Get weights based on user and context
        weights = self.model_weights.get_dynamic_weights(user_features, context)
        
        final_scores = []
        for i in range(len(cf_scores)):
            combined_score = (
                weights.collaborative * cf_scores[i] +
                weights.content_based * content_scores[i] +
                weights.popularity * popularity_scores[i]
            )
            final_scores.append(combined_score)
        
        return final_scores
```

### 3.2 Collaborative Filtering Model

#### **Matrix Factorization with Neural Networks**
```python
class CollaborativeFilteringModel:
    def __init__(self):
        self.model = self._build_neural_cf_model()
        self.user_embeddings = None
        self.venue_embeddings = None
        
    def _build_neural_cf_model(self):
        """Build neural collaborative filtering model"""
        
        # User and venue input layers
        user_input = Input(shape=(), name='user_id')
        venue_input = Input(shape=(), name='venue_id')
        
        # Embedding layers
        user_embedding = Embedding(
            input_dim=self.num_users,
            output_dim=128,
            name='user_embedding'
        )(user_input)
        
        venue_embedding = Embedding(
            input_dim=self.num_venues,
            output_dim=128,
            name='venue_embedding'
        )(venue_input)
        
        # Flatten embeddings
        user_flat = Flatten()(user_embedding)
        venue_flat = Flatten()(venue_embedding)
        
        # Concatenate and add dense layers
        concat = Concatenate()([user_flat, venue_flat])
        dense1 = Dense(256, activation='relu')(concat)
        dropout1 = Dropout(0.3)(dense1)
        dense2 = Dense(128, activation='relu')(dropout1)
        dropout2 = Dropout(0.3)(dense2)
        
        # Output layer
        output = Dense(1, activation='sigmoid', name='rating')(dropout2)
        
        model = Model(inputs=[user_input, venue_input], outputs=output)
        model.compile(
            optimizer=Adam(learning_rate=0.001),
            loss='binary_crossentropy',
            metrics=['accuracy', 'precision', 'recall']
        )
        
        return model
    
    async def train(self, training_data: pd.DataFrame):
        """Train collaborative filtering model"""
        
        # Prepare training data
        user_ids = training_data['user_id'].values
        venue_ids = training_data['venue_id'].values
        ratings = (training_data['rating'] >= 4).astype(int)  # Binary: good/bad
        
        # Split train/validation
        train_size = int(0.8 * len(training_data))
        
        # Train model
        history = self.model.fit(
            [user_ids[:train_size], venue_ids[:train_size]],
            ratings[:train_size],
            validation_data=(
                [user_ids[train_size:], venue_ids[train_size:]],
                ratings[train_size:]
            ),
            epochs=50,
            batch_size=1024,
            callbacks=[
                EarlyStopping(patience=5, restore_best_weights=True),
                ReduceLROnPlateau(patience=3, factor=0.5)
            ]
        )
        
        # Extract embeddings for similarity calculations
        self.user_embeddings = self.model.get_layer('user_embedding').get_weights()[0]
        self.venue_embeddings = self.model.get_layer('venue_embedding').get_weights()[0]
        
        return history
```

---

## 4. Treinamento e Otimização

### 4.1 Training Pipeline

#### **Automated Training Workflow**
```python
class MLTrainingPipeline:
    def __init__(self):
        self.data_loader = DataLoader()
        self.feature_engineer = FeatureEngineer()
        self.model_trainer = ModelTrainer()
        self.validator = ModelValidator()
        self.deployment_manager = ModelDeploymentManager()
        
    async def run_training_pipeline(self, model_config: ModelConfig):
        """Run complete ML training pipeline"""
        
        try:
            # 1. Data Preparation
            logger.info("Starting data preparation...")
            training_data = await self.data_loader.load_training_data(
                start_date=model_config.training_start_date,
                end_date=model_config.training_end_date
            )
            
            # Data quality checks
            quality_report = await self.data_loader.validate_data_quality(training_data)
            if not quality_report.is_acceptable():
                raise DataQualityError(f"Data quality issues: {quality_report.issues}")
            
            # 2. Feature Engineering
            logger.info("Engineering features...")
            features = await self.feature_engineer.create_features(training_data)
            
            # 3. Model Training
            logger.info("Training model...")
            trained_model = await self.model_trainer.train(
                features=features,
                config=model_config
            )
            
            # 4. Model Validation
            logger.info("Validating model...")
            validation_results = await self.validator.validate_model(
                model=trained_model,
                test_data=features.test_set
            )
            
            # Check if model meets quality thresholds
            if not validation_results.meets_quality_threshold():
                logger.warning(f"Model doesn't meet quality threshold: {validation_results}")
                return TrainingResult(
                    success=False,
                    reason="Quality threshold not met",
                    validation_results=validation_results
                )
            
            # 5. A/B Test Preparation
            logger.info("Preparing for A/B test...")
            ab_test_config = await self.deployment_manager.prepare_ab_test(
                new_model=trained_model,
                current_model=self.get_current_production_model(),
                traffic_split=0.1  # 10% traffic to new model
            )
            
            # 6. Deploy to staging
            logger.info("Deploying to staging...")
            staging_deployment = await self.deployment_manager.deploy_to_staging(
                model=trained_model,
                ab_test_config=ab_test_config
            )
            
            return TrainingResult(
                success=True,
                model=trained_model,
                validation_results=validation_results,
                staging_deployment=staging_deployment
            )
            
        except Exception as e:
            logger.error(f"Training pipeline failed: {e}")
            await self.handle_training_failure(e)
            raise
```

### 4.2 Hyperparameter Optimization

#### **Automated Hyperparameter Tuning**
```python
class HyperparameterOptimizer:
    def __init__(self):
        self.optimization_method = 'bayesian'  # bayesian, grid, random
        self.max_trials = 100
        self.objective_metric = 'ndcg_at_10'
        
    async def optimize_hyperparameters(
        self, 
        model_class: Type[BaseModel],
        training_data: TrainingData
    ) -> OptimizationResult:
        """Optimize hyperparameters using Bayesian optimization"""
        
        def objective(trial):
            # Define hyperparameter search space
            params = {
                'learning_rate': trial.suggest_float('learning_rate', 1e-5, 1e-1, log=True),
                'batch_size': trial.suggest_categorical('batch_size', [128, 256, 512, 1024]),
                'embedding_dim': trial.suggest_int('embedding_dim', 32, 256),
                'num_layers': trial.suggest_int('num_layers', 1, 5),
                'dropout_rate': trial.suggest_float('dropout_rate', 0.1, 0.5),
                'regularization': trial.suggest_float('regularization', 1e-6, 1e-2, log=True)
            }
            
            # Train model with these parameters
            model = model_class(**params)
            model.fit(training_data.train_set)
            
            # Evaluate on validation set
            predictions = model.predict(training_data.validation_set)
            score = self.calculate_ndcg_at_k(
                training_data.validation_set.true_ratings,
                predictions,
                k=10
            )
            
            return score
        
        # Run optimization
        study = optuna.create_study(direction='maximize')
        study.optimize(objective, n_trials=self.max_trials)
        
        return OptimizationResult(
            best_params=study.best_params,
            best_score=study.best_value,
            optimization_history=study.trials_dataframe()
        )
```

---

## 5. Avaliação e Métricas

### 5.1 Offline Metrics

#### **Recommendation Quality Metrics**
```python
class ModelEvaluator:
    def __init__(self):
        self.metrics = [
            'precision_at_k',
            'recall_at_k', 
            'ndcg_at_k',
            'map_at_k',
            'coverage',
            'diversity',
            'novelty'
        ]
    
    async def evaluate_model(
        self, 
        model: RecommendationModel,
        test_data: TestData
    ) -> EvaluationResults:
        """Comprehensive model evaluation"""
        
        results = {}
        
        for k in [1, 3, 5, 10]:
            # Get predictions
            predictions = await model.predict_batch(test_data.user_contexts)
            
            # Calculate metrics
            results[f'precision_at_{k}'] = self.precision_at_k(
                test_data.true_preferences, predictions, k
            )
            
            results[f'recall_at_{k}'] = self.recall_at_k(
                test_data.true_preferences, predictions, k
            )
            
            results[f'ndcg_at_{k}'] = self.ndcg_at_k(
                test_data.true_preferences, predictions, k
            )
        
        # Coverage metrics
        results['catalog_coverage'] = self.calculate_catalog_coverage(predictions)
        results['user_coverage'] = self.calculate_user_coverage(predictions)
        
        # Diversity metrics
        results['intra_list_diversity'] = self.calculate_diversity(predictions)
        results['novelty'] = self.calculate_novelty(predictions, test_data.popularity)
        
        # Business metrics
        results['conversion_prediction'] = self.predict_conversion_rate(
            predictions, test_data.historical_conversions
        )
        
        return EvaluationResults(
            metrics=results,
            detailed_breakdown=self.create_detailed_breakdown(
                test_data, predictions, results
            )
        )
    
    def ndcg_at_k(
        self, 
        true_preferences: List[List[int]], 
        predictions: List[List[int]], 
        k: int
    ) -> float:
        """Calculate Normalized Discounted Cumulative Gain at K"""
        
        ndcg_scores = []
        
        for true_prefs, pred_list in zip(true_preferences, predictions):
            # Calculate DCG
            dcg = 0.0
            for i, item_id in enumerate(pred_list[:k]):
                if item_id in true_prefs:
                    relevance = 1.0  # Binary relevance for simplicity
                    dcg += relevance / math.log2(i + 2)
            
            # Calculate IDCG (ideal DCG)
            ideal_order = sorted(true_prefs, reverse=True)[:k]
            idcg = 0.0
            for i, _ in enumerate(ideal_order):
                idcg += 1.0 / math.log2(i + 2)
            
            # Calculate NDCG
            if idcg > 0:
                ndcg_scores.append(dcg / idcg)
            else:
                ndcg_scores.append(0.0)
        
        return np.mean(ndcg_scores)
```

### 5.2 Online A/B Testing

#### **Real-time A/B Testing Framework**
```python
class ABTestFramework:
    def __init__(self):
        self.test_manager = ABTestManager()
        self.metrics_collector = MetricsCollector()
        self.statistical_analyzer = StatisticalAnalyzer()
        
    async def run_model_ab_test(
        self,
        control_model: RecommendationModel,
        treatment_model: RecommendationModel,
        test_config: ABTestConfig
    ) -> ABTestResult:
        """Run A/B test comparing two recommendation models"""
        
        # Start A/B test
        test_id = await self.test_manager.start_test(
            name=f"recommendation_model_{treatment_model.version}",
            control_variant=ModelVariant(model=control_model, traffic_percent=50),
            treatment_variant=ModelVariant(model=treatment_model, traffic_percent=50),
            duration_days=test_config.duration_days,
            success_metrics=test_config.success_metrics
        )
        
        # Collect metrics during test period
        metrics_data = await self.metrics_collector.collect_test_metrics(
            test_id=test_id,
            metrics=[
                'click_through_rate',
                'conversion_rate', 
                'user_satisfaction',
                'recommendation_acceptance_rate',
                'session_length',
                'revenue_per_user'
            ]
        )
        
        # Statistical analysis
        statistical_results = await self.statistical_analyzer.analyze_test_results(
            control_data=metrics_data.control,
            treatment_data=metrics_data.treatment,
            confidence_level=0.95,
            minimum_detectable_effect=0.05
        )
        
        # Make deployment decision
        deployment_decision = self.make_deployment_decision(
            statistical_results,
            test_config.deployment_criteria
        )
        
        return ABTestResult(
            test_id=test_id,
            statistical_results=statistical_results,
            deployment_decision=deployment_decision,
            detailed_metrics=metrics_data
        )
    
    def make_deployment_decision(
        self,
        statistical_results: StatisticalResults,
        criteria: DeploymentCriteria
    ) -> DeploymentDecision:
        """Make data-driven deployment decision"""
        
        # Check statistical significance
        if not statistical_results.is_statistically_significant():
            return DeploymentDecision(
                decision='no_change',
                reason='No statistically significant difference detected'
            )
        
        # Check business impact
        primary_metric_improvement = statistical_results.get_metric_improvement(
            criteria.primary_metric
        )
        
        if primary_metric_improvement >= criteria.minimum_improvement:
            # Check for negative impacts on secondary metrics
            secondary_metric_issues = []
            for metric in criteria.secondary_metrics:
                improvement = statistical_results.get_metric_improvement(metric)
                if improvement < criteria.secondary_metric_threshold:
                    secondary_metric_issues.append(metric)
            
            if not secondary_metric_issues:
                return DeploymentDecision(
                    decision='deploy_treatment',
                    reason=f'Significant improvement in {criteria.primary_metric}: {primary_metric_improvement:.2%}'
                )
            else:
                return DeploymentDecision(
                    decision='conditional_deploy',
                    reason=f'Good primary metric improvement but issues with: {secondary_metric_issues}'
                )
        
        return DeploymentDecision(
            decision='no_change',
            reason=f'Improvement ({primary_metric_improvement:.2%}) below threshold ({criteria.minimum_improvement:.2%})'
        )
```

---

## 6. Model Deployment e MLOps

### 6.1 Model Serving Pipeline

#### **Production Model Serving**
```python
class ModelServingPipeline:
    def __init__(self):
        self.model_registry = ModelRegistry()
        self.feature_store = FeatureStore()
        self.prediction_cache = PredictionCache()
        self.monitoring = ModelMonitoring()
        
    async def serve_recommendations(
        self,
        user_context: UserContext,
        request_id: str
    ) -> RecommendationResponse:
        """Serve recommendations with full production pipeline"""
        
        start_time = time.time()
        
        try:
            # 1. Get active model
            active_model = await self.model_registry.get_active_model('recommendation')
            
            # 2. Check prediction cache
            cache_key = self.build_cache_key(user_context)
            cached_prediction = await self.prediction_cache.get(cache_key)
            
            if cached_prediction and not self.is_cache_stale(cached_prediction):
                await self.monitoring.track_cache_hit(request_id)
                return cached_prediction
            
            # 3. Get real-time features
            user_features = await self.feature_store.get_user_features(user_context.user_id)
            venue_features = await self.feature_store.get_venue_features(
                user_context.location, user_context.max_distance
            )
            
            # 4. Make prediction
            prediction = await active_model.predict(
                user_features=user_features,
                venue_features=venue_features,
                context=user_context
            )
            
            # 5. Post-process prediction
            processed_prediction = await self.post_process_prediction(
                prediction, user_context
            )
            
            # 6. Cache prediction
            await self.prediction_cache.set(
                cache_key, 
                processed_prediction, 
                ttl=300  # 5 minutes
            )
            
            # 7. Track metrics
            await self.monitoring.track_prediction(
                model_version=active_model.version,
                request_id=request_id,
                user_context=user_context,
                prediction=processed_prediction,
                latency_ms=int((time.time() - start_time) * 1000)
            )
            
            return processed_prediction
            
        except Exception as e:
            # Fallback to rule-based recommendations
            await self.monitoring.track_model_error(request_id, str(e))
            return await self.get_fallback_recommendations(user_context)
```

### 6.2 Model Monitoring

#### **Model Performance Monitoring**
```python
class ModelMonitoring:
    def __init__(self):
        self.metrics_client = PrometheusClient()
        self.alerting = AlertingService()
        self.drift_detector = DataDriftDetector()
        
    async def monitor_model_performance(self, model_version: str):
        """Continuous monitoring of model performance in production"""
        
        # Real-time metrics
        prediction_latency = await self.get_prediction_latency(model_version)
        error_rate = await self.get_error_rate(model_version)
        throughput = await self.get_throughput(model_version)
        
        # Model quality metrics
        recent_ctr = await self.get_recent_ctr(model_version, hours=24)
        conversion_rate = await self.get_conversion_rate(model_version, hours=24)
        user_satisfaction = await self.get_user_satisfaction(model_version, hours=24)
        
        # Data drift detection
        feature_drift = await self.drift_detector.detect_feature_drift(
            model_version, hours=24
        )
        
        prediction_drift = await self.drift_detector.detect_prediction_drift(
            model_version, hours=24
        )
        
        # Check thresholds and alert if needed
        alerts = []
        
        if prediction_latency > 2000:  # 2 seconds
            alerts.append(Alert(
                severity='high',
                message=f'High prediction latency: {prediction_latency}ms'
            ))
        
        if error_rate > 0.05:  # 5%
            alerts.append(Alert(
                severity='critical',
                message=f'High error rate: {error_rate:.2%}'
            ))
        
        if recent_ctr < 0.1:  # 10% CTR threshold
            alerts.append(Alert(
                severity='medium',
                message=f'Low click-through rate: {recent_ctr:.2%}'
            ))
        
        if feature_drift.is_significant():
            alerts.append(Alert(
                severity='medium',
                message=f'Feature drift detected: {feature_drift.affected_features}'
            ))
        
        # Send alerts
        for alert in alerts:
            await self.alerting.send_alert(alert)
        
        # Log monitoring metrics
        await self.metrics_client.gauge('model.prediction_latency', prediction_latency)
        await self.metrics_client.gauge('model.error_rate', error_rate)
        await self.metrics_client.gauge('model.ctr', recent_ctr)
        await self.metrics_client.gauge('model.conversion_rate', conversion_rate)
```

---

## 7. Continuous Learning

### 7.1 Online Learning

#### **Incremental Model Updates**
```python
class OnlineLearningSystem:
    def __init__(self):
        self.model = self.load_base_model()
        self.learning_rate = 0.001
        self.batch_size = 1000
        self.update_frequency = 3600  # 1 hour
        
    async def continuous_learning_loop(self):
        """Continuously update model with new data"""
        
        while True:
            try:
                # Collect recent training data
                new_data = await self.collect_recent_training_data()
                
                if len(new_data) >= self.batch_size:
                    # Incremental training
                    await self.incremental_update(new_data)
                    
                    # Validate updated model
                    validation_score = await self.quick_validation()
                    
                    if validation_score > self.current_model_score:
                        # Deploy updated model
                        await self.deploy_updated_model()
                        logger.info(f"Model updated with score: {validation_score}")
                    else:
                        # Revert to previous model
                        await self.revert_to_previous_model()
                        logger.warning("Model update reverted due to poor performance")
                
                # Wait before next update
                await asyncio.sleep(self.update_frequency)
                
            except Exception as e:
                logger.error(f"Online learning error: {e}")
                await asyncio.sleep(300)  # 5 minute backoff
    
    async def incremental_update(self, new_data: TrainingData):
        """Perform incremental model update"""
        
        # Prepare data
        features, labels = self.prepare_training_data(new_data)
        
        # Partial fit (for models that support it)
        if hasattr(self.model, 'partial_fit'):
            self.model.partial_fit(features, labels)
        else:
            # For neural networks, use warm start
            self.model.fit(
                features, labels,
                epochs=1,
                batch_size=self.batch_size,
                initial_epoch=self.model.current_epoch
            )
        
        # Update model metadata
        self.model.last_updated = datetime.utcnow()
        self.model.training_samples += len(new_data)
```

### 7.2 Active Learning

#### **Smart Data Collection**
```python
class ActiveLearningSystem:
    def __init__(self):
        self.uncertainty_sampler = UncertaintySampler()
        self.diversity_sampler = DiversitySampler()
        self.labeling_budget = 1000  # labels per week
        
    async def select_samples_for_labeling(
        self, 
        unlabeled_pool: List[UserVenueInteraction]
    ) -> List[UserVenueInteraction]:
        """Select most informative samples for human labeling"""
        
        # Get model uncertainty for each sample
        uncertainties = await self.calculate_prediction_uncertainties(unlabeled_pool)
        
        # Select high-uncertainty samples
        uncertainty_samples = self.uncertainty_sampler.sample(
            unlabeled_pool, 
            uncertainties,
            n_samples=int(self.labeling_budget * 0.7)  # 70% uncertainty sampling
        )
        
        # Select diverse samples to cover edge cases
        diversity_samples = self.diversity_sampler.sample(
            unlabeled_pool,
            n_samples=int(self.labeling_budget * 0.3)  # 30% diversity sampling
        )
        
        selected_samples = uncertainty_samples + diversity_samples
        
        # Send for human labeling
        await self.send_for_labeling(selected_samples)
        
        return selected_samples
    
    async def calculate_prediction_uncertainties(
        self, 
        samples: List[UserVenueInteraction]
    ) -> List[float]:
        """Calculate model uncertainty for each sample"""
        
        uncertainties = []
        
        for sample in samples:
            # Get model predictions with multiple forward passes (Monte Carlo Dropout)
            predictions = []
            for _ in range(10):  # 10 forward passes
                pred = await self.model.predict_with_dropout(sample)
                predictions.append(pred)
            
            # Calculate uncertainty as variance of predictions
            uncertainty = np.var(predictions)
            uncertainties.append(uncertainty)
        
        return uncertainties
```

---
