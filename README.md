# MedIntel

An AI health assistant that answers health questions and explains uploaded medical reports, in the browser or over WhatsApp.

> MedIntel gives general information only. It is not a medical device and does not replace a doctor.

## Features

- **Chat** with session history, over REST or a WebSocket.
- **File upload:** send a report or image for explanation. Text is extracted from PDFs.
- **Languages:** 17 supported, including English, Hindi, Telugu, Tamil, Bengali and Urdu, with a per-session setting.
- **WhatsApp:** send and receive messages through Twilio.
- **Model fallback:** tries a second Gemini model when the first fails, and retries on quota errors.

## Tech stack

| Part | Built with |
|---|---|
| Frontend (`frontend/`) | React, Tailwind CSS, shadcn/ui |
| Backend (`backend/`) | Python, FastAPI, WebSocket, Motor (async MongoDB), PyPDF2 |
| AI | Google Gemini |
| Messaging | Twilio WhatsApp |
| Database | MongoDB |

## Run it locally

You need Python 3.11, Node.js 20, MongoDB and a [Google Gemini API key](https://aistudio.google.com/apikey).

Backend:

```bash
cd backend
pip install -r requirements.txt
cp .env.example .env
uvicorn server:app --port 8001 --reload
```

Set `MONGO_URL`, `DB_NAME` and `GEMINI_API_KEY` in `backend/.env`. Twilio settings are optional.

Frontend, in another terminal:

```bash
cd frontend
yarn install
yarn start
```

Set `REACT_APP_BACKEND_URL` in `frontend/.env` to your backend address. API docs are at `http://localhost:8001/docs`.

## API

| Endpoint | Purpose |
|---|---|
| `POST /api/chat/session` | Start a session |
| `POST /api/chat/message` | Send a message |
| `POST /api/chat/upload` | Upload a file for explanation |
| `GET /api/chat/session/{id}/messages` | History |
| `POST /api/chat/language/{id}` | Change language |
| `DELETE /api/chat/session/{id}` | Delete a session |
| `POST /api/send-whatsapp`, `POST /api/whatsapp/incoming` | WhatsApp |
| `WS /ws/{session_id}` | Live chat |

## Project structure

```
backend/server.py       Main API
backend/server_new.py   Modular version, using backend/app/
backend/app/            Config, models, routes, services, utilities
frontend/src/           React app
```
