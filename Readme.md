CSCI 578 Group Project - Fall 2025

# PocketLLM Portal

This project contains:
- `backend/`  (Node.js + Express + SSE + rate limiting + sessions + cache)
- `frontend/` (static SPA UI via Nginx)
- `docker-compose.yml`  (backend + frontend + Ollama)

## Quick Start

1. Install Docker + Docker Compose (v2).
2. From the project root, run:

   ```bash

   docker exec -it ollama ollama pull llama3.2 
   docker compose up -d --build
