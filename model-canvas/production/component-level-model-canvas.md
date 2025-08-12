# Canvas de Nível de Componente - CheckIn

## Visão Geral
Este canvas detalha a arquitetura de componentes do CheckIn, especificando a responsabilidade, interfaces, dependências e padrões de cada componente do sistema para garantir uma implementação modular e escalável.

---

## 1. Arquitetura de Componentes Frontend

### 1.1 Core Components

#### **AssistantChat**
**Responsabilidade**: Interface conversacional com IA para recomendações
```typescript
interface AssistantChatProps {
  userContext: UserContext;
  onSuggestionGenerated: (suggestions: Suggestion[]) => void;
  onUserFeedback: (feedback: UserFeedback) => void;
}

interface AssistantChatState {
  messages: ChatMessage[];
  isProcessing: boolean;
  currentContext: ConversationContext;
}
```

**Dependências**:
- `ChatService` para comunicação com backend
- `ContextProcessor` para análise de entrada do usuário
- `TypingIndicator` para feedback visual

**Padrões Implementados**:
- State management com useReducer
- Real-time updates via WebSocket
- Error boundaries para robustez
- Debouncing para input do usuário

#### **SuggestionCard**
**Responsabilidade**: Exibir recomendação individual com detalhes
```typescript
interface SuggestionCardProps {
  suggestion: Suggestion;
  onAccept: (suggestion: Suggestion) => void;
  onReject: (suggestion: Suggestion) => void;
  onViewDetails: (venueId: string) => void;
  variant: 'compact' | 'detailed' | 'featured';
}
```

**Sub-componentes**:
- `VenueImage` - Imagem do estabelecimento
- `VenueInfo` - Nome, tipo, distância, rating
- `VibeTagsDisplay` - Tags de ambiente atual
- `ActionButtons` - Aceitar, rejeitar, detalhes

**Estados**:
- `loading` - Carregando informações adicionais
- `selected` - Selecionado pelo usuário
- `rejected` - Rejeitado pelo usuário

#### **CheckInButton**
**Responsabilidade**: Iniciar processo de check-in
```typescript
interface CheckInButtonProps {
  venue: Venue;
  currentLocation: Coordinates;
  onCheckInComplete: (checkIn: CheckIn) => void;
  disabled?: boolean;
}
```

**Estados**:
- `available` - Pronto para check-in
- `processing` - Processando check-in
- `success` - Check-in realizado
- `error` - Erro no processo

### 1.2 Layout Components

#### **MainNavigation**
**Responsabilidade**: Navegação principal entre seções
```typescript
interface MainNavigationProps {
  currentRoute: string;
  unreadNotifications: number;
  activeCheckIn?: CheckIn;
}
```

**Funcionalidades**:
- Badge de notificações não lidas
- Indicador de check-in ativo
- Animações de transição suaves
- Deep linking support

#### **TopHeader**
**Responsabilidade**: Header contextual com ações
```typescript
interface TopHeaderProps {
  title: string;
  actions?: HeaderAction[];
  showSearch?: boolean;
  showProfile?: boolean;
}
```

### 1.3 Social Components

#### **EventCard**
**Responsabilidade**: Exibir evento social com RSVP
```typescript
interface EventCardProps {
  event: SocialEvent;
  currentUser: User;
  onRSVP: (eventId: string, response: RSVPResponse) => void;
  showAttendees?: boolean;
}
```

**Sub-componentes**:
- `EventDetails` - Informações do evento
- `VenuePreview` - Preview do local
- `AttendeesList` - Lista de participantes
- `RSVPButtons` - Botões de confirmação

#### **FriendsList**
**Responsabilidade**: Lista de amigos com status de presença
```typescript
interface FriendsListProps {
  friends: Friend[];
  showPresence?: boolean;
  onInvite?: (friendId: string) => void;
  filterByAvailability?: boolean;
}
```

---

## 2. Arquitetura de Componentes Backend

### 2.1 API Components

#### **RecommendationController**
**Responsabilidade**: Endpoint para geração de recomendações
```python
@router.post("/recommendations")
async def generate_recommendations(
    request: RecommendationRequest,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
) -> RecommendationResponse:
```

