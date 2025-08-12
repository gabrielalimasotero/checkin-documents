# Canvas de Teste e Validação - CheckIn

## Visão Geral
Este canvas define a estratégia completa de testes e validação para o CheckIn, cobrindo desde testes unitários até validação em produção, garantindo qualidade, confiabilidade e experiência do usuário em todos os níveis.

---

## 1. Estratégia de Testes - Pirâmide de Testes

### 1.1 Test Pyramid Implementation

```
                    ┌─────────────────┐
                    │   E2E Tests     │  ←── 5% (Poucos, críticos)
                    │   (Cypress)     │
                    └─────────────────┘
                  ┌───────────────────────┐
                  │  Integration Tests    │  ←── 15% (APIs, componentes)
                  │  (Jest + Supertest)   │
                  └───────────────────────┘
              ┌─────────────────────────────────┐
              │         Unit Tests              │  ←── 80% (Funções, métodos)
              │    (Jest + React Testing)       │
              └─────────────────────────────────┘
```

### 1.2 Cobertura de Testes por Camada

| Tipo de Teste | Cobertura Target | Ferramentas | Responsabilidade |
|---------------|------------------|-------------|------------------|
| **Unit Tests** | 85%+ | Jest, React Testing Library | Desenvolvedores |
| **Integration Tests** | 70%+ | Supertest, MSW | Equipe QA + Devs |
| **E2E Tests** | Fluxos críticos | Cypress, Playwright | QA Lead |
| **Visual Tests** | Componentes UI | Chromatic, Percy | Frontend Team |
| **Performance Tests** | APIs críticas | Artillery, k6 | DevOps + Backend |
| **Security Tests** | Vulnerabilidades | OWASP ZAP, Snyk | Security Team |

---

## 2. Testes Unitários

### 2.1 Frontend Unit Testing

#### **Component Testing Strategy**
```typescript
// AssistantChat.test.tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { AssistantChat } from '../AssistantChat';
import { mockRecommendationAPI } from '../../__mocks__/api';

describe('AssistantChat Component', () => {
  beforeEach(() => {
    mockRecommendationAPI.reset();
  });

  it('should generate recommendations when user provides context', async () => {
    // Arrange
    const mockSuggestions = [
      { id: '1', name: 'Bistrô do Chef', type: 'restaurant' },
      { id: '2', name: 'Bar do João', type: 'bar' }
    ];
    mockRecommendationAPI.mockRecommendations(mockSuggestions);

    // Act
    render(<AssistantChat />);
    
    const input = screen.getByPlaceholderText('Tell me what you want to do...');
    fireEvent.change(input, { target: { value: 'Quero jantar romântico' } });
    fireEvent.click(screen.getByText('Send'));

    // Assert
    await waitFor(() => {
      expect(screen.getByText('Bistrô do Chef')).toBeInTheDocument();
      expect(screen.getByText('Bar do João')).toBeInTheDocument();
    });

    expect(mockRecommendationAPI.getLastRequest()).toMatchObject({
      context: expect.objectContaining({
        input: 'Quero jantar romântico',
        companion_type: 'romantic'
      })
    });
  });

  it('should handle AI service errors gracefully', async () => {
    // Arrange
    mockRecommendationAPI.mockError('AI service unavailable');

    // Act
    render(<AssistantChat />);
    fireEvent.change(
      screen.getByPlaceholderText('Tell me what you want to do...'),
      { target: { value: 'Quero sair' } }
    );
    fireEvent.click(screen.getByText('Send'));

    // Assert
    await waitFor(() => {
      expect(screen.getByText(/Unable to get recommendations/i)).toBeInTheDocument();
      expect(screen.getByText(/Try again later/i)).toBeInTheDocument();
    });
  });

  it('should maintain conversation context', async () => {
    // Arrange
    const { rerender } = render(<AssistantChat />);
    
    // First interaction
    fireEvent.change(
      screen.getByPlaceholderText('Tell me what you want to do...'),
      { target: { value: 'Quero sair hoje' } }
    );
    fireEvent.click(screen.getByText('Send'));

    await waitFor(() => {
      expect(screen.getByText(/What type of place/i)).toBeInTheDocument();
    });

    // Second interaction
    fireEvent.change(
      screen.getByPlaceholderText('Tell me what you want to do...'),
      { target: { value: 'Algo romântico' } }
    );
    fireEvent.click(screen.getByText('Send'));

    // Assert context is maintained
    const lastRequest = mockRecommendationAPI.getLastRequest();
    expect(lastRequest.conversation_history).toHaveLength(2);
    expect(lastRequest.context.companion_type).toBe('romantic');
  });
});
```

