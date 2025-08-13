# Canvas de Feedback e Iteração - CheckIn

## Visão Geral
Este canvas estabelece um sistema abrangente de coleta, análise e implementação de feedback para garantir melhoria contínua do CheckIn baseada em dados reais de usuários, estabelecimentos e stakeholders.

---

## 1. Estratégia de Feedback Multi-Canal

### 1.1 Canais de Feedback Identificados

#### **Feedback de Usuários Finais**
```
Canais Passivos:
├── In-app ratings (pós check-in)
├── App store reviews (iOS/Android)
├── Social media mentions
├── Customer support tickets
└── Analytics comportamentais

Canais Ativos:
├── Surveys in-app (contextuais)
├── Focus groups (trimestrais)
├── User interviews (semanais)
├── Beta tester community
└── NPS surveys (mensais)
```

#### **Feedback de Estabelecimentos**
```
Canais B2B:
├── Dashboard feedback widget
├── Account manager check-ins
├── Partner satisfaction surveys
├── Usage analytics review
└── Revenue impact analysis
```

#### **Feedback Interno**
```
Canais Internos:
├── Team retrospectives
├── Bug reports e monitoring
├── Performance metrics
├── Customer success insights
└── Sales team feedback
```

### 1.2 Framework de Coleta Contextual

#### **Triggered Feedback System**
```python
class ContextualFeedbackTrigger:
    def __init__(self):
        self.feedback_triggers = {
            'post_recommendation_acceptance': {
                'timing': 'immediately_after_action',
                'question': 'How helpful was this suggestion?',
                'type': 'quick_rating',
                'frequency': 'every_time'
            },
            'post_checkin_completion': {
                'timing': '15_minutes_after_checkin',
                'question': 'How was your experience at {venue_name}?',
                'type': 'detailed_review',
                'frequency': 'every_checkin'
            },
            'recommendation_rejection': {
                'timing': 'immediately_after_rejection',
                'question': 'Why wasn\'t this suggestion right for you?',
                'type': 'multiple_choice',
                'frequency': 'every_3rd_rejection'
            },
            'session_completion': {
                'timing': 'end_of_session',
                'question': 'Did CheckIn help you decide where to go today?',
                'type': 'yes_no_with_comment',
                'frequency': 'weekly_max'
            }
        }
    
    async def trigger_feedback_request(
        self, 
        user_id: str, 
        trigger_event: str,
        context: dict
    ):
        """Trigger contextual feedback based on user actions"""
        
        if not await self.should_request_feedback(user_id, trigger_event):
            return
        
        trigger_config = self.feedback_triggers[trigger_event]
        
        feedback_request = FeedbackRequest(
            user_id=user_id,
            trigger_event=trigger_event,
            context=context,
            question=trigger_config['question'].format(**context),
            type=trigger_config['type'],
            timestamp=datetime.utcnow()
        )
        
        await self.send_feedback_request(feedback_request)
        await self.track_feedback_request(feedback_request)
    
    async def should_request_feedback(
        self, 
        user_id: str, 
        trigger_event: str
    ) -> bool:
        """Determine if we should request feedback to avoid survey fatigue"""
        
        # Check frequency limits
        recent_requests = await self.get_recent_feedback_requests(
            user_id, 
            hours=24
        )
        
        if len(recent_requests) >= 3:  # Max 3 feedback requests per day
            return False
        
        # Check user preference
        user_feedback_settings = await self.get_user_feedback_preferences(user_id)
        if not user_feedback_settings.allow_feedback_requests:
            return False
        
        # Check event-specific frequency
        trigger_config = self.feedback_triggers[trigger_event]
        if trigger_config['frequency'] != 'every_time':
            # Implement frequency logic
            pass
        
        return True
```

---

## 2. Análise e Processamento de Feedback

### 2.1 Sistema de Categorização Automática

