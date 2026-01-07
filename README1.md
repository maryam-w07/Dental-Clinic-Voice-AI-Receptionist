Dental Clinic Voice AI Receptionist

This project is a voice-based AI agent built for a dental clinic using LiveKit with telephony integration. The agent handles real phone calls, talks to patients naturally, and manages appointments end-to-end — including checking availability, booking, rescheduling, and canceling appointments.

This project was built to understand how voice AI systems work in production, not just as demos. It combines real-time speech processing, LLM reasoning, and a real backend (Google Calendar) that a clinic could realistically use.

What This Agent Can Do

Answer incoming calls as a polite dental clinic receptionist

Book dental appointments after checking doctor availability

Suggest alternative time slots when a doctor is busy

Cancel existing appointments using appointment IDs

Share information about available doctors and services

Enforce clinic rules:

Working hours only

Future dates only

No appointments on Sundays

The agent behaves like a real receptionist by confirming details with the caller before creating or canceling calendar events.

Tech Stack

Python 3.9.13

Docker

LiveKit Agents SDK – real-time voice and media agent framework

OpenAI Models

GPT-4o-mini (LLM reasoning)

GPT-4o-transcribe (speech-to-text)

tts-1 (text-to-speech)

Silero VAD – voice activity detection

Google Calendar API – appointment storage and availability checks

LiveKit Cloud Project – deployment and telephony integration (free tier works)

LiveKit CLI – installed via brew, winget, or equivalent

High-Level Architecture

Caller dials the clinic phone number (telephony via LiveKit Cloud)

Speech is transcribed in real time (STT)

The LLM decides what to do next based on instructions and tools

Calendar tools are invoked when needed (availability, booking, canceling)

Responses are converted back to speech (TTS)

Appointments are stored directly in Google Calendar

Running Locally

Clone the repository

Create and activate a virtual environment

Install dependencies from requirements.txt

Create a .env file:

Create a LiveKit Cloud project

Generate API keys for LiveKit Cloud and OpenAI

Set up Google Calendar API:

Authenticate

Add client_secret.json

Generate token.json (auto-generated during auth)

In the LiveKit Dashboard:

Go to Telephony

Purchase a phone number

Create a Dispatch Rule

Rule Type: Individual

Agent Name: must match your local agent name

Run the agent locally:

python agent.py start

Testing
Option A: Testing via Web

You can test the agent using the LiveKit Playground.

Connect using LiveKit Cloud credentials or manually with a Cloud URL and token

In the Agent / Room Name field, enter the agent name defined in your code
(e.g., dental-receptionist)

Option B: Testing via Phone (SIP)

Call the purchased phone number

Your local terminal will display logs as soon as you speak

Deployment

To move from local testing to a permanent cloud-hosted agent, deploy to LiveKit Cloud using the LiveKit CLI managed build workflow.

Deployment Workflow
1. Authenticate with LiveKit Cloud
lk cloud auth

2. Register the Agent and Securely Mount Secrets

Before deploying, register the agent and mount API keys as secure environment variables.
This ensures sensitive keys are never stored in code.

Navigate to your project directory (containing agent.py or main.py) and run:

lk agent create \
  --name dental-receptionist \
  --secrets OPENAI_API_KEY=sk-xxxx \
  --secrets GOOGLE_CALENDAR_ID=your-email@gmail.com \
  --secrets LIVEKIT_API_KEY=your-api-key \
  --secrets LIVEKIT_API_SECRET=your-api-secret


This command:

Generates livekit.toml

Generates a Dockerfile

Automatically builds the agent image

3. Deploy the Agent

Once registered, deploy the agent to LiveKit Cloud:

lk agent deploy


This builds the Docker image and pushes it to LiveKit’s global infrastructure.

4. Verify Deployment

Check whether the agent is live and how many worker processes are running:

lk agent status

Key Deployment points:

Deployment uses LiveKit’s native agent workflow

The LiveKit CLI builds the container automatically

Secrets are injected via secure mounts

The agent is fully managed by LiveKit Cloud

Worker processes scale on demand for:

LiveKit sessions

Telephony calls

The same codebase works locally and in the cloud
