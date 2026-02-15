# Architecture Documentation

## System Overview

The AI Political Navigator is a full-stack web application consisting of:
- **Backend**: FastAPI server with Google Gemini AI integration
- **Frontend**: React SPA with interactive visualizations
- **AI Layer**: Google Gemini for content generation and analysis

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    Frontend (React)                      │
│  ┌──────────┐  ┌──────────┐  ┌────────────────────┐   │
│  │ World Map│  │ Country  │  │  AI Chat Widget    │   │
│  │   Page   │  │   Page   │  │                    │   │
│  └──────────┘  └──────────┘  └────────────────────┘   │
│         │              │               │                │
│         └──────────────┴───────────────┘                │
│                        │                                │
│                  ┌─────▼─────┐                         │
│                  │ API Client │                         │
│                  │  (Axios)   │                         │
│                  └─────┬─────┘                         │
└────────────────────────┼───────────────────────────────┘
                         │ HTTP/WebSocket
                         │
┌────────────────────────▼───────────────────────────────┐
│              Backend (FastAPI)                          │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐ │
│  │   REST API   │  │  WebSocket   │  │  AI Service │ │
│  │  Endpoints   │  │   Handler    │  │             │ │
│  └──────┬───────┘  └──────────────┘  └──────┬──────┘ │
│         │                                     │         │
│         └─────────────┬───────────────────────┘         │
│                       │                                 │
│              ┌────────▼─────────┐                      │
│              │  Data Models     │                      │
│              │  (Pydantic)      │                      │
│              └────────┬─────────┘                      │
│                       │                                 │
│              ┌────────▼─────────┐                      │
│              │  In-Memory DB    │                      │
│              │  (Python Dict)   │                      │
│              └──────────────────┘                      │
└─────────────────────┬───────────────────────────────────┘
                      │
                      │ API Calls
                      │
┌─────────────────────▼───────────────────────────────────┐
│              Google Gemini AI                            │
│  - Content Generation                                   │
│  - Text Analysis                                        │
│  - Chat Responses                                       │
└─────────────────────────────────────────────────────────┘
```

## Component Architecture

### Frontend Components

#### Pages
1. **WorldMapPage**
   - Entry point of the application
   - Interactive world map using react-simple-maps
   - Country search functionality
   - Sample data initialization

2. **CountryPage**
   - Dynamic route: `/country/:countryId`
   - Category-based event filtering
   - Displays historical figures
   - Highlighted text with entity links

3. **FigurePage**
   - Dynamic route: `/figure/:figureId`
   - Biography and achievements
   - Related countries navigation

#### Components
1. **WorldMap**
   - SVG-based world map
   - Click handlers for countries
   - Hover tooltips
   - Visual highlighting

2. **ChatWidget**
   - Floating chat button
   - Expandable chat window
   - Message history
   - Context-aware conversations

3. **HighlightedText**
   - Automatic entity detection
   - Interactive highlighting
   - Navigation to entities

### Backend Architecture

#### API Layer
- FastAPI application with CORS middleware
- RESTful endpoints for CRUD operations
- WebSocket support for real-time updates
- Automatic OpenAPI documentation

#### Data Models
```python
Country
├── id: str
├── name: str
├── code: str (ISO 3166-1)
├── capital: str
├── population: int
├── gdp: float
├── government_type: str
├── current_events: List[CountryEvent]
└── historical_figures: List[HistoricalFigure]

CountryEvent
├── id: str
├── title: str
├── date: str
├── category: str (enum)
├── description: str
├── severity: str (low/medium/high)
├── related_countries: List[str]
└── related_figures: List[str]

HistoricalFigure
├── id: str
├── name: str
├── role: str
├── birth_year: int?
├── death_year: int?
├── biography: str
├── achievements: List[str]
└── related_countries: List[str]
```

#### AI Integration
- **Gemini API**: `gemini-2.0-flash-exp`
- **Temperature**: 0.5-0.7 (balanced creativity/accuracy)
- **Prompts**: Structured JSON output for consistency
- **Error Handling**: Fallbacks and retry logic

## Data Flow

### Country Information Generation
```
User clicks "Generate Country Data"
    ↓
Frontend → POST /api/generate-country-info/{name}
    ↓
Backend constructs detailed prompt
    ↓
Gemini AI generates comprehensive JSON
    ↓
Backend validates with Pydantic models
    ↓
Store in in-memory database
    ↓
Return structured data to frontend
    ↓
Frontend displays country page
```

### Chat Interaction
```
User sends message
    ↓
Frontend → POST /api/chat
    ↓
Backend builds context-aware prompt
    ↓
Includes page context + chat history
    ↓
Gemini AI generates response
    ↓
Backend returns formatted response
    ↓
Frontend displays in chat widget
```

### Text Analysis & Highlighting
```
Component receives text content
    ↓
Frontend → POST /api/analyze-text
    ↓