#### **NLP-based Feedback Analysis**
```python
class FeedbackAnalysisEngine:
    def __init__(self):
        self.sentiment_analyzer = SentimentAnalyzer()
        self.topic_extractor = TopicExtractor()
        self.feedback_classifier = FeedbackClassifier()
        
    async def analyze_feedback(self, feedback: Feedback) -> FeedbackAnalysis:
        """Comprehensive analysis of user feedback"""
        
        # Sentiment analysis
        sentiment = await self.sentiment_analyzer.analyze(feedback.text)
        
        # Topic extraction
        topics = await self.topic_extractor.extract(feedback.text)
        
        # Classification by category
        categories = await self.feedback_classifier.classify(feedback.text)
        
        # Priority scoring
        priority = self.calculate_priority_score(
            sentiment, topics, categories, feedback.user_context
        )
        
        # Extract actionable insights
        insights = await self.extract_actionable_insights(
            feedback.text, categories, sentiment
        )
        
        return FeedbackAnalysis(
            feedback_id=feedback.id,
            sentiment=sentiment,
            topics=topics,
            categories=categories,
            priority_score=priority,
            actionable_insights=insights,
            requires_immediate_action=priority > 0.8
        )
    
    def calculate_priority_score(
        self, 
        sentiment: SentimentScore,
        topics: List[Topic],
        categories: List[Category],
        user_context: UserContext
    ) -> float:
        """Calculate priority score for feedback"""
        
        base_score = 0.5
        
        # Sentiment weight
        if sentiment.negative > 0.7:
            base_score += 0.3
        elif sentiment.positive > 0.8:
            base_score += 0.1
        
        # Critical topic weight
        critical_topics = ['bug', 'crash', 'error', 'broken', 'wrong_recommendation']
        if any(topic.name in critical_topics for topic in topics):
            base_score += 0.3
        
        # User importance weight
        if user_context.is_power_user:
            base_score += 0.1
        if user_context.is_paying_customer:
            base_score += 0.15
        
        # Category weight
        high_priority_categories = ['technical_issue', 'core_feature', 'ai_accuracy']
        if any(cat.name in high_priority_categories for cat in categories):
            base_score += 0.2
        
        return min(base_score, 1.0)
```

### 2.2 Dashboard de Insights

#### **Real-time Feedback Dashboard**
```python
class FeedbackDashboard:
    def __init__(self):
        self.metrics_calculator = FeedbackMetricsCalculator()
        self.trend_analyzer = TrendAnalyzer()
        
    async def generate_feedback_insights(
        self, 
        time_period: TimePeriod = TimePeriod.LAST_30_DAYS
    ) -> FeedbackInsights:
        """Generate comprehensive feedback insights"""
        
        # Aggregate metrics
        metrics = await self.metrics_calculator.calculate_metrics(time_period)
        
        # Trend analysis
        trends = await self.trend_analyzer.analyze_trends(time_period)
        
        # Top issues identification
        top_issues = await self.identify_top_issues(time_period)
        
        # User satisfaction trends
        satisfaction_trends = await self.analyze_satisfaction_trends(time_period)
        
        # Feature-specific feedback
        feature_feedback = await self.analyze_feature_feedback(time_period)
        
        return FeedbackInsights(
            period=time_period,
            overall_metrics=metrics,
            trends=trends,
            top_issues=top_issues,
            satisfaction_trends=satisfaction_trends,
            feature_feedback=feature_feedback,
            recommendations=self.generate_recommendations(
                metrics, trends, top_issues
            )
        )
    
    async def identify_top_issues(self, time_period: TimePeriod) -> List[Issue]:
        """Identify most critical issues from feedback"""
        
        all_feedback = await self.get_feedback(time_period)
        
        # Group by issue type
        issue_groups = defaultdict(list)
        for feedback in all_feedback:
            for category in feedback.analysis.categories:
                if category.confidence > 0.7:
                    issue_groups[category.name].append(feedback)
        
        # Calculate impact score for each issue
        issues = []
        for issue_type, feedbacks in issue_groups.items():
            impact_score = self.calculate_issue_impact(feedbacks)
            
            issues.append(Issue(
                type=issue_type,
                frequency=len(feedbacks),
                impact_score=impact_score,
                affected_users=len(set(f.user_id for f in feedbacks)),
                sample_feedback=feedbacks[:3],
                trend=self.calculate_issue_trend(issue_type, time_period)
            ))
        
        # Sort by impact and return top 10
        return sorted(issues, key=lambda x: x.impact_score, reverse=True)[:10]
```

---

## 3. Sistema de Priorização e Roadmap

### 3.1 Framework de Priorização de Feedback