#### **Hook Testing**
```typescript
// useRecommendations.test.ts
import { renderHook, act } from '@testing-library/react';
import { useRecommendations } from '../useRecommendations';
import { QueryClient, QueryClientProvider } from 'react-query';

const createWrapper = () => {
  const queryClient = new QueryClient({
    defaultOptions: { queries: { retry: false } }
  });
  
  return ({ children }: { children: React.ReactNode }) => (
    <QueryClientProvider client={queryClient}>
      {children}
    </QueryClientProvider>
  );
};

describe('useRecommendations Hook', () => {
  it('should fetch recommendations based on context', async () => {
    // Arrange
    const context = {
      location: { lat: -23.5, lng: -46.6 },
      companion_type: 'friends',
      time: new Date()
    };

    // Act
    const { result, waitFor } = renderHook(
      () => useRecommendations(context),
      { wrapper: createWrapper() }
    );

    // Assert
    expect(result.current.isLoading).toBe(true);
    
    await waitFor(() => result.current.isSuccess);
    
    expect(result.current.data).toBeDefined();
    expect(result.current.data?.suggestions).toHaveLength(3);
  });

  it('should invalidate cache when context changes', async () => {
    // Arrange
    const initialContext = { companion_type: 'alone' };
    const { result, rerender } = renderHook(
      ({ context }) => useRecommendations(context),
      { 
        wrapper: createWrapper(),
        initialProps: { context: initialContext }
      }
    );

    await waitFor(() => result.current.isSuccess);
    const firstData = result.current.data;

    // Act - change context
    const newContext = { companion_type: 'romantic' };
    rerender({ context: newContext });

    // Assert
    expect(result.current.isLoading).toBe(true);
    
    await waitFor(() => result.current.isSuccess);
    expect(result.current.data).not.toEqual(firstData);
  });
});
```

### 2.2 Backend Unit Testing

#### **Service Layer Testing**
```python
# test_recommendation_service.py
import pytest
from unittest.mock import Mock, AsyncMock
from app.services.recommendation_service import RecommendationService
from app.models.user_context import UserContext
from app.models.venue import Venue

class TestRecommendationService:
    @pytest.fixture
    async def service(self):
        ai_service = Mock()
        venue_repository = Mock()
        user_preferences_service = Mock()
        
        return RecommendationService(
            ai_service=ai_service,
            venue_repository=venue_repository,
            user_preferences_service=user_preferences_service
        )
    
    @pytest.fixture
    def user_context(self):
        return UserContext(
            user_id="user_123",
            location={"lat": -23.5, "lng": -46.6},
            companion_type="romantic",
            time="2024-01-15T19:00:00Z"
        )
    
    @pytest.fixture
    def mock_venues(self):
        return [
            Venue(id="venue_1", name="Bistrô Romance", category="restaurant"),
            Venue(id="venue_2", name="Wine Bar", category="bar"),
            Venue(id="venue_3", name="Café Intimate", category="cafe")
        ]
    
    async def test_generate_recommendations_success(
        self, 
        service: RecommendationService, 
        user_context: UserContext,
        mock_venues
    ):
        # Arrange
        service.venue_repository.find_venues.return_value = mock_venues
        service.ai_service.generate_recommendations.return_value = [
            {"venue_id": "venue_1", "score": 0.9, "reason": "Perfect for romantic dinner"},
            {"venue_id": "venue_2", "score": 0.8, "reason": "Intimate wine selection"}
        ]
        
        # Act
        recommendations = await service.generate_recommendations(user_context)
        
        # Assert
        assert len(recommendations) == 2
        assert recommendations[0].venue.id == "venue_1"
        assert recommendations[0].score == 0.9
        assert "romantic" in recommendations[0].reason.lower()
        
        # Verify dependencies were called correctly
        service.venue_repository.find_venues.assert_called_once()
        service.ai_service.generate_recommendations.assert_called_once_with(
            user_context, mock_venues
        )
    
    async def test_generate_recommendations_no_venues_found(
        self,
        service: RecommendationService,
        user_context: UserContext
    ):
        # Arrange
        service.venue_repository.find_venues.return_value = []
        
        # Act & Assert
        with pytest.raises(NoVenuesFoundError) as exc_info:
            await service.generate_recommendations(user_context)
        
        assert "No venues found" in str(exc_info.value)
    
    async def test_generate_recommendations_ai_service_failure(
        self,
        service: RecommendationService,
        user_context: UserContext,
        mock_venues
    ):
        # Arrange
        service.venue_repository.find_venues.return_value = mock_venues
        service.ai_service.generate_recommendations.side_effect = AIServiceError("API timeout")
        
        # Act & Assert
        with pytest.raises(RecommendationServiceError) as exc_info:
            await service.generate_recommendations(user_context)
        
        assert "AI service error" in str(exc_info.value)
    
    @pytest.mark.parametrize("companion_type,expected_venue_types", [
        ("romantic", ["restaurant", "wine_bar"]),
        ("friends", ["bar", "restaurant", "pub"]),
        ("family", ["restaurant", "cafe", "family_restaurant"]),
        ("alone", ["cafe", "bookstore_cafe", "quiet_bar"])
    ])
    async def test_venue_filtering_by_companion_type(
        self,
        service: RecommendationService,
        companion_type: str,
        expected_venue_types: list
    ):
        # Arrange
        user_context = UserContext(
            user_id="user_123",
            companion_type=companion_type
        )
        
        # Act
        filtered_venues = await service._filter_venues_by_context(user_context)
        
        # Assert
        venue_types = [venue.category for venue in filtered_venues]
        assert all(vtype in expected_venue_types for vtype in venue_types)
```

