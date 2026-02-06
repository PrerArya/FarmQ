# 🌾 AI Farming Assistant

An AI-powered web application to help farmers detect plant diseases, get personalized fertilizer suggestions, and receive live voice assistance for farming queries.

![demo-banner](docs/demo-preview.png)

---

## 🚀 Features

- 📸 **Plant Disease Detection** – Upload or capture plant images to detect common diseases using deep learning.
- 🌿 **Fertilizer Recommendation** – Suggests ideal fertilizer based on soil and crop details.
- 🎤 **Live Voice Assistant** – Talk to an AI agent using voice (WebSocket-based real-time chat).
- 📍 **Location-Based Tips** – Leverages your location for localized weather/crop suggestions.

---

## 🧑‍💻 Tech Stack

| Layer       | Tech                                     |
|------------|------------------------------------------|
| Frontend   | React.js, Lucide Icons                   |
| Backend    | FastAPI, WebSockets, Python              |
| AI Runtime | Amazon Bedrock (Claude 3 Sonnet default) |
| Dev Assist | Amazon Q (developer productivity)        |
| Models     | ResNet9 (PyTorch), ML Classifiers        |
| Extras     | React Webcam, Dropzone, TTS              |

---

## 📦 Installation Guide

### 1. Clone the repository

```bash
git clone https://github.com/Abhishek-Jain-12072000/FarmQ.git
cd FarmQ
```

### 2. Backend Setup (FastAPI)

```bash
# Create a virtual environment
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows

# Install dependencies
pip install -r requirements.txt

# Export AWS creds and model for Amazon Bedrock
export AWS_REGION=us-east-1
export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...
export BEDROCK_MODEL_ID=anthropic.claude-3-sonnet-20240229-v1:0

# Run the server
uvicorn app:app --reload

```

🔗 API: http://localhost:8000

### 3. Frontend Setup (React)

```bash
cd frontend
npm install     # or yarn install
npm start       # or yarn start

```

### 4. Voice Chat (WebSocket)
    - Connects to: ws://localhost:8000/ws/voicechat
    - Requires microphone permission for live audio input
    - Streams user speech and bot replies in real-time


🧪 API Reference

| Endpoint           | Method | Description                         |
| ------------------ | ------ | ----------------------------------- |
| `/upload-image`    | POST   | Upload plant image for prediction   |
| `/disease-predict` | POST   | Predict disease from uploaded image |
| `/fertilizer`      | POST   | Get fertilizer suggestion           |
| `/ws/voicechat`    | WS     | WebSocket endpoint for live chat    |

🤖 Models Used

Voice Chat: Llama-2-7b integration with context-based response (LangChain)

Disease Detection: ResNet9 trained on plant disease dataset

Fertilizer Prediction: Scikit-learn classifier trained on crop-soil data






