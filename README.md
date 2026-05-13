# Hospital Flow Backend

Hospital Flow Backend is a full-stack hospital operations dashboard for patient placement, room readiness, patient messaging, and real-time safety alerts. It combines a React/Vite frontend with an Express/MongoDB backend, explainable bed-placement logic, and an LLM-assisted patient support workflow.

## Features

- Patient, admission, and message workflows backed by Express and MongoDB
- Real-time operational alerts through an in-memory event bus
- Explainable bed placement using hard constraints, weighted scoring, and A* pathfinding
- Patient monitor view with local computer-vision signal processing and relay-agent alerts
- LLM-assisted patient chat using bounded medical-record context and async embedding warmup
- Safety guardrails for urgent symptoms, non-diagnostic CV alerts, and minimal PHI exposure
- Unit, room, task, admission, patient, and admin dashboards

## Tech Stack

- Frontend: React, TypeScript, Vite, Tailwind CSS
- Backend: Node.js, Express, MongoDB
- AI/ML: Dedalus chat/embedding API, MediaPipe Tasks Vision, ElevenLabs text-to-speech
- Testing: Vitest, React Testing Library
- Tooling: ESLint, TypeScript

## Getting Started

~~~bash
npm install
cd server && npm install && cd ..
cp .env.example .env
~~~

For local development, set:

~~~bash
VITE_API_BASE=http://localhost:5050
~~~

Create `server/config.env`:

~~~bash
ATLAS_URI=your_mongodb_connection_string
PORT=5050
DEDALUS_API_KEY=your_dedalus_api_key
ELEVENLABS_API_KEY=your_elevenlabs_api_key
~~~

Run the backend:

~~~bash
npm run server
~~~

Run the frontend:

~~~bash
npm run dev
~~~

Open `http://localhost:5173`.

## Scripts

~~~bash
npm run dev       # start Vite frontend
npm run server    # start Express backend
npm run build     # build production frontend
npm run test      # run Vitest tests
npm run lint      # run ESLint
~~~

## Key Workflows

### Patient and Admissions

Patients can be created, assigned to rooms/beds, listed with pagination, and linked to admission records. Admission queue endpoints deduplicate by patient and keep the latest admission state visible to the dashboard.

### Bed Placement

The recommendation engine filters beds by availability, isolation, oxygen, and ventilator constraints. It scores feasible beds by room type, equipment fit, acuity, mobility risk, and A* travel cost.

### Patient Chat

The patient assistant uses medical-record notes, document summaries, vitals, and recent messages as bounded context. Stable record chunks are hashed and embedded asynchronously so chat stays responsive.

### Patient Monitoring

The monitor processes derived CV metrics locally and emits non-diagnostic alerts. The relay agent supports deterministic rules or a Dedalus-backed LLM decision path with cooldowns, retry backoff, and evidence-first messages.

## Testing

~~~bash
npm run test
~~~

Current coverage focuses on bed recommendation, A* pathfinding, CV alert rules, relay-agent behavior, and frontend rendering.

## Safety Notes

This project is an MVP and is not a medical device. CV alerts and LLM responses are assistive only, non-diagnostic, and require human verification. Webcam processing is local in-browser; video frames are not stored or uploaded.

## Roadmap

- Replace mock room data with fully persisted facility models
- Add authenticated roles, audit logs, and encryption for production PHI handling
- Move all LLM calls behind backend-only service boundaries
- Add WebSocket or SSE streams for multi-client live updates
- Add integration tests for API routes and database workflows
