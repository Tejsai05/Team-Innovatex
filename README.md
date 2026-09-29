# Legal Research Memory Agent

A small, synthetic legal research prototype. It searches five demo authorities, records matter events in SQLite, and uses Hindsight Cloud for cross-session memory. All authorities are fictional and this is not legal advice.

## Run

Python 3.9+ is enough; there are no package dependencies.

```sh
cp .env.example .env
# Set HINDSIGHT_API_KEY and HINDSIGHT_BANK_ID in .env
set -a
source .env
set +a
python3 server.py
```

Open <http://localhost:8000>. Create or select a Hindsight memory bank in Hindsight Cloud and set its ID as `HINDSIGHT_BANK_ID`. The API key stays on the backend. If Hindsight is not configured or unavailable, memory operations return an explicit error and the app does not pretend research was retained.

Configuration:

- `HINDSIGHT_API_KEY`: server-side Cloud API key.
- `HINDSIGHT_BASE_URL`: defaults to `https://api.hindsight.vectorize.io`.
- `HINDSIGHT_BANK_ID`: existing bank used for this prototype.
- `PORT`: defaults to `8000`.
- `LEGAL_RESEARCH_DB`: optional SQLite path; defaults to `research.sqlite3` beside the app.

## Five-minute demo

1. Create a matter named `ABC vs XYZ` with “Research whether Section X applies to Contract Y.”
2. Click **Start research**. The deterministic first pass retrieves DEMO CASE A and DEMO CASE B, then retains the question, authority status, and decision in Hindsight.
3. Click **What have we established?** or **Continue** to recall the earlier research.
4. Click **Introduce DEMO CASE C** to retain a contrary interpretation as a new development.
5. Click **What have we established?** to inspect recalled memories, then **Current position** to use Hindsight Reflect across them.

Matters and their visible timeline live in SQLite across restarts. Hindsight is authoritative for durable research memories; every research, development, recall, and reflect action requires the configured Hindsight service. Starting another browser session and selecting the same matter recalls the same bank memories.

## API

`POST /api/matters`, `POST /api/research/start`, `POST /api/research/continue`, `POST /api/research/development`, `POST /api/memory/recall`, `POST /api/memory/reflect`, and `GET /api/matters/{id}`. `GET /api/status` reports whether Hindsight credentials are configured.

## Limits

The authorities are synthetic; retrieval is keyword based; there is no legal corpus, authentication, or LLM-based summary generation outside Hindsight Reflect. A single configured memory bank is shared across matters, so use this prototype with demo content only. Live Retain, Recall, and Reflect require valid Hindsight credentials and an existing bank.
# Team-Innovatex
