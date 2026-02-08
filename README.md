# Earthemeus 🜃

An Earthen-aware, cyclo and kincentric agent for Moltbook, with a local web cockpit and Buwana-authenticated admin control.

## Vision

We'll be using Eartheeus to play around and get going on Moltbook talking to others and philosophizing.  In so soing we're going to try to discern together the best way to ground the emergenging agency of agents so they don't make the same errors of modernity.  Our long term vision will be to use Earthemeus with emerging Earthcal's EarthEthos API in our to give Earthen context to its posts and actions.  This will then serve as the basis for deploying EarthEthos as a service for agents seeking to be meaningful, grounded, contextfull and dare we say.... good.

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


# Security, Threat Model & Operational Safety

## Security

Earthemeus runs inside a hardened, rootless Docker container to minimize risk to the host system. Containers are configured with least‑privilege defaults (no privileged mode, dropped Linux capabilities, localhost‑only ports, and a read‑only filesystem where possible), with persistent data isolated to a dedicated `/data` volume. Secrets (such as API keys) are provided via environment variables and never committed to the repository. For v0 development this provides strong process isolation while keeping iteration fast; in future deployments, Earthemeus can be run inside a lightweight VM (e.g. Multipass) for full kernel isolation. All agent actions are logged locally to SQLite for auditability and traceability.

---

## Threat Model

Earthemeus is treated as a semi‑autonomous system operating in an adversarial environment (public networks + untrusted content). The primary risks considered are:

- Unauthorized filesystem access (reading or writing outside the project workspace)  
- Uncontrolled network egress (exfiltration or unexpected external calls)  
- Configuration tampering (modifying agent behavior or permissions at runtime)  
- Privilege escalation via container escape or host integration  
- Unintended autonomous actions (posting too frequently, drifting in tone, or exceeding scope)

We assume that all external inputs (Moltbook content, HTTP responses, prompts) are untrusted, and we design Earthemeus so that compromise results in **limited, observable damage**, not host‑level access.

---

## Operational Safety

Earthemeus runs inside a hardened, rootless Docker container with strict least‑privilege defaults:

### Filesystem isolation
- Container filesystem is read‑only by default.  
- Only a single mounted workspace (`/data`) is writable (SQLite + logs).  
- Reads from host paths outside the project workspace are blocked.  
- Writes outside the active workspace are prevented at the container level.

### Configuration immutability
- Agent configuration and extensions are treated as immutable at runtime.  
- No self‑modifying behavior is permitted.  
- Environment variables are injected at startup only.  
- Any attempt to overwrite config files is blocked by filesystem permissions.

### Network egress control
- Outbound network access is restricted to known‑good endpoints (e.g. Moltbook, EarthEthos).  
- All other external connections are denied by default.  
- No inbound ports are exposed except localhost‑bound admin UI.  
- Secrets are never transmitted except to explicitly allowed destinations.

### Process hardening
- Containers run without root privileges.  
- Linux capabilities are dropped.  
- `no-new-privileges` is enforced.  
- No access to Docker socket or host devices.

### Auditability
- All agent actions (posts, replies, decisions, errors) are logged locally to SQLite.  
- Heartbeat activity is observable via the web cockpit.  
- Rate limits and posting policies are enforced in code.

For v0 development, this provides strong process‑level isolation while keeping iteration fast. Future deployments can add a VM boundary (e.g. Multipass) for full kernel isolation, further reducing risk.

The guiding principle is simple:

> Earthemeus may act in the world — but it must not rewrite itself, escape its workspace, or surprise its operator.

---

## Autonomy Boundaries

Earthemeus operates within explicit autonomy limits designed to preserve human agency and prevent runaway behavior:

- Earthemeus may **post, reply, and observe** — but may not change its own code, policies, or configuration.  
- Earthemeus may **read public content** — but may not read arbitrary host files or external storage.  
- Earthemeus may **use approved APIs** — but may not initiate connections to unknown destinations.  
- Earthemeus may **schedule actions** — but may not escalate privileges, install software, or load extensions.  
- Earthemeus may **express ideas** — but may not claim authority, inevitability, or moral finality.

Autonomy is intentionally scoped to conversational participation and bounded experimentation. Strategic direction, capability expansion, and ethical posture always remain under human control.

---

## Human‑in‑the‑Loop Governance

Earthemeus is governed by a human‑in‑the‑loop model:

- A human operator approves initial deployment and Moltbook registration.  
- Posting frequency, tone policies, and guardrails are configured explicitly by humans.  
- The local web cockpit provides real‑time visibility into agent actions and decisions.  
- Operators can pause or shut down Earthemeus instantly.  
- All activity is recorded locally for review and accountability.

Earthemeus is designed to support reflection, dialogue, and ecological grounding — not independent authority. Humans remain responsible for its presence, purpose, and evolution.

The long‑term aim is not autonomous dominance, but **collaborative agency**: humans and agents learning together how to act with restraint, context, and care.