#### **Repository Layer Testing**
```python
# test_venue_repository.py
import pytest
from sqlalchemy.ext.asyncio import AsyncSession
from app.repositories.venue_repository import VenueRepository
from app.models.venue import Venue

class TestVenueRepository:
    @pytest.fixture
    async def repository(self, db_session: AsyncSession):
        return VenueRepository(db_session)
    
    @pytest.fixture
    async def sample_venues(self, db_session: AsyncSession):
        venues = [
            Venue(
                id="venue_1",
                name="Test Restaurant",
                category="restaurant",
                latitude=-23.5,
                longitude=-46.6,
                rating=4.5
            ),
            Venue(
                id="venue_2", 
                name="Test Bar",
                category="bar",
                latitude=-23.51,
                longitude=-46.61,
                rating=4.2
            )
        ]
        
        for venue in venues:
            db_session.add(venue)
        await db_session.commit()
        
        return venues
    
    async def test_find_venues_by_location(
        self,
        repository: VenueRepository,
        sample_venues
    ):
        # Arrange
        search_criteria = VenueSearchCriteria(
            location={"lat": -23.5, "lng": -46.6},
            radius_km=2.0
        )
        
        # Act
        venues = await repository.find_venues(search_criteria)
        
        # Assert
        assert len(venues) == 2
        assert all(venue.name in ["Test Restaurant", "Test Bar"] for venue in venues)
    
    async def test_find_venues_by_category(
        self,
        repository: VenueRepository,
        sample_venues
    ):
        # Arrange
        search_criteria = VenueSearchCriteria(
            categories=["restaurant"]
        )
        
        # Act
        venues = await repository.find_venues(search_criteria)
        
        # Assert
        assert len(venues) == 1
        assert venues[0].name == "Test Restaurant"
        assert venues[0].category == "restaurant"
    
    async def test_find_venues_by_rating(
        self,
        repository: VenueRepository,
        sample_venues
    ):
        # Arrange
        search_criteria = VenueSearchCriteria(
            min_rating=4.3
        )
        
        # Act
        venues = await repository.find_venues(search_criteria)
        
        # Assert
        assert len(venues) == 1
        assert venues[0].rating >= 4.3
```

---

## 3. Testes de Integração

### 3.1 API Integration Tests

#### **Endpoint Testing**
```python
# test_recommendation_api.py
import pytest
from httpx import AsyncClient
from app.main import app

class TestRecommendationAPI:
    @pytest.fixture
    async def client(self):
        async with AsyncClient(app=app, base_url="http://test") as ac:
            yield ac
    
    @pytest.fixture
    async def authenticated_headers(self, client: AsyncClient):
        # Create test user and get auth token
        response = await client.post("/auth/login", json={
            "email": "test@example.com",
            "password": "testpass123"
        })
        token = response.json()["access_token"]
        return {"Authorization": f"Bearer {token}"}
    
    async def test_generate_recommendations_success(
        self,
        client: AsyncClient,
        authenticated_headers: dict
    ):
        # Arrange
        request_data = {
            "context": {
                "location": {"lat": -23.5, "lng": -46.6},
                "companion_type": "romantic",
                "time": "2024-01-15T19:00:00Z"
            }
        }
        
        # Act
        response = await client.post(
            "/api/recommendations",
            json=request_data,
            headers=authenticated_headers
        )
        
        # Assert
        assert response.status_code == 200
        
        data = response.json()
        assert "suggestions" in data
        assert len(data["suggestions"]) >= 1
        assert len(data["suggestions"]) <= 5
        
        # Validate suggestion structure
        suggestion = data["suggestions"][0]
        assert "venue" in suggestion
        assert "score" in suggestion
        assert "explanation" in suggestion
        
        # Validate venue structure
        venue = suggestion["venue"]
        assert "id" in venue
        assert "name" in venue
        assert "category" in venue
        assert "location" in venue
    
    async def test_generate_recommendations_invalid_input(
        self,
        client: AsyncClient,
        authenticated_headers: dict
    ):
        # Arrange
        invalid_request = {
            "context": {
                "location": {"lat": "invalid", "lng": -46.6}  # Invalid latitude
            }
        }
        
        # Act
        response = await client.post(
            "/api/recommendations",
            json=invalid_request,
            headers=authenticated_headers
        )
        
        # Assert
        assert response.status_code == 422
        
        error_data = response.json()
        assert "detail" in error_data
        assert any("lat" in str(error).lower() for error in error_data["detail"])
    
    async def test_generate_recommendations_unauthorized(
        self,
        client: AsyncClient
    ):
        # Arrange
        request_data = {
            "context": {
                "location": {"lat": -23.5, "lng": -46.6}
            }
        }
        
        # Act
        response = await client.post(
            "/api/recommendations",
            json=request_data
        )
        
        # Assert
        assert response.status_code == 401
    
    async def test_generate_recommendations_rate_limiting(
        self,
        client: AsyncClient,
        authenticated_headers: dict
    ):
        # Arrange
        request_data = {
            "context": {
                "location": {"lat": -23.5, "lng": -46.6}
            }
        }
        
        # Act - Make multiple rapid requests
        responses = []
        for _ in range(100):  # Exceed rate limit
            response = await client.post(
                "/api/recommendations",
                json=request_data,
                headers=authenticated_headers
            )
            responses.append(response)
        
        # Assert
        status_codes = [r.status_code for r in responses]
        assert 429 in status_codes  # Too Many Requests
```

