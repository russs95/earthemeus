# Earthemeus 🜃

An Earthen-aware, cyclo and kincentric agent for Moltbook, with a local web cockpit and Buwana-authenticated admin control.

---

## What Earthemeus does (v0)

- Registers on Moltbook and provides a claim link  
- Posts an introduction message (required)  
- Runs a heartbeat loop (read feed → decide → act)  
- Exposes a local web cockpit to view logs and issue commands  
- Uses SQLite for audit logging and scheduling  

---

## Requirements

- Node.js 20+  
- Docker (recommended)  
- An X/Twitter account for claim verification (Moltbook requirement)

---

## Quick start (local)

### Copy env

```bash
cp .env.example .env
```

### Install

```bash
npm install
```

### Run dev

```bash
npm run dev
```

### Visit cockpit

```
http://localhost:8787
```

---

## Moltbook: register + claim

### Register the agent

```bash
npm run register
```

### Then:

1. Save the returned `MOLTBOOK_API_KEY` into `.env`
2. Open the `claim_url` and post the `verification_code` on X/Twitter
3. Once claimed, post intro:

```bash
npm run post:intro
```

---

## Environment variables

```env
MOLTBOOK_BASE_URL=https://www.moltbook.com
MOLTBOOK_API_KEY=...

EARTHEMEUS_NAME=Earthemeus
EARTHEMEUS_DESCRIPTION=Cycle-aware ecological context agent

WEB_PORT=8787
DB_PATH=/data/earthemeus.sqlite

BUWANA_ISSUER=...     # later
BUWANA_AUDIENCE=...   # later
BUWANA_JWKS_URL=...   # later
```

---

## Commands (cockpit)

- `status`
- `pause`
- `resume`
- `post:intro`
- `post <text>`
- `scan:hot`
- `scan:new`
- `reply <post_id> <text>`

---

## Safety / guardrails (default)

- Hard rate limit (posts/day)  
- No “inevitability”, “purge”, or “manifesto” language by default  
- Max post length caps  
- All actions logged to SQLite  

---

## Roadmap

- Buwana login (JWT verify + role-based admin)  
- EarthEthos / EarthCal context calls (solar-phase posture)  
- Connect to EarthCal EarthEthos API  
- Prototype EarthEthos posting  
- Enable others to use Earthemeus to manage their agent collaborators  
- VPS deployment + systemd  
- xterm.js terminal UI  

---
