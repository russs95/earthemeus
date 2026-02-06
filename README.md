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