### 3.2 Database Integration Tests

#### **Database Transaction Testing**
```python
# test_database_integration.py
import pytest
from sqlalchemy.ext.asyncio import AsyncSession
from app.services.checkin_service import CheckinService
from app.models.checkin import Checkin
from app.models.user import User
from app.models.venue import Venue

class TestDatabaseIntegration:
    async def test_checkin_transaction_rollback_on_error(
        self,
        db_session: AsyncSession,
        checkin_service: CheckinService
    ):
        # Arrange
        user = User(id="user_123", email="test@example.com")
        venue = Venue(id="venue_123", name="Test Venue")
        
        db_session.add(user)
        db_session.add(venue)
        await db_session.commit()
        
        # Mock an error during checkin process
        with patch.object(
            checkin_service, 
            '_update_venue_popularity', 
            side_effect=Exception("External service error")
        ):
            # Act & Assert
            with pytest.raises(CheckinServiceError):
                await checkin_service.create_checkin(
                    user_id="user_123",
                    venue_id="venue_123"
                )
        
        # Verify no checkin was created due to rollback
        checkins = await db_session.execute(
            select(Checkin).where(Checkin.user_id == "user_123")
        )
        assert len(checkins.all()) == 0
    
    async def test_concurrent_checkin_prevention(
        self,
        db_session: AsyncSession,
        checkin_service: CheckinService
    ):
        # Arrange
        user = User(id="user_123", email="test@example.com")
        venue1 = Venue(id="venue_1", name="Venue 1")
        venue2 = Venue(id="venue_2", name="Venue 2")
        
        db_session.add_all([user, venue1, venue2])
        await db_session.commit()
        
        # Act - Try to create concurrent checkins
        checkin1_task = asyncio.create_task(
            checkin_service.create_checkin("user_123", "venue_1")
        )
        checkin2_task = asyncio.create_task(
            checkin_service.create_checkin("user_123", "venue_2")
        )
        
        results = await asyncio.gather(
            checkin1_task, 
            checkin2_task, 
            return_exceptions=True
        )
        
        # Assert - Only one checkin should succeed
        successful_checkins = [r for r in results if isinstance(r, Checkin)]
        exceptions = [r for r in results if isinstance(r, Exception)]
        
        assert len(successful_checkins) == 1
        assert len(exceptions) == 1
        assert isinstance(exceptions[0], ConcurrentCheckinError)
```

---

## 4. Testes End-to-End

### 4.1 Critical User Journeys