#### **Impact vs Effort Matrix**
```python
class FeedbackPrioritization:
    def __init__(self):
        self.impact_calculator = ImpactCalculator()
        self.effort_estimator = EffortEstimator()
        
    async def prioritize_feedback_items(
        self, 
        feedback_items: List[FeedbackItem]
    ) -> List[PrioritizedItem]:
        """Prioritize feedback items using multiple criteria"""
        
        prioritized_items = []
        
        for item in feedback_items:
            # Calculate impact score
            impact = await self.impact_calculator.calculate_impact(item)
            
            # Estimate implementation effort
            effort = await self.effort_estimator.estimate_effort(item)
            
            # Calculate priority score
            priority_score = self.calculate_priority_score(impact, effort, item)
            
            prioritized_items.append(PrioritizedItem(
                feedback_item=item,
                impact_score=impact,
                effort_estimate=effort,
                priority_score=priority_score,
                recommended_action=self.get_recommended_action(
                    impact, effort, priority_score
                )
            ))
        
        # Sort by priority score
        return sorted(
            prioritized_items, 
            key=lambda x: x.priority_score, 
            reverse=True
        )
    
    def calculate_priority_score(
        self, 
        impact: ImpactScore, 
        effort: EffortEstimate,
        item: FeedbackItem
    ) -> float:
        """Calculate overall priority score"""
        
        # Base calculation: Impact / Effort
        base_score = impact.total_score / effort.complexity_score
        
        # Frequency multiplier
        frequency_multiplier = min(item.frequency / 100, 2.0)
        
        # User segment multiplier
        segment_multiplier = 1.0
        if item.affects_power_users:
            segment_multiplier += 0.3
        if item.affects_paying_customers:
            segment_multiplier += 0.5
        
        # Strategic alignment multiplier
        strategic_multiplier = 1.0
        if item.aligns_with_okrs:
            strategic_multiplier += 0.4
        if item.competitive_advantage:
            strategic_multiplier += 0.3
        
        return base_score * frequency_multiplier * segment_multiplier * strategic_multiplier
```

### 3.2 Feedback-Driven Roadmap Integration

#### **Automatic Roadmap Updates**
```python
class FeedbackRoadmapIntegration:
    def __init__(self):
        self.roadmap_manager = RoadmapManager()
        self.feedback_analyzer = FeedbackAnalyzer()
        
    async def update_roadmap_from_feedback(
        self, 
        feedback_period: TimePeriod = TimePeriod.WEEKLY
    ):
        """Update product roadmap based on feedback insights"""
        
        # Get latest feedback insights
        insights = await self.feedback_analyzer.get_insights(feedback_period)
        
        # Identify roadmap impacts
        roadmap_impacts = await self.identify_roadmap_impacts(insights)
        
        for impact in roadmap_impacts:
            if impact.action_type == 'new_feature':
                await self.roadmap_manager.add_feature_request(
                    feature=impact.feature_description,
                    priority=impact.priority,
                    supporting_feedback=impact.supporting_feedback,
                    effort_estimate=impact.effort_estimate
                )
            
            elif impact.action_type == 'reprioritize':
                await self.roadmap_manager.update_feature_priority(
                    feature_id=impact.feature_id,
                    new_priority=impact.new_priority,
                    reason=impact.justification
                )
            
            elif impact.action_type == 'accelerate':
                await self.roadmap_manager.accelerate_feature(
                    feature_id=impact.feature_id,
                    justification=impact.justification
                )
        
        # Generate roadmap update summary
        summary = await self.generate_update_summary(roadmap_impacts)
        await self.notify_stakeholders(summary)
```

---

## 4. Loop de Feedback Fechado

### 4.1 Comunicação de Volta aos Usuários

