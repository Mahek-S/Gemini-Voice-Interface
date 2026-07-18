# 🎙️ Gemini Voice Interface

A real-time multimodal voice interface built on **Google Vertex AI Gemini Live API (Native Audio)**. The application enables low-latency voice conversations through a browser using streaming audio, secure WebSocket communication, and OAuth-based authentication.

This project integrates Google's official Gemini Live Native Audio implementation with a custom Python backend, secure authentication layer, and browser interface to provide a production-oriented voice interaction system.

---

## Features

- 🎤 Real-time voice conversations with Gemini Live
- ⚡ Low-latency bidirectional audio streaming
- 🔊 Native audio responses
- 📝 Live input and output transcription
- 🌍 Multilingual conversations
- ❤️ Configurable affective dialog support
- ⛔ Natural interruption (barge-in)
- 🔒 Secure OAuth authentication using Google Cloud Application Default Credentials (ADC)
- 🌐 Browser-based interface
- 🧩 Support for Gemini function calling

---

## Architecture

```
Browser (Microphone + Speaker)
            │
            │ WebSocket
            ▼
      Custom Python Backend
   • Authentication
   • Configuration
   • WebSocket Proxy
            │
            │ OAuth Bearer Token
            ▼
 Vertex AI Gemini Live API
 gemini-live-2.5-flash-native-audio
```

Unlike browser-based API key integrations, authentication is handled entirely on the backend. The browser never receives Google Cloud credentials or access tokens.

---

## Project Structure

```
gemini-voice-interface/
├── server.py                        # Python backend and WebSocket proxy
├── requirements.txt                 # Python dependencies
├── .env                             # Local configuration (not committed)
└── frontend/
    ├── index.html                   # Web interface
    ├── script.js                    # Application logic
    ├── geminilive.js                # Gemini Live client
    ├── mediaUtils.js                # Audio utilities
    ├── tools.js                     # Function calling support
    └── audio-processors/
        ├── capture.worklet.js
        └── playback.worklet.js
```

---

## Prerequisites

- Python 3.9+
- Google Cloud account
- Vertex AI API enabled
- Google Cloud CLI (`gcloud`)

---

## Setup

### Clone the repository

```bash
git clone https://github.com/Mahek-S/gemini-voice-interface.git
cd gemini-voice-interface
```

### Install dependencies

```bash
pip install -r requirements.txt
```

### Authenticate with Google Cloud

```bash
gcloud auth application-default login
```

### Configure the project

Create a `.env` file:

```env
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_CLOUD_LOCATION=us-central1
```

### Enable Vertex AI

```bash
gcloud services enable aiplatform.googleapis.com --project=your-project-id
```

### Run the application

```bash
python server.py
```

Open:

```
http://localhost:8000
```

Click **Connect**, allow microphone access, and start speaking.

---

## Authentication

This project does **not** use API keys.

During local development, authentication is handled using **Google Cloud Application Default Credentials (ADC)**.

```bash
gcloud auth application-default login
```

The backend automatically generates short-lived OAuth access tokens using:

```python
google.auth.default()
```

The browser communicates only with the local Python backend. The backend authenticates with Vertex AI and securely proxies all communication between the browser and Gemini Live.

---

## How It Works

1. The browser captures microphone audio using the Web Audio API.
2. Audio is streamed over a WebSocket connection to the Python backend.
3. The backend authenticates with Vertex AI using Google Cloud OAuth credentials.
4. The backend establishes a secure WebSocket connection with Gemini Live.
5. Audio is streamed bidirectionally between the browser and Gemini Live through the backend proxy.
6. Gemini streams native audio responses back to the browser in real time.

---

## Model

| Property | Value |
|-----------|-------|
| Model | `gemini-live-2.5-flash-native-audio` |
| Platform | Google Vertex AI |
| Audio Input | PCM 16-bit, 16 kHz |
| Audio Output | PCM 16-bit, 24 kHz |
| Context Window | 128K tokens |
| Native Audio | ✅ |
| Affective Dialog | ✅ |
| Interruptions | ✅ |

---

## Key Technologies

### Native Audio

Unlike traditional voice assistants that follow a Speech-to-Text → LLM → Text-to-Speech pipeline, Gemini Live Native Audio processes streaming audio directly, enabling lower latency and more natural conversations.

### OAuth Authentication

Instead of exposing API keys in the browser, the backend authenticates using Google Cloud Application Default Credentials and generates short-lived OAuth access tokens for Vertex AI.

### WebSocket Proxy

A custom Python WebSocket proxy securely forwards audio and model responses between the browser and Gemini Live while keeping authentication entirely server-side.

### AudioWorklet Processing

Microphone capture and audio playback use the Web Audio API's AudioWorklet interface for low-latency, non-blocking audio processing.

---

## Configuration

The interface allows configuration of:

- Voice selection
- System instructions
- Input transcription
- Output transcription
- Affective dialog
- Proactive audio
- Temperature
- Activity detection

---

## Tech Stack

| Layer | Technology |
|---------|------------|
| Frontend | HTML, CSS, JavaScript |
| Audio | Web Audio API, AudioWorklet |
| Communication | WebSockets |
| Backend | Python, aiohttp, websockets |
| Authentication | Google Cloud Application Default Credentials (ADC) |
| AI Platform | Google Vertex AI |
| Model | Gemini Live 2.5 Flash Native Audio |

---

## Troubleshooting

### Microphone not detected

- Verify browser microphone permissions.
- Check operating system microphone permissions.

### Unable to connect

- Ensure `python server.py` is running.
- Authenticate using:

```bash
gcloud auth application-default login
```

- Verify that the Vertex AI API is enabled for your Google Cloud project.

### No audio output

- Click anywhere on the page before starting (required by modern browsers).
- Check your system output device and volume.

---

## Roadmap

- Conversation history
- Multiple assistant personalities
- Resume interview mode
- Memory and context management
- Retrieval-Augmented Generation (RAG)
- Production deployment
- Improved UI/UX

---

## Acknowledgements

This project builds upon Google's official **Gemini Live Native Audio** reference implementation while extending it with a custom backend integration, secure authentication flow, browser interface, and application-level functionality.

---

## References

- Gemini Live API Documentation: https://cloud.google.com/vertex-ai/generative-ai/docs/live-api/get-started-websocket
- Gemini Live Native Audio Model: https://cloud.google.com/vertex-ai/generative-ai/docs/models/gemini-live-2.5-flash-native-audio
- Google Cloud Vertex AI: https://cloud.google.com/vertex-ai
- Google Native Audio Reference Implementation: https://github.com/GoogleCloudPlatform/generative-ai/tree/main/gemini/multimodal-live-api/native-audio-websocket-demo-apps/plain-js-demo-app