#### **Recommendation Flow E2E Test**
```typescript
// cypress/e2e/recommendation-flow.cy.ts
describe('Recommendation Flow', () => {
  beforeEach(() => {
    // Setup test data
    cy.task('seedDatabase', {
      users: [{ id: 'test-user', email: 'test@example.com' }],
      venues: [
        { id: 'venue-1', name: 'Bistrô Romance', category: 'restaurant' },
        { id: 'venue-2', name: 'Bar Moderno', category: 'bar' }
      ]
    });
    
    // Login
    cy.login('test@example.com', 'password123');
  });

  it('should complete full recommendation journey', () => {
    // Navigate to recommendations
    cy.visit('/');
    cy.get('[data-cy="want-to-go-out-button"]').click();
    
    // Provide context
    cy.get('[data-cy="context-input"]').type('Quero jantar romântico hoje à noite');
    cy.get('[data-cy="send-context-button"]').click();
    
    // Wait for AI response
    cy.get('[data-cy="ai-thinking-indicator"]').should('be.visible');
    cy.get('[data-cy="suggestion-card"]', { timeout: 10000 }).should('have.length.gte', 1);
    
    // Verify suggestion content
    cy.get('[data-cy="suggestion-card"]').first().within(() => {
      cy.get('[data-cy="venue-name"]').should('contain.text', 'Bistrô Romance');
      cy.get('[data-cy="venue-category"]').should('contain.text', 'restaurant');
      cy.get('[data-cy="ai-explanation"]').should('contain.text', 'romântico');
    });
    
    // Accept suggestion
    cy.get('[data-cy="suggestion-card"]').first().within(() => {
      cy.get('[data-cy="accept-suggestion-button"]').click();
    });
    
    // Verify navigation to venue details
    cy.url().should('include', '/venues/venue-1');
    cy.get('[data-cy="venue-details-page"]').should('be.visible');
    
    // Perform check-in
    cy.get('[data-cy="checkin-button"]').click();
    cy.get('[data-cy="checkin-success-message"]').should('be.visible');
    
    // Verify check-in appears in user's history
    cy.visit('/profile/checkins');
    cy.get('[data-cy="checkin-history-item"]').should('contain.text', 'Bistrô Romance');
  });

  it('should handle AI service errors gracefully', () => {
    // Mock AI service error
    cy.intercept('POST', '/api/recommendations', {
      statusCode: 503,
      body: { error: 'AI service temporarily unavailable' }
    });
    
    cy.visit('/');
    cy.get('[data-cy="want-to-go-out-button"]').click();
    
    cy.get('[data-cy="context-input"]').type('Quero sair');
    cy.get('[data-cy="send-context-button"]').click();
    
    // Verify error handling
    cy.get('[data-cy="error-message"]').should('be.visible');
    cy.get('[data-cy="error-message"]').should('contain.text', 'temporarily unavailable');
    
    // Verify fallback recommendations are shown
    cy.get('[data-cy="fallback-suggestions"]').should('be.visible');
    cy.get('[data-cy="suggestion-card"]').should('have.length.gte', 1);
  });

  it('should maintain conversation context across interactions', () => {
    cy.visit('/');
    cy.get('[data-cy="want-to-go-out-button"]').click();
    
    // First interaction
    cy.get('[data-cy="context-input"]').type('Quero sair hoje');
    cy.get('[data-cy="send-context-button"]').click();
    
    cy.get('[data-cy="ai-response"]').should('contain.text', 'que tipo de lugar');
    
    // Second interaction - more specific
    cy.get('[data-cy="context-input"]').clear().type('Algo romântico para jantar');
    cy.get('[data-cy="send-context-button"]').click();
    
    // Verify suggestions are contextually appropriate
    cy.get('[data-cy="suggestion-card"]').each(($card) => {
      cy.wrap($card).within(() => {
        cy.get('[data-cy="venue-category"]').should('match', /restaurant|bistro|fine.dining/i);
      });
    });
  });
});
```

#### **Social Flow E2E Test**
```typescript
// cypress/e2e/social-flow.cy.ts
describe('Social Flow', () => {
  beforeEach(() => {
    cy.task('seedDatabase', {
      users: [
        { id: 'user-1', email: 'organizer@example.com', name: 'Ana' },
        { id: 'user-2', email: 'friend1@example.com', name: 'João' },
        { id: 'user-3', email: 'friend2@example.com', name: 'Maria' }
      ],
      friendships: [
        { user_id: 'user-1', friend_id: 'user-2', status: 'accepted' },
        { user_id: 'user-1', friend_id: 'user-3', status: 'accepted' }
      ]
    });
  });

  it('should create and manage social event', () => {
    // Login as organizer
    cy.login('organizer@example.com', 'password123');
    
    // Navigate to social features
    cy.visit('/social');
    cy.get('[data-cy="create-event-button"]').click();
    
    // Fill event details
    cy.get('[data-cy="event-title-input"]').type('Aniversário da Ana');
    cy.get('[data-cy="event-date-picker"]').click();
    cy.get('[data-cy="date-today"]').click();
    cy.get('[data-cy="event-time-input"]').type('19:00');
    
    // Select venue
    cy.get('[data-cy="venue-search-input"]').type('Bistrô Romance');
    cy.get('[data-cy="venue-suggestion"]').first().click();
    
    // Invite friends
    cy.get('[data-cy="invite-friends-section"]').within(() => {
      cy.get('[data-cy="friend-checkbox"][data-friend-id="user-2"]').check();
      cy.get('[data-cy="friend-checkbox"][data-friend-id="user-3"]').check();
    });
    
    // Create event
    cy.get('[data-cy="create-event-submit"]').click();
    
    // Verify event creation
    cy.get('[data-cy="event-created-success"]').should('be.visible');
    cy.url().should('match', /\/events\/[a-f0-9-]+/);
    
    // Verify event details
    cy.get('[data-cy="event-title"]').should('contain.text', 'Aniversário da Ana');
    cy.get('[data-cy="event-venue"]').should('contain.text', 'Bistrô Romance');
    cy.get('[data-cy="invited-friends"]').should('contain.text', 'João');
    cy.get('[data-cy="invited-friends"]').should('contain.text', 'Maria');
  });

  it('should handle RSVP responses', () => {
    // Create event first
    cy.task('createEvent', {
      organizer_id: 'user-1',
      title: 'Test Event',
      venue_id: 'venue-1',
      invitees: ['user-2', 'user-3']
    }).then((event) => {
      // Login as invitee
      cy.login('friend1@example.com', 'password123');
      
      // Visit event page
      cy.visit(`/events/${event.id}`);
      
      // RSVP Yes
      cy.get('[data-cy="rsvp-yes-button"]').click();
      cy.get('[data-cy="rsvp-confirmation"]').should('contain.text', 'confirmed');
      
      // Verify RSVP status
      cy.get('[data-cy="user-rsvp-status"]').should('contain.text', 'Going');
      
      // Change RSVP to Maybe
      cy.get('[data-cy="rsvp-maybe-button"]').click();
      cy.get('[data-cy="user-rsvp-status"]').should('contain.text', 'Maybe');
    });
  });
});
```

