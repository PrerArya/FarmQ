# Requirements Document - AI Farming Assistant

## 1. Project Overview

**Project Name:** AI Farming Assistant (FarmQ)

**Purpose:** An AI-powered web application designed to help farmers detect plant diseases, receive personalized fertilizer recommendations, get crop suggestions, and access live voice assistance for farming queries.

**Target Users:** Farmers, agricultural professionals, and farming enthusiasts worldwide

---

## 2. Functional Requirements

### 2.1 Plant Disease Detection

**FR-1.1:** The system shall allow users to upload plant images in JPG, PNG, or GIF format (max 10MB)

**FR-1.2:** The system shall support real-time image capture via webcam

**FR-1.3:** The system shall analyze uploaded/captured images using a ResNet9 deep learning model

**FR-1.4:** The system shall identify plant diseases from a predefined set of disease classes

**FR-1.5:** The system shall provide disease information including:
- Disease name and description
- Symptoms
- Treatment recommendations
- Prevention measures
- Confidence score (percentage)

**FR-1.6:** The system shall support voice description of plant symptoms (future enhancement)

### 2.2 Crop Recommendation

**FR-2.1:** The system shall recommend optimal crops based on:
- Nitrogen (N) content in soil
- Phosphorus (P) content in soil
- Potassium (K) content in soil
- Soil pH level
- Temperature
- Humidity
- Rainfall

**FR-2.2:** The system shall use a Random Forest classifier for crop predictions

**FR-2.3:** The system shall fetch real-time weather data based on city location

**FR-2.4:** The system shall handle missing or invalid input parameters with default values

### 2.3 Fertilizer Recommendation

**FR-3.1:** The system shall recommend fertilizers based on:
- Crop type
- Current N, P, K levels in soil
- Target N, P, K levels for the crop

**FR-3.2:** The system shall calculate nutrient deficiencies/excesses

**FR-3.3:** The system shall provide specific fertilizer advice for:
- High/Low Nitrogen
- High/Low Phosphorus
- High/Low Potassium

**FR-3.4:** The system shall support fertilizer recommendations for crops listed in the fertilizer database

### 2.4 AI Voice Assistant

**FR-4.1:** The system shall provide a WebSocket-based real-time chat interface

**FR-4.2:** The system shall support text-based queries via REST API

**FR-4.3:** The system shall integrate with Amazon Bedrock (Claude 3 Sonnet) for natural language processing

**FR-4.4:** The system shall support voice input via browser speech recognition

**FR-4.5:** The system shall provide voice output via text-to-speech synthesis

**FR-4.6:** The system shall maintain conversation context using memory buffers

**FR-4.7:** The system shall support queries about:
- Crop recommendations
- Fertilizer suggestions
- General farming advice

**FR-4.8:** The system shall stream responses in real-time for better user experience

### 2.5 Market Price Predictions

**FR-5.1:** The system shall display price predictions for major crops (wheat, rice, corn)

**FR-5.2:** The system shall show:
- Current market price
- Predicted future price
- Price trend (up/down)
- Confidence level
- Historical price data (6 months)
- Key market factors

**FR-5.3:** The system shall support multiple timeframes (1 month, 3 months, 6 months, 1 year)

**FR-5.4:** The system shall provide location-based pricing in local currency

**FR-5.5:** The system shall offer AI-powered recommendations based on price trends

### 2.6 Location Services

**FR-6.1:** The system shall detect user location via browser geolocation API

**FR-6.2:** The system shall support IP-based location detection as fallback

**FR-6.3:** The system shall display prices in local currency based on country

**FR-6.4:** The system shall show country name in the dashboard

### 2.7 User Interface

**FR-7.1:** The system shall provide a responsive web interface for desktop and mobile

**FR-7.2:** The system shall include a landing page with project overview

**FR-7.3:** The system shall provide a dashboard with navigation to:
- Home
- Crops page
- Buyers page
- Chat/Voice assistant
- Disease detection
- Market predictions

**FR-7.4:** The system shall support mobile menu with overlay

**FR-7.5:** The system shall provide visual feedback for loading states

---

## 3. Non-Functional Requirements

### 3.1 Performance

**NFR-1.1:** Disease detection shall complete within 5 seconds

**NFR-1.2:** Chat responses shall stream within 2 seconds of query submission

**NFR-1.3:** WebSocket connections shall maintain real-time communication with <100ms latency

**NFR-1.4:** The system shall support at least 100 concurrent users

### 3.2 Reliability

**NFR-2.1:** The system shall have 99% uptime

**NFR-2.2:** The system shall handle API failures gracefully with error messages

**NFR-2.3:** The system shall maintain WebSocket reconnection logic

### 3.3 Usability

**NFR-3.1:** The interface shall be intuitive and require no training

**NFR-3.2:** The system shall provide clear error messages