**Dependências**:
- `RecommendationService` - Lógica de negócio
- `AIService` - Integração com OpenAI
- `VenueRepository` - Dados de estabelecimentos
- `UserPreferencesRepository` - Preferências do usuário

#### **CheckInController**
**Responsabilidade**: Gerenciar ciclo de check-ins
```python
@router.post("/checkins")
async def create_checkin(
    checkin_data: CheckInCreate,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
) -> CheckInResponse:
```

#### **SocialController**
**Responsabilidade**: Features sociais (eventos, convites)
```python
@router.post("/events")
async def create_event(
    event_data: EventCreate,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
) -> EventResponse:
```

### 2.2 Service Components

#### **RecommendationService**
**Responsabilidade**: Orquestrar geração de recomendações
```python
class RecommendationService:
    def __init__(
        self,
        ai_service: AIService,
        venue_repository: VenueRepository,
        user_preferences_service: UserPreferencesService
    ):
        self.ai_service = ai_service
        self.venue_repository = venue_repository
        self.user_preferences_service = user_preferences_service
    
    async def generate_recommendations(
        self, 
        context: UserContext
    ) -> List[Suggestion]:
```

**Métodos principais**:
- `generate_recommendations()` - Gerar sugestões
- `refine_based_on_feedback()` - Melhorar com feedback
- `get_contextual_venues()` - Filtrar estabelecimentos
- `rank_suggestions()` - Ordenar por relevância

#### **AIService**
**Responsabilidade**: Integração com OpenAI e processamento de IA
```python
class AIService:
    def __init__(self, openai_client: OpenAI):
        self.client = openai_client
        self.conversation_memory = ConversationMemory()
    
    async def process_user_request(
        self, 
        user_input: str, 
        context: UserContext
    ) -> AIResponse:
```

**Funcionalidades**:
- Processamento de linguagem natural
- Geração de prompts contextuais
- Gerenciamento de memória de conversa
- Fallback para regras quando IA falha

#### **SocialCoordinationService**
**Responsabilidade**: Lógica de coordenação social
```python
class SocialCoordinationService:
    async def create_event(
        self, 
        event_data: EventCreate, 
        organizer: User
    ) -> SocialEvent:
    
    async def send_invitations(
        self, 
        event: SocialEvent, 
        invitees: List[User]
    ) -> List[Invitation]:
    
    async def process_rsvp(
        self, 
        invitation: Invitation, 
        response: RSVPResponse
    ) -> RSVP:
```

### 2.3 Repository Components

#### **VenueRepository**
**Responsabilidade**: Acesso a dados de estabelecimentos
```python
class VenueRepository:
    async def find_by_criteria(
        self, 
        criteria: VenueSearchCriteria
    ) -> List[Venue]:
    
    async def get_venue_with_real_time_data(
        self, 
        venue_id: str
    ) -> VenueWithRealTimeInfo:
    
    async def update_venue_vibe_tags(
        self, 
        venue_id: str, 
        tags: List[VibeTag]
    ) -> None:
```

#### **UserRepository**
**Responsabilidade**: Gerenciamento de dados de usuários
```python
class UserRepository:
    async def get_user_preferences(
        self, 
        user_id: str
    ) -> UserPreferences:
    
    async def update_preferences_from_feedback(
        self, 
        user_id: str, 
        feedback: UserFeedback
    ) -> None:
    
    async def get_user_social_network(
        self, 
        user_id: str
    ) -> List[User]:
```

---

## 3. Componentes de Infraestrutura

### 3.1 Data Layer Components

#### **DatabaseManager**
**Responsabilidade**: Gerenciamento de conexões e transações
```python
class DatabaseManager:
    def __init__(self, connection_string: str):
        self.engine = create_async_engine(connection_string)
        self.session_factory = async_sessionmaker(self.engine)
    
    async def get_session(self) -> AsyncSession:
    async def execute_in_transaction(self, func: Callable) -> Any:
```