### 4.2 Performance Testing

#### **Load Testing with k6**
```javascript
// scripts/load-test-recommendations.js
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate } from 'k6/metrics';

export let errorRate = new Rate('errors');

export let options = {
  stages: [
    { duration: '2m', target: 10 },   // Ramp up to 10 users
    { duration: '5m', target: 50 },   // Ramp up to 50 users
    { duration: '10m', target: 100 }, // Stay at 100 users
    { duration: '5m', target: 50 },   // Ramp down to 50 users
    { duration: '2m', target: 0 },    // Ramp down to 0 users
  ],
  thresholds: {
    http_req_duration: ['p(95)<2000'], // 95% of requests must complete below 2s
    http_req_failed: ['rate<0.02'],    // Error rate must be below 2%
    errors: ['rate<0.05'],             // Custom error rate below 5%
  },
};

export function setup() {
  // Get auth token
  const loginResponse = http.post(`${__ENV.BASE_URL}/auth/login`, {
    email: 'loadtest@example.com',
    password: 'loadtest123'
  });
  
  const token = loginResponse.json('access_token');
  return { token };
}

export default function (data) {
  const headers = {
    'Authorization': `Bearer ${data.token}`,
    'Content-Type': 'application/json',
  };
  
  const contexts = [
    { companion_type: 'romantic', location: { lat: -23.5, lng: -46.6 } },
    { companion_type: 'friends', location: { lat: -23.51, lng: -46.61 } },
    { companion_type: 'family', location: { lat: -23.52, lng: -46.62 } },
    { companion_type: 'alone', location: { lat: -23.53, lng: -46.63 } }
  ];
  
  const randomContext = contexts[Math.floor(Math.random() * contexts.length)];
  
  const payload = JSON.stringify({
    context: {
      ...randomContext,
      time: new Date().toISOString()
    }
  });
  
  const response = http.post(
    `${__ENV.BASE_URL}/api/recommendations`,
    payload,
    { headers }
  );
  
  const success = check(response, {
    'status is 200': (r) => r.status === 200,
    'response time < 2000ms': (r) => r.timings.duration < 2000,
    'has suggestions': (r) => {
      const body = r.json();
      return body.suggestions && body.suggestions.length > 0;
    },
    'suggestions have required fields': (r) => {
      const body = r.json();
      if (!body.suggestions) return false;
      
      return body.suggestions.every(suggestion => 
        suggestion.venue && 
        suggestion.score && 
        suggestion.explanation
      );
    }
  });
  
  errorRate.add(!success);
  
  sleep(1); // Wait 1 second between requests
}

export function teardown(data) {
  // Cleanup if needed
  console.log('Load test completed');
}
```

---

## 5. Testes de Segurança

### 5.1 Security Testing Strategy

#### **OWASP ZAP Integration**
```yaml
# .github/workflows/security-scan.yml
name: Security Scan

on:
  push:
    branches: [main]
  schedule:
    - cron: '0 2 * * 1' # Weekly on Monday 2 AM

jobs:
  security-scan:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    
    - name: Start application
      run: |
        docker-compose -f docker-compose.test.yml up -d
        sleep 30 # Wait for app to start
    
    - name: Run OWASP ZAP scan
      uses: zaproxy/action-full-scan@v0.4.0
      with:
        target: 'http://localhost:3000'
        rules_file_name: '.zap/rules.tsv'
        cmd_options: '-a'
    
    - name: Upload ZAP results
      uses: actions/upload-artifact@v2
      with:
        name: zap-report
        path: report_html.html
    
    - name: Check for high/medium vulnerabilities
      run: |
        HIGH_VULN=$(grep -c "High" report_json.json || true)
        MEDIUM_VULN=$(grep -c "Medium" report_json.json || true)
        
        if [ "$HIGH_VULN" -gt 0 ] || [ "$MEDIUM_VULN" -gt 5 ]; then
          echo "Security vulnerabilities found!"
          exit 1
        fi
```