#### **"You Asked, We Delivered" System**
```python
class FeedbackClosureSystem:
    def __init__(self):
        self.user_tracker = UserFeedbackTracker()
        self.communication_manager = CommunicationManager()
        
    async def close_feedback_loop(
        self, 
        feature_release: FeatureRelease,
        related_feedback: List[Feedback]
    ):
        """Notify users when their feedback is implemented"""
        
        # Identify users who provided related feedback
        feedback_providers = set(f.user_id for f in related_feedback)
        
        # Create personalized notifications
        for user_id in feedback_providers:
            user_feedback = [f for f in related_feedback if f.user_id == user_id]
            
            notification = await self.create_feedback_closure_notification(
                user_id=user_id,
                feature_release=feature_release,
                user_feedback=user_feedback
            )
            
            await self.communication_manager.send_notification(notification)
        
        # Create public communication
        public_update = await self.create_public_feedback_update(
            feature_release=feature_release,
            feedback_stats=self.calculate_feedback_stats(related_feedback)
        )
        
        await self.communication_manager.publish_update(public_update)
    
    async def create_feedback_closure_notification(
        self,
        user_id: str,
        feature_release: FeatureRelease,
        user_feedback: List[Feedback]
    ) -> PersonalizedNotification:
        """Create personalized feedback closure notification"""
        
        # Extract user's specific requests
        user_requests = [f.content for f in user_feedback]
        
        # Generate personalized message
        message = f"""
        Great news! We've released an update that addresses your feedback.
        
        You asked for: {self.summarize_user_requests(user_requests)}
        
        We delivered: {feature_release.description}
        
        Your feedback on {user_feedback[0].created_at.strftime('%B %d')} 
        helped us prioritize this improvement. Thank you for helping make 
        CheckIn better!
        """
        
        return PersonalizedNotification(
            user_id=user_id,
            type='feedback_closure',
            title='Your feedback made this happen! 🎉',
            message=message,
            cta_text='Try the new feature',
            cta_action=feature_release.deep_link
        )
```

### 4.2 Métricas de Efetividade do Feedback

#### **Feedback Impact Tracking**
```python
class FeedbackImpactTracker:
    def __init__(self):
        self.metrics_collector = MetricsCollector()
        
    async def track_feedback_impact(self) -> FeedbackImpactReport:
        """Track the impact of feedback on product improvements"""
        
        # Implementation rate
        total_feedback_items = await self.count_feedback_items()
        implemented_items = await self.count_implemented_items()
        implementation_rate = implemented_items / total_feedback_items
        
        # Time to implementation
        avg_implementation_time = await self.calculate_avg_implementation_time()
        
        # User satisfaction post-implementation
        satisfaction_improvement = await self.measure_satisfaction_improvement()
        
        # Feature adoption from feedback
        feedback_driven_adoption = await self.measure_feedback_driven_adoption()
        
        return FeedbackImpactReport(
            implementation_rate=implementation_rate,
            avg_implementation_time=avg_implementation_time,
            satisfaction_improvement=satisfaction_improvement,
            adoption_rate=feedback_driven_adoption,
            top_feedback_sources=await self.identify_top_feedback_sources(),
            roi_of_feedback_program=await self.calculate_feedback_roi()
        )
    
    async def calculate_feedback_roi(self) -> float:
        """Calculate ROI of feedback program"""
        
        # Costs
        feedback_program_costs = await self.get_feedback_program_costs()
        
        # Benefits
        retention_improvement = await self.measure_retention_improvement()
        feature_success_rate = await self.measure_feature_success_rate()
        development_efficiency = await self.measure_development_efficiency()
        
        total_benefits = (
            retention_improvement.value +
            feature_success_rate.value +
            development_efficiency.value
        )
        
        return (total_benefits - feedback_program_costs) / feedback_program_costs
```

---

## 5. Feedback Específico por Stakeholder

### 5.1 Feedback de Estabelecimentos

#### **Partner Satisfaction System**
```python
class PartnerFeedbackSystem:
    def __init__(self):
        self.partner_manager = PartnerManager()
        self.analytics_engine = PartnerAnalyticsEngine()
        
    async def collect_partner_feedback(
        self, 
        partner_id: str,
        trigger_event: str
    ):
        """Collect feedback from venue partners"""
        
        partner = await self.partner_manager.get_partner(partner_id)
        
        feedback_triggers = {
            'monthly_review': {
                'questions': [
                    'How satisfied are you with the customer quality from CheckIn?',
                    'How would you rate the dashboard analytics?',
                    'What features would help you most?'
                ],
                'method': 'email_survey'
            },
            'feature_usage': {
                'questions': [
                    'How useful is the new analytics feature?',
                    'Any suggestions for improvement?'
                ],
                'method': 'in_app_popup'
            },
            'performance_change': {
                'questions': [
                    'We noticed a change in your metrics. How can we help?',
                    'Are you facing any challenges with CheckIn customers?'
                ],
                'method': 'account_manager_call'
            }
        }
        
        trigger_config = feedback_triggers[trigger_event]
        
        feedback_request = PartnerFeedbackRequest(
            partner_id=partner_id,
            trigger_event=trigger_event,
            questions=trigger_config['questions'],
            method=trigger_config['method'],
            context=await self.get_partner_context(partner_id)
        )
        
        await self.send_partner_feedback_request(feedback_request)
```

