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
* **LiveKit Agents SDK** – real‑time voice/media agent framework
* **OpenAI models**

  * GPT‑4o-mini (LLM reasoning)
  * GPT‑4o‑transcribe (speech‑to‑text)
  * tts-1 (text‑to‑speech)
* **Silero VAD** – voice activity detection
* **Google Calendar API** – appointment storage and availability checks
* **LiveKit Cloud Project** – deployment and telephony integration(free tier works).
* **LiveKit CLI** installed (brew/winget install livekit.LivekitCLI or equivalent).

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
6. Go to the LiveKit Dashboard > Telephony.Purchase a number and create a Dispatch Rule.
  Set the Rule Type to Individual and set the Agent Name to match your local agent's name.
8. Run the agent: you can either install livekit cli or use python agent.py start in ur project terminal.

```
python agent.py start
```
## Testing

**Option A: Testing via Web**

You can test the agent using the **LiveKit Playground**.

-Connect to playground with LiveKit Cloud or manually with a URL(cloud) and token.
-In the Agent/Room Name field, enter the name defined in your code (e.g., dental-receptionist).

**Option B: Testing via Phone (SIP)**

Call the number. Your local terminal will show logs as soon as you speak.
---

## Deployment

To move from local testing to a permanent cloud-hosted agent, deploy to LiveKit Cloud using the LiveKit CLI managed build workflow.

workflow:
1.via livekit cli, autherize to livekit cloud project.
>lk cloud auth
2.Before deploying, you must register the agent and "mount" your API keys as secure environment variables. This ensures your keys are never stored in the code.
initialize your agent,navigate to your project directory containing your agent.py or main.py and run
>lk agent create \
  --name dental-receptionist \
  --secrets OPENAI_API_KEY=sk-xxxx \
  --secrets GOOGLE_CALENDAR_ID=your-email@gmail.com \
  --secrets LIVEKIT_API_KEY=your-api-key \
  --secrets LIVEKIT_API_SECRET=your-api-secret
(this will generate a livekit.toml and docker file and start the build process.
3.Once the agent is registered, use the deploy command to build the Docker image and push it to LiveKit’s global infrastructure:
>lk agent deploy
4. Verify Deployment
You can check if your agent is live and see how many "Worker" processes are ready to answer calls:
>lk agent status

Key deployment points:

* Deployment uses LiveKit’s native agent workflow:

The LiveKit CLI builds the agent container automatically

Secrets are provided via secure mounts

The agent is deployed and managed by LiveKit Cloud

Workers are started on demand for LiveKit sessions and telephony calls

 The same codebase works locally and in the cloud


---
## Testing

**A live demo number is available upon request.**
**Please contact me if you’d like to test the agent.**


---------------------------------------