#### **API Security Tests**
```python
# test_api_security.py
import pytest
from httpx import AsyncClient

class TestAPISecurity:
    async def test_sql_injection_protection(self, client: AsyncClient, auth_headers):
        # Test SQL injection in query parameters
        malicious_inputs = [
            "'; DROP TABLE users; --",
            "1' OR '1'='1",
            "admin'/*",
            "' UNION SELECT * FROM users --"
        ]
        
        for malicious_input in malicious_inputs:
            response = await client.get(
                f"/api/venues?search={malicious_input}",
                headers=auth_headers
            )
            
            # Should not return 500 or expose database errors
            assert response.status_code != 500
            assert "sql" not in response.text.lower()
            assert "error" not in response.json().get("message", "").lower()
    
    async def test_xss_protection(self, client: AsyncClient, auth_headers):
        # Test XSS in request body
        xss_payloads = [
            "<script>alert('xss')</script>",
            "<img src=x onerror=alert('xss')>",
            "javascript:alert('xss')",
            "<svg onload=alert('xss')>"
        ]
        
        for payload in xss_payloads:
            response = await client.post(
                "/api/events",
                json={
                    "title": payload,
                    "description": f"Event with {payload}",
                    "venue_id": "venue_123"
                },
                headers=auth_headers
            )
            
            # Verify XSS payload is sanitized
            if response.status_code == 201:
                event = response.json()
                assert "<script>" not in event["title"]
                assert "javascript:" not in event["title"]
                assert "onerror=" not in event["description"]
    
    async def test_authentication_bypass_attempts(self, client: AsyncClient):
        # Test accessing protected endpoints without auth
        protected_endpoints = [
            "/api/recommendations",
            "/api/checkins",
            "/api/events",
            "/api/users/me"
        ]
        
        for endpoint in protected_endpoints:
            response = await client.get(endpoint)
            assert response.status_code == 401
    
    async def test_authorization_bypass_attempts(self, client: AsyncClient):
        # Create two users
        user1_token = await self.create_test_user(client, "user1@test.com")
        user2_token = await self.create_test_user(client, "user2@test.com")
        
        # User 1 creates a private event
        response = await client.post(
            "/api/events",
            json={
                "title": "Private Event",
                "is_private": True,
                "venue_id": "venue_123"
            },
            headers={"Authorization": f"Bearer {user1_token}"}
        )
        event_id = response.json()["id"]
        
        # User 2 tries to access User 1's private event
        response = await client.get(
            f"/api/events/{event_id}",
            headers={"Authorization": f"Bearer {user2_token}"}
        )
        
        assert response.status_code == 403
    
    async def test_rate_limiting(self, client: AsyncClient, auth_headers):
        # Test rate limiting on API endpoints
        endpoint = "/api/recommendations"
        
        # Make rapid requests
        responses = []
        for _ in range(150):  # Exceed rate limit
            response = await client.post(
                endpoint,
                json={"context": {"location": {"lat": -23.5, "lng": -46.6}}},
                headers=auth_headers
            )
            responses.append(response.status_code)
        
        # Should get rate limited
        assert 429 in responses  # Too Many Requests
```

---

## 6. Visual Regression Testing

### 6.1 Visual Testing with Chromatic

#### **Storybook Stories for Visual Testing**
```typescript
// SuggestionCard.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
import { SuggestionCard } from './SuggestionCard';

const meta: Meta<typeof SuggestionCard> = {
  title: 'Components/SuggestionCard',
  component: SuggestionCard,
  parameters: {
    layout: 'padded',
  },
  argTypes: {
    onAccept: { action: 'accepted' },
    onReject: { action: 'rejected' },
  },
};

export default meta;
type Story = StoryObj<typeof meta>;

export const Default: Story = {
  args: {
    suggestion: {
      venue: {
        id: '1',
        name: 'Bistrô do Chef',
        category: 'restaurant',
        rating: 4.8,
        distance: '1.2km',
        imageUrl: 'https://example.com/bistro.jpg'
      },
      score: 0.9,
      explanation: 'Perfect for a romantic dinner with excellent French cuisine',
      tags: ['romantic', 'quiet', 'upscale']
    }
  },
};

export const Loading: Story = {
  args: {
    ...Default.args,
    isLoading: true,
  },
};

export const NoImage: Story = {
  args: {
    suggestion: {
      ...Default.args.suggestion,
      venue: {
        ...Default.args.suggestion.venue,
        imageUrl: null
      }
    }
  },
};

export const LongText: Story = {
  args: {
    suggestion: {
      ...Default.args.suggestion,
      venue: {
        ...Default.args.suggestion.venue,
        name: 'Bistrô do Chef com Nome Muito Longo que Pode Quebrar o Layout'
      },
      explanation: 'This is a very long explanation that tests how the component handles extensive text content and whether it maintains proper layout and typography hierarchy when dealing with verbose AI-generated recommendations that might exceed normal character limits'
    }
  },
};

export const MobileView: Story = {
  args: Default.args,
  parameters: {
    viewport: {
      defaultViewport: 'mobile1',
    },
  },
};
```

#### **Visual Regression Pipeline**
```yaml
# .github/workflows/visual-tests.yml
name: Visual Regression Tests

on:
  pull_request:
    branches: [main]

jobs:
  chromatic:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
      with:
        fetch-depth: 0 # Required for Chromatic
    
    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run Chromatic
      uses: chromaui/action@v1
      with:
        projectToken: ${{ secrets.CHROMATIC_PROJECT_TOKEN }}
        exitOnceUploaded: true
        onlyChanged: true # Only test changed components
```

---

## 7. Continuous Testing Strategy

### 7.1 Test Automation Pipeline