**NFR-3.3:** The system shall support drag-and-drop file uploads

**NFR-3.4:** The system shall provide visual feedback for all user actions

### 3.4 Security

**NFR-4.1:** The system shall validate all file uploads for type and size

**NFR-4.2:** The system shall sanitize user inputs to prevent injection attacks

**NFR-4.3:** The system shall use HTTPS for production deployment

**NFR-4.4:** The system shall secure AWS credentials via environment variables

### 3.5 Scalability

**NFR-5.1:** The system shall support horizontal scaling via containerization

**NFR-5.2:** The system shall use stateless API design for easy scaling

**NFR-5.3:** The system shall support deployment on cloud platforms (Heroku, AWS)

### 3.6 Compatibility

**NFR-6.1:** The frontend shall support modern browsers (Chrome, Firefox, Safari, Edge)

**NFR-6.2:** The system shall support speech recognition in compatible browsers

**NFR-6.3:** The system shall provide fallback options when browser features are unavailable

### 3.7 Maintainability

**NFR-7.1:** The code shall follow modular architecture

**NFR-7.2:** The system shall use standard frameworks (React, FastAPI)

**NFR-7.3:** The system shall include clear separation of concerns (frontend/backend)

---

## 4. Technical Requirements

### 4.1 Backend Requirements

**TR-1.1:** Python 3.8+

**TR-1.2:** FastAPI framework for REST API and WebSocket support

**TR-1.3:** PyTorch for deep learning model inference

**TR-1.4:** Scikit-learn for ML classifiers

**TR-1.5:** LangChain for AI agent orchestration

**TR-1.6:** Amazon Bedrock SDK for LLM integration

**TR-1.7:** Pandas for data processing

### 4.2 Frontend Requirements

**TR-2.1:** React.js 18+

**TR-2.2:** React Router for navigation

**TR-2.3:** Lucide React for icons

**TR-2.4:** React Dropzone for file uploads

**TR-2.5:** React Webcam for camera capture

**TR-2.6:** WebSocket API for real-time communication

**TR-2.7:** Web Speech API for voice features

### 4.3 AI/ML Requirements

**TR-3.1:** Pre-trained ResNet9 model for disease classification

**TR-3.2:** Random Forest model for crop recommendation

**TR-3.3:** Amazon Bedrock with Claude 3 Sonnet model

**TR-3.4:** LangChain agents with conversational memory

### 4.4 Data Requirements

**TR-4.1:** Disease classification dataset with 38+ disease classes

**TR-4.2:** Fertilizer recommendation database (CSV format)

**TR-4.3:** Disease information database with symptoms, treatments, and prevention

**TR-4.4:** Crop-soil-weather correlation data

### 4.5 Infrastructure Requirements

**TR-5.1:** Docker support for containerization

**TR-5.2:** Heroku deployment configuration

**TR-5.3:** AWS credentials for Bedrock access

**TR-5.4:** OpenWeatherMap API key for weather data

---

## 5. External Dependencies

### 5.1 Third-Party APIs

**DEP-1.1:** Amazon Bedrock API (Claude 3 Sonnet)

**DEP-1.2:** OpenWeatherMap API

**DEP-1.3:** IP Geolocation API (ipapi.co)

### 5.2 External Libraries

**DEP-2.1:** PyTorch

**DEP-2.2:** Scikit-learn

**DEP-2.3:** LangChain

**DEP-2.4:** FastAPI

**DEP-2.5:** React.js

**DEP-2.6:** React Dropzone

**DEP-2.7:** React Webcam

---

## 6. Constraints

**C-1:** The system requires AWS credentials for Bedrock integration

**C-2:** Disease detection is limited to pre-trained disease classes

**C-3:** Weather data requires valid city names

**C-4:** Voice features require browser support for Web Speech API

**C-5:** Real-time chat requires WebSocket support

**C-6:** Image uploads are limited to 10MB

---

## 7. Assumptions

**A-1:** Users have internet connectivity

**A-2:** Users grant camera and microphone permissions when needed

**A-3:** AWS Bedrock service is available and accessible

**A-4:** OpenWeatherMap API is operational

**A-5:** Users have modern web browsers with JavaScript enabled

---

## 8. Future Enhancements

**FE-1:** Multi-language support for international users

**FE-2:** Mobile native applications (iOS/Android)

**FE-3:** Offline mode for disease detection

**FE-4:** User authentication and profile management

**FE-5:** Historical query tracking and analytics

**FE-6:** Integration with IoT sensors for real-time soil monitoring

**FE-7:** Community forum for farmers

**FE-8:** Expert consultation booking system

**FE-9:** Marketplace for buying/selling crops

**FE-10:** Advanced analytics dashboard with yield predictions

**FE-11:** Integration with government agricultural databases

**FE-12:** Pest detection and management recommendations