### 5.2 Feedback da Equipe Interna

#### **Team Feedback Integration**
```python
class InternalFeedbackSystem:
    def __init__(self):
        self.team_manager = TeamManager()
        
    async def collect_team_feedback(self):
        """Collect feedback from internal team members"""
        
        feedback_areas = {
            'customer_success': {
                'team': 'customer_success',
                'questions': [
                    'What are the most common user complaints this week?',
                    'Which features are users asking for most?',
                    'Any patterns in support tickets?'
                ]
            },
            'sales': {
                'team': 'sales',
                'questions': [
                    'What objections do prospects raise most often?',
                    'Which features would help close more deals?',
                    'What do competitors offer that we don\'t?'
                ]
            },
            'engineering': {
                'team': 'engineering', 
                'questions': [
                    'Which parts of the codebase are most problematic?',
                    'What technical debt should we prioritize?',
                    'Which features are hardest to maintain?'
                ]
            }
        }
        
        for area, config in feedback_areas.items():
            weekly_feedback = await self.collect_weekly_team_input(
                team=config['team'],
                questions=config['questions']
            )
            
            await self.process_team_feedback(area, weekly_feedback)
```

---

## 6. Feedback Automático e Proativo

### 6.1 Behavioral Feedback Collection

#### **Implicit Feedback from User Behavior**
```python
class BehavioralFeedbackCollector:
    def __init__(self):
        self.analytics = AnalyticsEngine()
        self.behavior_analyzer = BehaviorAnalyzer()
        
    async def collect_behavioral_feedback(self):
        """Extract feedback from user behavior patterns"""
        
        # Feature abandonment analysis
        abandoned_features = await self.analyze_feature_abandonment()
        
        # User flow drop-offs
        flow_dropoffs = await self.analyze_flow_dropoffs()
        
        # Recommendation acceptance patterns
        recommendation_patterns = await self.analyze_recommendation_patterns()
        
        # Session length trends
        session_trends = await self.analyze_session_trends()
        
        # Convert behaviors to feedback insights
        behavioral_insights = []
        
        for feature in abandoned_features:
            if feature.abandonment_rate > 0.5:
                behavioral_insights.append(BehavioralInsight(
                    type='feature_problem',
                    feature=feature.name,
                    insight=f'High abandonment rate ({feature.abandonment_rate:.1%})',
                    severity='high',
                    suggested_action='investigate_user_experience'
                ))
        
        for dropoff in flow_dropoffs:
            if dropoff.rate > 0.3:
                behavioral_insights.append(BehavioralInsight(
                    type='flow_friction',
                    flow=dropoff.flow_name,
                    insight=f'Users dropping off at {dropoff.step}',
                    severity='medium',
                    suggested_action='optimize_flow_step'
                ))
        
        return behavioral_insights
```

### 6.2 Predictive Feedback Analysis

#### **Sentiment Prediction and Early Warning**
```python
class PredictiveFeedbackAnalysis:
    def __init__(self):
        self.ml_model = SentimentPredictionModel()
        self.trend_analyzer = TrendAnalyzer()
        
    async def predict_user_satisfaction_trends(self) -> SatisfactionPrediction:
        """Predict future satisfaction trends based on current feedback"""
        
        # Get recent feedback data
        recent_feedback = await self.get_recent_feedback(days=30)
        
        # Extract features for prediction
        features = await self.extract_prediction_features(recent_feedback)
        
        # Predict satisfaction trend
        predicted_trend = await self.ml_model.predict_trend(features)
        
        # Identify risk factors
        risk_factors = await self.identify_risk_factors(recent_feedback)
        
        # Generate alerts if needed
        alerts = []
        if predicted_trend.direction == 'declining':
            alerts.append(SatisfactionAlert(
                severity='high' if predicted_trend.confidence > 0.8 else 'medium',
                message=f'User satisfaction predicted to decline by {predicted_trend.magnitude:.1%}',
                risk_factors=risk_factors,
                recommended_actions=await self.get_mitigation_actions(risk_factors)
            ))
        
        return SatisfactionPrediction(
            trend=predicted_trend,
            confidence=predicted_trend.confidence,
            risk_factors=risk_factors,
            alerts=alerts,
            recommended_actions=await self.get_proactive_actions(predicted_trend)
        )
```

