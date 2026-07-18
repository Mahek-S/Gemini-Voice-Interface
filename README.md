# 🎙️ Gemini Voice Interface

A real-time multimodal voice assistant built on **Google Vertex AI Gemini Live API (Native Audio)**. The application enables low-latency voice conversations through a browser using streaming audio, WebSockets, and OAuth-based authentication without exposing API keys.

> This project uses Google's official Native Audio reference implementation as the communication layer and extends it with a custom application backend, configuration management, and web interface.

---

## Features

- 🎤 Real-time voice conversations with Gemini Live
- ⚡ Low-latency bidirectional audio streaming
- 🔊 Native Audio responses
- 🗣️ Voice Activity Detection (VAD)
- 📝 Optional input/output transcription
- 🌐 Browser-based interface
- 🔒 Secure OAuth authentication using Google Cloud Application Default Credentials (ADC)
- 🔁 Python WebSocket proxy for secure communication
- 🎥 Camera and screen sharing support
- 🧩 Function calling support
- 🌍 Google Grounding support

---

## Architecture

```
Browser
      │
      │ WebSocket
      ▼
Python WebSocket Proxy
(server.py)
      │
      │ OAuth Bearer Token
      ▼
Vertex AI Gemini Live API
(Native Audio)
```

The browser never communicates directly with Vertex AI.

The backend authenticates using Google Cloud credentials and securely proxies all WebSocket communication between the client and Gemini Live.

---

## Tech Stack

### Backend

- Python
- aiohttp
- websockets
- google-auth
- python-dotenv

### Frontend

- HTML
- CSS
- JavaScript

### Google Cloud

- Vertex AI Gemini Live API
- Google Cloud OAuth (Application Default Credentials)

---

## Project Structure

```
.
├── frontend/
│   ├── index.html
│   ├── script.js
│   ├── geminilive.js
│   ├── audio.js
│   ├── video.js
│   └── ...
│
├── server.py
├── requirements.txt
├── .env
└── README.md
```

---

## Authentication

This project does **not** use API keys.

Authentication is performed using **Google Cloud Application Default Credentials (ADC)**.

During development:

```bash
gcloud auth application-default login
```

The backend automatically generates short-lived OAuth access tokens using:

```python
google.auth.default()
```

These tokens are used to authenticate requests to Vertex AI.

---

## Installation

### Clone the repository

```bash
git clone https://github.com/<username>/gemini-voice-interface.git
cd gemini-voice-interface
```

### Install dependencies

```bash
pip install -r requirements.txt
```

### Authenticate

```bash
gcloud auth application-default login
```

### Configure

Create a `.env` file.

```env
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_CLOUD_LOCATION=us-central1
```

---

## Run

```bash
python server.py
```

Open:

```
http://localhost:8000
```

---

## How it Works

1. The browser establishes a WebSocket connection with the local Python server.
2. The backend obtains an OAuth access token using Google Cloud credentials.
3. The backend creates a secure WebSocket connection with Vertex AI Gemini Live.
4. Audio, video, and text messages are streamed bidirectionally between the browser and Gemini Live through the backend proxy.
5. Responses are streamed back in real time using Gemini Native Audio.

---

## Current Capabilities

- Real-time speech-to-speech interaction
- Audio streaming
- Voice interruption handling
- Input/output transcription
- Camera streaming
- Screen sharing
- Function calling
- Grounding support

---

## Future Work

- Conversation history
- Multiple assistant personalities
- Resume interview mode
- Meeting assistant
- Memory and context management
- Retrieval-Augmented Generation (RAG)
- Production deployment
- Custom UI/UX improvements

---

## Acknowledgements

This project is built on top of Google's official **Gemini Live Native Audio** reference implementation while extending it with custom backend integration, configuration management, and application-level functionality.

Google Cloud Vertex AI Documentation:
https://cloud.google.com/vertex-ai

Gemini Live API:
https://ai.google.dev/gemini-api/docs/live