# Dental Clinic Voice AI Receptionist

This project is a **voice-based AI agent** built for a dental clinic using Livekit with telephone integeration. The agent handles real phone calls, talks to patients naturally, and manages appointments end‑to‑end — including checking availability, booking, rescheduling, and canceling appointments.

I built this project to understand how **voice AI systems work in production**, not just as demos. It combines real‑time speech, LLM reasoning, and an actual backend (Google Calendar) that a clinic could realistically use.

---

## What this agent can do

* Answer incoming calls as a polite dental clinic receptionist
* Book dental appointments after checking doctor availability
* Suggest alternative time slots when a doctor is busy
* Cancel existing appointments using appointment IDs
* Share information about available doctors and services
* Enforce clinic rules (working hours, future dates only, no Sundays)

The agent behaves like a real receptionist — it confirms details with the caller before creating or canceling calendar events.

---

## Tech stack

* **Python** 3.9.13
* **Docker**
* **LiveKit Agents** – real‑time voice/media agent framework
* **OpenAI models**

  * GPT‑4o-mini (LLM reasoning)
  * GPT‑4o‑transcribe (speech‑to‑text)
  * tts-1 (text‑to‑speech)
* **Silero VAD** – voice activity detection
* **Google Calendar API** – appointment storage and availability checks
* **LiveKit Cloud Project** – deployment and telephony integration
* **LiveKit CLI** installed (brew install livekit-cli or equivalent).

---

## High‑level architecture

1. Caller dials the clinic number (telephony via LiveKit Cloud)
2. Speech is transcribed in real time (STT)
3. The LLM decides what to do next based on instructions and tools
4. Calendar tools are called when needed (availability, booking, canceling)
5. Responses are converted back to speech (TTS)
6. Appointments are stored directly in Google Calendar

---

## Running locally

1. Clone the repository
2. Create and activate a virtual environment
3. Install dependencies(requirenments.txt)
4. Add `.env`, create a livekit cloud project and generate api keys for livekit cloud nd openai.
5. for google calendar api,authenticate nd add your client_secret.json nd token.json(auto-generated).
6. purchase a livekit number for telephone integeration and configure the dispatch rules for that number(1 free number per id).
   -Add the agents name, destination room nd rule type for communication.
8. Run the agent: you can either install livekit cli or use python agent.py start in ur project terminal.

```
python agent.py start
```

You can then test the agent using the **LiveKit Playground**.

-Connect to playground with LiveKit Cloud or manually with a URL(cloud) and token.
-In the description section before connecting, add the agent's name and room name before connecting.

---

## Deployment

This agent is deployed on **LiveKit Cloud**.

Key deployment points:

* Secrets (OAuth token) are mounted securely
* The same codebase works locally and in the cloud
* Once deployed, the agent runs continuously and answers real calls nd books appointments.

---
## Testing

**A live demo number is available upon request.**
**Please contact me if you’d like to test the agent.**


---------------------------------------