#### **CacheManager**
**Responsabilidade**: Gerenciamento de cache Redis
```python
class CacheManager:
    async def get_recommendations_cache(
        self, 
        cache_key: str
    ) -> Optional[List[Suggestion]]:
    
    async def set_venue_data_cache(
        self, 
        venue_id: str, 
        data: VenueData, 
        ttl: int = 3600
    ) -> None:
```

### 3.2 External Integration Components

#### **GooglePlacesConnector**
**Responsabilidade**: Integração com Google Places API
```python
class GooglePlacesConnector:
    async def get_venue_details(
        self, 
        place_id: str
    ) -> GooglePlaceDetails:
    
    async def search_nearby_venues(
        self, 
        location: Coordinates, 
        radius: int
    ) -> List[GooglePlace]:
```

#### **NotificationService**
**Responsabilidade**: Envio de notificações push
```python
class NotificationService:
    async def send_recommendation_notification(
        self, 
        user: User, 
        suggestion: Suggestion
    ) -> None:
    
    async def send_social_notification(
        self, 
        user: User, 
        event: NotificationEvent
    ) -> None:
```

---

## 4. Padrões de Componentes

### 4.1 Padrões de Design Frontend

#### **Compound Components Pattern**
```typescript
// Usage example
<SuggestionCard suggestion={suggestion}>
  <SuggestionCard.Image />
  <SuggestionCard.Content>
    <SuggestionCard.Title />
    <SuggestionCard.VibeTags />
    <SuggestionCard.Distance />
  </SuggestionCard.Content>
  <SuggestionCard.Actions>
    <SuggestionCard.AcceptButton />
    <SuggestionCard.RejectButton />
  </SuggestionCard.Actions>
</SuggestionCard>
```

#### **Render Props Pattern**
```typescript
<DataFetcher
  url="/api/recommendations"
  render={({ data, loading, error }) => (
    <SuggestionsList 
      suggestions={data} 
      loading={loading} 
      error={error} 
    />
  )}
/>
```

#### **Custom Hooks Pattern**
```typescript
function useRecommendations(context: UserContext) {
  const [suggestions, setSuggestions] = useState<Suggestion[]>([]);
  const [loading, setLoading] = useState(false);
  
  const generateRecommendations = useCallback(async () => {
    // Implementation
  }, [context]);
  
  return { suggestions, loading, generateRecommendations };
}
```

### 4.2 Padrões de Design Backend

#### **Dependency Injection Pattern**
```python
class ServiceContainer:
    def __init__(self):
        self._services = {}
    
    def register(self, interface: Type, implementation: Any):
        self._services[interface] = implementation
    
    def get(self, interface: Type) -> Any:
        return self._services[interface]
```

#### **Repository Pattern**
```python
class BaseRepository(ABC):
    @abstractmethod
    async def get_by_id(self, id: str) -> Optional[T]:
        pass
    
    @abstractmethod
    async def create(self, entity: T) -> T:
        pass
    
    @abstractmethod
    async def update(self, entity: T) -> T:
        pass
```

#### **Command Pattern for Business Logic**
```python
class GenerateRecommendationsCommand:
    def __init__(
        self,
        user_context: UserContext,
        recommendation_service: RecommendationService
    ):
        self.user_context = user_context
        self.recommendation_service = recommendation_service
    
    async def execute(self) -> List[Suggestion]:
        return await self.recommendation_service.generate_recommendations(
            self.user_context
        )
```

---

## 5. Estados e Ciclo de Vida dos Componentes

### 5.1 Component State Management

#### **AssistantChat States**
```typescript
type AssistantChatState = 
  | { status: 'idle' }
  | { status: 'listening', input: string }
  | { status: 'processing', context: UserContext }
  | { status: 'responding', suggestions: Suggestion[] }
  | { status: 'error', error: ChatError };
```

#### **CheckIn Component Lifecycle**
```typescript
enum CheckInStatus {
  AVAILABLE = 'available',
  CONFIRMING_LOCATION = 'confirming_location',
  PROCESSING = 'processing',
  SUCCESS = 'success',
  ERROR = 'error'
}
```

### 5.2 Error Handling Patterns

