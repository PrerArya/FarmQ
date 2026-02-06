# Design Document - AI Farming Assistant

## 1. System Architecture

### 1.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Client Layer                          │
│  ┌──────────────────────────────────────────────────────┐  │
│  │           React.js Frontend Application              │  │
│  │  - Landing Page    - Disease Detection               │  │
│  │  - Dashboard       - Chat Interface                  │  │
│  │  - Crop Prediction - Market Predictions              │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            │
                    HTTP/WebSocket
                            │
┌─────────────────────────────────────────────────────────────┐
│                      Application Layer                       │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              FastAPI Backend Server                  │  │
│  │  - REST API Endpoints                                │  │
│  │  - WebSocket Handler                                 │  │
│  │  - CORS Middleware                                   │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            │
                ┌───────────┴───────────┐
                │                       │
┌───────────────▼──────────┐  ┌────────▼──────────────────────┐
│      AI/ML Layer         │  │    External Services          │
│  - ResNet9 Model         │  │  - Amazon Bedrock (Claude)    │
│  - Random Forest Model   │  │  - OpenWeatherMap API         │
│  - LangChain Agents      │  │  - IP Geolocation API         │
└──────────────────────────┘  └───────────────────────────────┘
                │
┌───────────────▼──────────┐
│      Data Layer          │
│  - Disease Database      │
│  - Fertilizer CSV        │
│  - Model Files (.pkl)    │
│  - Uploaded Images       │
└──────────────────────────┘
```

### 1.2 Technology Stack

**Frontend:**
- React.js 18+ (UI framework)
- React Router (navigation)
- Lucide React (icons)
- React Dropzone (file uploads)
- React Webcam (camera capture)
- Web Speech API (voice I/O)
- WebSocket API (real-time communication)

**Backend:**
- FastAPI (web framework)
- Python 3.8+
- Uvicorn (ASGI server)

**AI/ML:**
- PyTorch (deep learning)
- Scikit-learn (ML models)
- LangChain (AI orchestration)
- Amazon Bedrock (LLM service)

**Deployment:**
- Docker (containerization)
- Heroku (cloud hosting)

---

## 2. Component Design

### 2.1 Frontend Components

#### 2.1.1 App Component
- **Purpose:** Root component managing routing and global state
- **State:** User location
- **Routes:**
  - `/` → Landing Page
  - `/dashboard/*` → Dashboard with nested routes

#### 2.1.2 Landing Page
- **Purpose:** Project introduction and entry point
- **Features:**
  - Hero section with project overview
  - Feature highlights
  - Call-to-action button

#### 2.1.3 Dashboard Component
- **Purpose:** Main application interface with navigation
- **Sub-components:**
  - Sidebar navigation
  - Mobile menu
  - Location display
  - Content area
- **State:**
  - Sidebar open/closed
  - Country code
  - Active route

#### 2.1.4 Disease Detection Page
- **Purpose:** Plant disease identification interface
- **Features:**
  - Image upload via drag-and-drop
  - Webcam capture
  - Voice description (future)
  - Analysis results display
- **State:**
  - Uploaded image
  - Analysis status
  - Results data
  - Active input method

#### 2.1.5 Chat Page
- **Purpose:** AI voice assistant interface
- **Features:**
  - Text input/output
  - Voice input (speech recognition)
  - Voice output (text-to-speech)
  - Real-time streaming responses
  - WebSocket connection
- **State:**
  - Messages array
  - Input text
  - Connection status
  - Listening status

#### 2.1.6 Prediction Page
- **Purpose:** Market price forecasts
- **Features:**
  - Crop selector
  - Timeframe selector
  - Price comparison
  - Historical chart
  - AI recommendations
- **State:**
  - Selected crop
  - Selected timeframe
  - Country code

#### 2.1.7 Crops Page
- **Purpose:** Browse available crops
- **Features:** Display crop information by region

#### 2.1.8 Buyers Page
- **Purpose:** Connect with buyers
- **Features:** Marketplace functionality

### 2.2 Backend Components

#### 2.2.1 FastAPI Application (app.py)
- **Purpose:** Main application server
- **Endpoints:**
  - `POST /upload-image` - Upload plant images
  - `POST /disease-predict` - Predict disease from image
  - `POST /farm-assistant` - Streaming chat endpoint
  - `POST /api/bedrock-chat` - Bedrock chat endpoint
  - `WS /ws/voicechat` - WebSocket for voice chat
- **Middleware:**
  - CORS (allow all origins)

#### 2.2.2 Disease Detection Module (disease.py)
- **Purpose:** Disease information database
- **Data Structure:**
  ```python
  {
    'disease_key': {
      'name': str,
      'description': str,
      'symptoms': list,
      'treatment': list,
      'prevention': list
    }
  }
  ```

#### 2.2.3 Fertilizer Module (fertilizer.py)
- **Purpose:** Fertilizer recommendation database
- **Data Structure:**
  ```python
  {
    'NHigh': str,  # Advice for high nitrogen
    'Nlow': str,   # Advice for low nitrogen
    'PHigh': str,  # Advice for high phosphorus
    'Plow': str,   # Advice for low phosphorus
    'KHigh': str,  # Advice for high potassium
    'Klow': str    # Advice for low potassium
  }
  ```

#### 2.2.4 Model Module (model.py)
- **Purpose:** ResNet9 neural network architecture
- **Architecture:**
  - Conv Block 1: 3 → 64 channels
  - Conv Block 2: 64 → 128 channels + MaxPool
  - Residual Block 1: 128 → 128 channels
  - Conv Block 3: 128 → 256 channels + MaxPool
  - Conv Block 4: 256 → 512 channels + MaxPool
  - Residual Block 2: 512 → 512 channels
  - Classifier: MaxPool + Flatten + Linear

#### 2.2.5 LangChain Tools
- **get_crop_recommendation_tool:** Recommends crops based on soil/weather
- **get_fertilizer_recommendation_tool:** Suggests fertilizers
- **general_farming_chat:** Handles general queries

#### 2.2.6 AI Agent
- **Type:** Conversational React Description Agent
- **LLM:** Amazon Bedrock (Claude 3 Sonnet)
- **Memory:** ConversationBufferMemory
- **Tools:** Crop recommendation, fertilizer recommendation, general chat

---

## 3. Data Models

### 3.1 Disease Prediction Request
```json
{
  "image_path": "string"
}
```

### 3.2 Disease Prediction Response
```json
{
  "status": "success",
  "disease": "string",
  "disease_info": {
    "name": "string",
    "description": "string",
    "symptoms": ["string"],
    "treatment": ["string"],
    "prevention": ["string"]
  }
}
```

### 3.3 Crop Recommendation Input
```python
{
  "N": float,        # Nitrogen (0-140)
  "P": float,        # Phosphorus (5-145)
  "K": float,        # Potassium (5-205)
  "temperature": float,  # Celsius (8-45)
  "humidity": float,     # Percentage (10-100)
  "ph": float,          # pH level (3.5-9)
  "rainfall": float     # mm (20-300)
}
```

### 3.4 Chat Message (WebSocket)
```json
{
  "type": "asr_partial | asr_end | agent_text_partial | agent_text_final",
  "text": "string"
}
```

### 3.5 Bedrock Chat Request
```json
{
  "query": "string"
}
```

### 3.6 Bedrock Chat Response
```json
{
  "status": "success",
  "response": "string"
}
```

---

## 4. API Design

### 4.1 REST Endpoints

#### POST /upload-image
- **Purpose:** Upload plant image for analysis
- **Request:** multipart/form-data with file
- **Response:**
  ```json
  {
    "status": "success",
    "file_path": "uploads/{uuid}_{filename}"
  }
  ```

#### POST /disease-predict
- **Purpose:** Predict disease from uploaded image
- **Request:**
  ```json
  {
    "image_path": "string"
  }
  ```
- **Response:** Disease prediction with details

#### POST /farm-assistant
- **Purpose:** Streaming chat responses
- **Request:**
  ```json
  {
    "query": "string"
  }
  ```
- **Response:** text/plain stream

#### POST /api/bedrock-chat
- **Purpose:** Non-streaming chat with Bedrock
- **Request:**
  ```json
  {
    "query": "string"
  }
  ```
- **Response:** JSON with response text

### 4.2 WebSocket Protocol

#### WS /ws/voicechat
- **Purpose:** Real-time voice chat
- **Client → Server Messages:**
  - `{"type": "asr_partial", "text": "..."}`
  - `{"type": "asr_end", "text": "..."}`
- **Server → Client Messages:**
  - `{"type": "user_text_partial", "text": "..."}`
  - `{"type": "agent_text_partial", "text": "..."}`
  - `{"type": "agent_text_final", "text": "..."}`

---

## 5. Database Design

### 5.1 File-Based Storage

**Disease Database:**
- Format: Python dictionary in disease.py
- Keys: Disease identifiers
- Values: Disease information objects

**Fertilizer Database:**
- Format: CSV file (Data/fertilizer.csv)
- Columns: Crop, N, P, K

**Model Files:**
- `models/plant_disease_model.pth` - ResNet9 weights
- `models/RandomForest.pkl` - Crop recommendation model
- `models/DecisionTree.pkl` - Alternative classifier
- `models/NBClassifier.pkl` - Naive Bayes classifier
- `models/SVMClassifier.pkl` - SVM classifier
- `models/XGBoost.pkl` - XGBoost classifier

**Uploaded Images:**
- Directory: `backend/uploads/`
- Naming: `{uuid}_{original_filename}`

---

## 6. Security Design

### 6.1 Authentication & Authorization
- **Current:** No authentication (public access)
- **Future:** JWT-based authentication for user accounts

### 6.2 Input Validation
- File type validation (images only)
- File size limit (10MB)
- Input sanitization for text queries
- Path validation for image paths

### 6.3 CORS Policy
- **Current:** Allow all origins (development)
- **Production:** Restrict to specific domains

### 6.4 Environment Variables
- AWS credentials stored in environment
- API keys not hardcoded
- Sensitive configuration externalized

### 6.5 Data Privacy
- Uploaded images stored temporarily
- No personal data collection
- WebSocket sessions isolated by UUID

---

## 7. Performance Design

### 7.1 Optimization Strategies

**Frontend:**
- Code splitting with React.lazy
- Image optimization
- Debounced input handlers
- Memoized components

**Backend:**
- Async/await for I/O operations
- Model loaded once at startup
- Connection pooling for external APIs
- Streaming responses for chat

**AI/ML:**
- Pre-loaded models in memory
- Batch processing for multiple predictions
- GPU acceleration (if available)

### 7.2 Caching Strategy
- Weather data cached for 1 hour
- Model predictions cached by image hash
- Static assets cached by browser

### 7.3 Scalability Considerations
- Stateless API design
- Horizontal scaling via Docker
- Load balancing support
- CDN for static assets

---

## 8. Error Handling

### 8.1 Frontend Error Handling
- Try-catch blocks for async operations
- User-friendly error messages
- Fallback UI for failed components
- Retry logic for network failures

### 8.2 Backend Error Handling
- HTTP exception handlers
- Validation error responses
- Graceful degradation for external API failures
- Logging for debugging

### 8.3 Error Response Format
```json
{
  "status": "error",
  "message": "string",
  "detail": "string"
}
```

---

## 9. Deployment Design

### 9.1 Docker Configuration
- **Dockerfile:** Multi-stage build
- **docker-compose.yml:** Service orchestration
- **Ports:** 8000 (backend), 3000 (frontend)

### 9.2 Heroku Deployment
- **Procfile:** Web process definition
- **heroku.yml:** Container deployment
- **runtime.txt:** Python version specification
- **Environment:** AWS credentials, API keys

### 9.3 Environment Configuration
```bash
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=xxx
AWS_SECRET_ACCESS_KEY=xxx
BEDROCK_MODEL_ID=anthropic.claude-3-sonnet-20240229-v1:0
WEATHER_API=xxx
```

---

## 10. UI/UX Design

### 10.1 Design Principles
- **Simplicity:** Clean, intuitive interface
- **Responsiveness:** Mobile-first design
- **Accessibility:** WCAG guidelines
- **Feedback:** Visual indicators for all actions
- **Consistency:** Unified color scheme and typography

### 10.2 Color Palette
- Primary Green: Agricultural theme
- Secondary Brown: Earth tones
- Golden Yellow: Highlights
- Rustic Orange: Alerts
- White/Gray: Backgrounds

### 10.3 Layout Structure
- **Sidebar Navigation:** Fixed left panel
- **Main Content:** Scrollable center area
- **Mobile:** Hamburger menu with overlay
- **Cards:** Consistent card-based design

### 10.4 Interaction Patterns
- Drag-and-drop file uploads
- Click-to-capture photos
- Push-to-talk voice input
- Real-time streaming text
- Smooth page transitions

---

## 11. Testing Strategy

### 11.1 Unit Testing
- Component testing with Jest/React Testing Library
- Backend function testing with pytest
- Model inference testing

### 11.2 Integration Testing
- API endpoint testing
- WebSocket communication testing
- External API integration testing

### 11.3 End-to-End Testing
- User flow testing with Cypress/Playwright
- Cross-browser compatibility testing
- Mobile responsiveness testing

### 11.4 Performance Testing
- Load testing with Locust
- Response time monitoring
- Memory leak detection

---

## 12. Monitoring & Logging

### 12.1 Application Logging
- Request/response logging
- Error logging with stack traces
- Performance metrics logging

### 12.2 Monitoring Metrics
- API response times
- Error rates
- WebSocket connection count
- Model inference latency

### 12.3 Alerting
- Error rate thresholds
- Response time degradation
- Service availability

---

## 13. Future Architecture Considerations

### 13.1 Microservices Migration
- Separate disease detection service
- Dedicated chat service
- Independent model serving

### 13.2 Database Integration
- PostgreSQL for user data
- Redis for caching
- S3 for image storage

### 13.3 Advanced Features
- Real-time collaboration
- Offline-first architecture
- Progressive Web App (PWA)
- Native mobile apps

### 13.4 AI/ML Enhancements
- Model versioning and A/B testing
- Continuous learning pipeline
- Multi-model ensemble
- Custom model training interface