Backend asks Gemini to identify entities
    ↓
AI returns entity positions and types
    ↓
Backend validates entity data
    ↓
Frontend renders with highlighting
    ↓
User clicks highlighted entity
    ↓
Navigate to entity page
```

## State Management

### Frontend State (Zustand)
```javascript
{
  // Data
  countries: Country[],
  currentCountry: Country | null,
  
  // UI
  isChatOpen: boolean,
  isLoading: boolean,
  
  // Chat
  chatHistory: ChatMessage[],
  chatContext: object | null,
  
  // Actions
  setCountries, setCurrentCountry,
  toggleChat, openChat, closeChat,
  setChatContext, addChatMessage,
  clearChatHistory, setLoading
}
```

### Backend State
- **In-Memory Database**: Python dictionaries
  - `COUNTRIES_DB: Dict[str, Country]`
  - `FIGURES_DB: Dict[str, HistoricalFigure]`
- **WebSocket Connections**: Active connection list
- **No persistent storage** (for prototype simplicity)

## API Endpoints

### Countries
- `GET /api/countries` - List all countries
- `GET /api/countries/{country_id}` - Get specific country
- `POST /api/generate-country-info/{name}` - Generate AI content

### Figures
- `GET /api/figures/{figure_id}` - Get figure details

### AI Services
- `POST /api/chat` - Chat with AI assistant
- `POST /api/analyze-text` - Analyze text for entities

### System
- `GET /` - Health check
- `WS /ws` - WebSocket connection

## Security Considerations

### Current Implementation
- CORS: Allow all origins (development mode)
- No authentication/authorization
- API key stored in backend environment
- Client-side validation only

### Production Recommendations
1. **Authentication**: Add JWT-based auth
2. **CORS**: Restrict to specific domains
3. **Rate Limiting**: Prevent API abuse
4. **Input Validation**: Server-side sanitization
5. **API Key Protection**: Use secure key management
6. **HTTPS**: Enable SSL/TLS
7. **Database**: Replace in-memory with PostgreSQL

## Performance Optimization

### Current Optimizations
1. **Code Splitting**: React lazy loading
2. **SVG Maps**: Lightweight vector graphics
3. **State Management**: Minimal re-renders with Zustand
4. **API Caching**: Browser cache headers
5. **WebSocket**: Efficient real-time updates

### Future Optimizations
1. **Database Indexing**: For faster queries
2. **CDN**: Serve static assets
3. **Redis Caching**: Cache AI responses
4. **Image Optimization**: Compress and lazy-load
5. **Server-Side Rendering**: Improve SEO
6. **GraphQL**: Reduce over-fetching

## Deployment Architecture

### Development
```
localhost:8000 ← Backend (FastAPI + Uvicorn)
localhost:3000 ← Frontend (Vite Dev Server)
```

### Production (Recommended)
```
Frontend (Vercel/Netlify)
    ↓ HTTPS
Backend (Railway/Render/Heroku)
    ↓ API Key
Google Gemini AI
```

## Technology Decisions

### Why FastAPI?
- Async support for better performance
- Automatic API documentation
- Pydantic integration for data validation
- Modern Python features

### Why React?
- Component-based architecture
- Large ecosystem
- Excellent developer experience
- Great for interactive UIs

### Why Gemini?
- Advanced language understanding
- Structured output support
- Reasonable pricing
- Good for political analysis

### Why In-Memory DB?
- Prototype simplicity
- Fast development
- No setup required
- Easy to migrate to persistent storage

## Future Architecture

### Planned Enhancements
1. **PostgreSQL Database**: Persistent storage
2. **Redis Cache**: AI response caching
3. **Authentication**: User accounts
4. **Real-time News**: External API integration
5. **Analytics**: User behavior tracking
6. **Mobile App**: React Native version
7. **Microservices**: Separate AI service
8. **CDN**: Global content delivery

## Scalability Considerations

### Current Limitations
- In-memory storage (not persistent)
- Single server instance
- No load balancing
- Limited concurrent users

### Scaling Strategy
1. **Horizontal Scaling**: Multiple backend instances
2. **Database Replication**: Read replicas
3. **Caching Layer**: Redis for hot data
4. **Queue System**: Background job processing
5. **CDN**: Static asset distribution
6. **Monitoring**: Application performance tracking

## Development Workflow

```
1. Feature Planning
2. Backend API Development
3. Frontend Component Development
4. Integration Testing
5. AI Prompt Optimization
6. UI/UX Refinement
7. Documentation Update
8. Deployment
```

## Testing Strategy

### Unit Tests
- Backend: pytest for API endpoints
- Frontend: Jest + React Testing Library

### Integration Tests
- API + AI interaction tests
- Component integration tests

### E2E Tests
- Playwright for user workflows
- Critical path testing

---

This architecture is designed for rapid development and easy iteration while maintaining a clear path to production scalability.