#### **Error Boundary Component**
```typescript
interface ErrorBoundaryState {
  hasError: boolean;
  error?: Error;
  errorInfo?: ErrorInfo;
}

class ErrorBoundary extends Component<Props, ErrorBoundaryState> {
  static getDerivedStateFromError(error: Error): ErrorBoundaryState {
    return { hasError: true, error };
  }
  
  componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    // Log error to monitoring service
    this.logErrorToService(error, errorInfo);
  }
}
```

#### **Graceful Degradation**
```typescript
function SuggestionCard({ suggestion }: Props) {
  const [imageLoadError, setImageLoadError] = useState(false);
  
  return (
    <Card>
      {!imageLoadError ? (
        <img 
          src={suggestion.venue.imageUrl} 
          onError={() => setImageLoadError(true)}
        />
      ) : (
        <PlaceholderImage />
      )}
      {/* Rest of component */}
    </Card>
  );
}
```

---

## 6. Performance e Otimização

### 6.1 Frontend Optimizations

#### **Component Memoization**
```typescript
const SuggestionCard = memo(({ suggestion, onAccept, onReject }: Props) => {
  // Component implementation
}, (prevProps, nextProps) => {
  return prevProps.suggestion.id === nextProps.suggestion.id &&
         prevProps.suggestion.lastUpdated === nextProps.suggestion.lastUpdated;
});
```

#### **Lazy Loading**
```typescript
const AssistantChat = lazy(() => import('./AssistantChat'));
const SocialFeatures = lazy(() => import('./SocialFeatures'));

function App() {
  return (
    <Suspense fallback={<LoadingSpinner />}>
      <Routes>
        <Route path="/chat" element={<AssistantChat />} />
        <Route path="/social" element={<SocialFeatures />} />
      </Routes>
    </Suspense>
  );
}
```

#### **Virtual Scrolling for Lists**
```typescript
import { FixedSizeList as List } from 'react-window';

function VenueList({ venues }: Props) {
  const Row = ({ index, style }: RowProps) => (
    <div style={style}>
      <VenueCard venue={venues[index]} />
    </div>
  );
  
  return (
    <List
      height={600}
      itemCount={venues.length}
      itemSize={120}
    >
      {Row}
    </List>
  );
}
```

### 6.2 Backend Optimizations

#### **Caching Strategy**
```python
class RecommendationService:
    @cached(ttl=300)  # 5 minutes cache
    async def get_venue_recommendations(
        self, 
        user_context: UserContext
    ) -> List[Suggestion]:
        # Expensive recommendation generation
        pass
    
    @cached(ttl=3600, key_func=lambda venue_id: f"venue:{venue_id}")
    async def get_venue_details(self, venue_id: str) -> VenueDetails:
        # Cached venue details
        pass
```

#### **Database Query Optimization**
```python
# Eager loading to prevent N+1 queries
async def get_venues_with_tags(
    self, 
    venue_ids: List[str]
) -> List[VenueWithTags]:
    return await self.db.execute(
        select(Venue)
        .options(selectinload(Venue.vibe_tags))
        .where(Venue.id.in_(venue_ids))
    )
```

---

## 7. Testing Strategy por Componente

### 7.1 Frontend Component Testing

#### **Unit Tests**
```typescript
describe('SuggestionCard', () => {
  it('should call onAccept when accept button is clicked', () => {
    const mockOnAccept = jest.fn();
    const suggestion = createMockSuggestion();
    
    render(
      <SuggestionCard 
        suggestion={suggestion} 
        onAccept={mockOnAccept} 
      />
    );
    
    fireEvent.click(screen.getByText('Accept'));
    expect(mockOnAccept).toHaveBeenCalledWith(suggestion);
  });
});
```

#### **Integration Tests**
```typescript
describe('RecommendationFlow', () => {
  it('should generate recommendations when user provides context', async () => {
    const mockAPI = new MockRecommendationAPI();
    
    render(
      <RecommendationProvider api={mockAPI}>
        <AssistantChat />
        <SuggestionsList />
      </RecommendationProvider>
    );
    
    // Simulate user interaction
    await userEvent.type(
      screen.getByRole('textbox'), 
      'Quero sair para jantar romântico'
    );
    
    await waitFor(() => {
      expect(screen.getByText('Bistrô do Chef')).toBeInTheDocument();
    });
  });
});
```