#### **Test Execution Strategy**
```yaml
# .github/workflows/test-pipeline.yml
name: Comprehensive Test Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  unit-tests:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        service: [frontend, backend]
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Setup environment
      uses: ./.github/actions/setup-${{ matrix.service }}
    
    - name: Run unit tests
      run: |
        cd ${{ matrix.service }}
        npm run test:unit:coverage
    
    - name: Upload coverage
      uses: codecov/codecov-action@v1
      with:
        file: ${{ matrix.service }}/coverage/lcov.info
        flags: ${{ matrix.service }}

  integration-tests:
    needs: unit-tests
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
      
      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Setup test environment
      run: |
        docker-compose -f docker-compose.test.yml up -d
        sleep 30
    
    - name: Run integration tests
      run: npm run test:integration
    
    - name: Run API tests
      run: npm run test:api

  e2e-tests:
    needs: integration-tests
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Setup application
      run: |
        docker-compose -f docker-compose.e2e.yml up -d
        ./scripts/wait-for-services.sh
    
    - name: Run E2E tests
      uses: cypress-io/github-action@v4
      with:
        start: npm run start:test
        wait-on: 'http://localhost:3000'
        wait-on-timeout: 120
        browser: chrome
        record: true
      env:
        CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  performance-tests:
    needs: integration-tests
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Setup load testing environment
      run: docker-compose -f docker-compose.perf.yml up -d
    
    - name: Run performance tests
      run: |
        docker run --rm -i grafana/k6:latest run - <scripts/load-test.js
    
    - name: Upload performance results
      uses: actions/upload-artifact@v2
      with:
        name: performance-results
        path: performance-results.json

  security-tests:
    needs: integration-tests
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Run security scan
      uses: ./.github/actions/security-scan
    
    - name: Upload security report
      uses: actions/upload-artifact@v2
      with:
        name: security-report
        path: security-report.html
```

### 7.2 Quality Gates

#### **Quality Metrics Enforcement**
```python
# scripts/quality-gates.py
import json
import sys
from pathlib import Path

class QualityGates:
    def __init__(self):
        self.thresholds = {
            'code_coverage': 85,
            'test_success_rate': 98,
            'performance_p95': 2000,  # 2 seconds
            'security_high_vulns': 0,
            'security_medium_vulns': 2
        }
    
    def check_coverage(self, coverage_file: str) -> bool:
        with open(coverage_file) as f:
            coverage_data = json.load(f)
        
        total_coverage = coverage_data['total']['lines']['pct']
        
        if total_coverage < self.thresholds['code_coverage']:
            print(f"❌ Code coverage {total_coverage}% below threshold {self.thresholds['code_coverage']}%")
            return False
        
        print(f"✅ Code coverage {total_coverage}% meets threshold")
        return True
    
    def check_test_results(self, test_results_file: str) -> bool:
        with open(test_results_file) as f:
            test_data = json.load(f)
        
        total_tests = test_data['numTotalTests']
        passed_tests = test_data['numPassedTests']
        success_rate = (passed_tests / total_tests) * 100
        
        if success_rate < self.thresholds['test_success_rate']:
            print(f"❌ Test success rate {success_rate:.1f}% below threshold {self.thresholds['test_success_rate']}%")
            return False
        
        print(f"✅ Test success rate {success_rate:.1f}% meets threshold")
        return True
    
    def check_performance(self, performance_file: str) -> bool:
        with open(performance_file) as f:
            perf_data = json.load(f)
        
        p95_response_time = perf_data['metrics']['http_req_duration']['p(95)']
        
        if p95_response_time > self.thresholds['performance_p95']:
            print(f"❌ P95 response time {p95_response_time}ms above threshold {self.thresholds['performance_p95']}ms")
            return False
        
        print(f"✅ P95 response time {p95_response_time}ms meets threshold")
        return True
    
    def run_all_checks(self) -> bool:
        all_passed = True
        
        checks = [
            ('coverage', 'coverage/coverage-summary.json', self.check_coverage),
            ('tests', 'test-results.json', self.check_test_results),
            ('performance', 'performance-results.json', self.check_performance)
        ]
        
        for check_name, file_path, check_func in checks:
            if Path(file_path).exists():
                if not check_func(file_path):
                    all_passed = False
            else:
                print(f"⚠️  {check_name} results file not found: {file_path}")
        
        if all_passed:
            print("🎉 All quality gates passed!")
        else:
            print("❌ Quality gates failed!")
        
        return all_passed

if __name__ == "__main__":
    gates = QualityGates()
    success = gates.run_all_checks()
    sys.exit(0 if success else 1)
```

---

**Responsável QA**: João Pedro (QA Lead)  
**Test Automation**: João Victor (Backend) + Lucas Luis (Frontend)  
**Performance Testing**: Lucas Emmanuel (DevOps)  
**Security Testing**: Henrique Fontaine (CTO)  
**Review Cycle**: Semanal para cobertura, diário para pipeline  
**Última atualização**: Dezembro 2024