---

## 7. Implementação e Governança

### 7.1 Processo de Review de Feedback

#### **Weekly Feedback Review Process**
```yaml
Monday - Feedback Collection Review:
  - Review weekly feedback volume and quality
  - Identify urgent issues requiring immediate attention
  - Update feedback categorization and tagging

Tuesday - Analysis and Insights:
  - Run automated analysis on new feedback
  - Generate insights and trend reports
  - Identify patterns and emerging themes

Wednesday - Prioritization Session:
  - Product team reviews prioritized feedback items
  - Align feedback insights with current roadmap
  - Make go/no-go decisions on feedback-driven features

Thursday - Planning and Assignment:
  - Assign ownership for feedback-driven initiatives
  - Update sprint planning with feedback items
  - Create tickets and specs for implementation

Friday - Communication and Closure:
  - Send updates to users who provided feedback
  - Publish weekly feedback summary to team
  - Update public roadmap with feedback-driven items
```

### 7.2 Feedback Quality and Governance

#### **Feedback Quality Assurance**
```python
class FeedbackQualityAssurance:
    def __init__(self):
        self.quality_checker = FeedbackQualityChecker()
        self.spam_detector = SpamDetector()
        
    async def ensure_feedback_quality(self, feedback: Feedback) -> QualityAssessment:
        """Assess and ensure feedback quality"""
        
        quality_checks = {
            'is_spam': await self.spam_detector.is_spam(feedback),
            'is_constructive': await self.quality_checker.is_constructive(feedback),
            'has_context': await self.quality_checker.has_sufficient_context(feedback),
            'is_actionable': await self.quality_checker.is_actionable(feedback),
            'language_appropriate': await self.quality_checker.check_language(feedback)
        }
        
        overall_quality = self.calculate_quality_score(quality_checks)
        
        # Take action based on quality
        if quality_checks['is_spam']:
            await self.mark_as_spam(feedback)
        elif overall_quality < 0.5:
            await self.request_clarification(feedback)
        elif overall_quality > 0.8:
            await self.prioritize_high_quality_feedback(feedback)
        
        return QualityAssessment(
            feedback_id=feedback.id,
            quality_score=overall_quality,
            quality_checks=quality_checks,
            action_taken=self.determine_action(quality_checks, overall_quality)
        )
```

---

## 8. Métricas de Sucesso do Sistema de Feedback

### 8.1 KPIs do Sistema de Feedback

#### **Volume e Engagement Metrics**
```yaml
Collection Metrics:
  feedback_volume_weekly: target 500+
  response_rate_surveys: target 25%+
  nps_response_rate: target 40%+
  feedback_quality_score: target 0.75+

Processing Metrics:
  time_to_analysis: target <24h
  categorization_accuracy: target 90%+
  sentiment_analysis_accuracy: target 85%+
  actionable_insights_rate: target 60%+

Implementation Metrics:
  feedback_to_feature_rate: target 20%
  time_to_implementation: target <8 weeks
  user_satisfaction_post_implementation: target +15%
  feature_adoption_feedback_driven: target 70%+
```

### 8.2 ROI do Programa de Feedback

#### **Business Impact Measurement**
```python
class FeedbackROICalculator:
    def calculate_feedback_program_roi(self) -> FeedbackROI:
        """Calculate comprehensive ROI of feedback program"""
        
        # Costs
        costs = FeedbackProgramCosts(
            tools_and_infrastructure=5000,  # monthly
            team_time=15000,  # monthly
            incentives_and_rewards=2000,  # monthly
            external_research=3000  # monthly
        )
        
        # Benefits
        benefits = FeedbackProgramBenefits(
            reduced_churn=self.calculate_churn_reduction_value(),
            increased_satisfaction=self.calculate_satisfaction_value(), 
            feature_success_rate=self.calculate_feature_success_value(),
            development_efficiency=self.calculate_efficiency_value(),
            word_of_mouth=self.calculate_wom_value()
        )
        
        monthly_roi = (benefits.total_value - costs.total_cost) / costs.total_cost
        
        return FeedbackROI(
            monthly_roi=monthly_roi,
            annual_roi=monthly_roi * 12,
            payback_period=costs.total_cost / benefits.monthly_value,
            net_value=benefits.total_value - costs.total_cost
        )
```

---