### 7.2 Backend Component Testing

#### **Service Tests**
```python
@pytest.mark.asyncio
async def test_recommendation_service_generates_suggestions():
    # Arrange
    mock_ai_service = Mock(spec=AIService)
    mock_venue_repo = Mock(spec=VenueRepository)
    
    service = RecommendationService(mock_ai_service, mock_venue_repo)
    context = UserContext(location=Coordinates(lat=-23.5, lng=-46.6))
    
    # Act
    suggestions = await service.generate_recommendations(context)
    
    # Assert
    assert len(suggestions) == 3
    assert all(s.venue.distance_km <= 5 for s in suggestions)
```

#### **API Tests**
```python
@pytest.mark.asyncio
async def test_recommendations_endpoint():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.post(
            "/recommendations",
            json={
                "context": {
                    "location": {"lat": -23.5, "lng": -46.6},
                    "time": "2024-01-15T19:00:00Z",
                    "companion_type": "romantic"
                }
            },
            headers={"Authorization": f"Bearer {test_token}"}
        )
    
    assert response.status_code == 200
    data = response.json()
    assert "suggestions" in data
    assert len(data["suggestions"]) >= 1
```

---

## 8. Monitoramento e Observabilidade

### 8.1 Component Health Monitoring

#### **Frontend Error Tracking**
```typescript
class ComponentErrorTracker {
  static trackError(
    componentName: string, 
    error: Error, 
    props: any
  ) {
    analytics.track('Component Error', {
      component: componentName,
      error: error.message,
      stack: error.stack,
      props: JSON.stringify(props, null, 2)
    });
  }
}
```

#### **Performance Monitoring**
```typescript
function usePerformanceTracker(componentName: string) {
  useEffect(() => {
    const startTime = performance.now();
    
    return () => {
      const endTime = performance.now();
      analytics.track('Component Render Time', {
        component: componentName,
        renderTime: endTime - startTime
      });
    };
  }, [componentName]);
}
```

### 8.2 Backend Component Metrics

#### **Service Performance Tracking**
```python
import time
from functools import wraps

def track_performance(operation_name: str):
    def decorator(func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            start_time = time.time()
            try:
                result = await func(*args, **kwargs)
                metrics.counter(f"{operation_name}.success").increment()
                return result
            except Exception as e:
                metrics.counter(f"{operation_name}.error").increment()
                raise
            finally:
                duration = time.time() - start_time
                metrics.histogram(f"{operation_name}.duration").observe(duration)
        return wrapper
    return decorator
```

---

## 9. Deployment e Versionamento

### 9.1 Component Versioning Strategy

#### **Semantic Versioning for Components**
```json
{
  "@checkin/suggestion-card": "^1.2.3",
  "@checkin/assistant-chat": "^2.0.1",
  "@checkin/social-components": "^1.5.0"
}
```

#### **Breaking Changes Management**
```typescript
// v1.x.x - Legacy API
interface SuggestionCardProps {
  suggestion: Suggestion;
  onAction: (action: string, suggestion: Suggestion) => void;
}

// v2.x.x - New API with backward compatibility
interface SuggestionCardProps {
  suggestion: Suggestion;
  onAccept?: (suggestion: Suggestion) => void;
  onReject?: (suggestion: Suggestion) => void;
  onAction?: (action: string, suggestion: Suggestion) => void; // deprecated
}
```

### 9.2 Component Deployment Pipeline

#### **Individual Component Testing**
```yaml
# .github/workflows/component-test.yml
name: Component Tests
on:
  push:
    paths:
      - 'src/components/**'

jobs:
  test-components:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Test Changed Components
        run: |
          CHANGED_COMPONENTS=$(git diff --name-only HEAD~1 | grep 'src/components' | cut -d'/' -f3 | sort -u)
          for component in $CHANGED_COMPONENTS; do
            npm test -- --testPathPattern="$component"
          done
```

---

**Responsável**: Henrique Fontaine (Arquiteto Técnico)  
**Colaboração**: Equipe de Desenvolvimento  
**Manutenção**: Revisão mensal da arquitetura  
**Última atualização**: Dezembro 2024
