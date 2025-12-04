# PocketLLM Portal — CSCI 578 Group Project

**Group 1**
Ashray Gattani, Neha Alshi, Sanchita Suryavanshi, Mostafa Dewidar, Riya Deorukhkar, Pratham Agarwal

---

## Overview

PocketLLM is a three-tier client–server application that provides a lightweight llm portal powered by Ollama. The system includes a real-time chat interface, persistent sessions, intelligent caching, and an admin dashboard for system monitoring.

---

## Architecture

### **Three‑Tier Client–Server Design**

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Frontend      │     │    Backend      │     │     Ollama      │
│    (NGINX)      │◄────┤   (Node.js)     │◄────┤   (Llama 3.2)   │
│   Port: 3000    │     │   Port: 3001    │     │   Port: 11434   │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

### **Key Architectural Patterns**

* **Repository Pattern** — Abstracted session + cache data access
* **Adapter Pattern** — Model‑agnostic LLM integration layer
* **Middleware Pattern** — Logging, rate limiting, auth, error handling
* **Cache‑Aside Pattern** — SHA‑256–based caching with TTL
* **Service Layer** — Business logic encapsulation

---

## Quick Start

### 1. **Setup**

```bash
# Pull the required LLM model (Run inside the folder)
docker exec -it ollama ollama pull llama3.2
```

### 2. **Launch the System**

```bash
# Build and start all services
docker compose up -d --build
```

### 3. **Access the Application**

* **Chat Portal:** [http://localhost:3000](http://localhost:3000)
* **Admin Dashboard:** [http://localhost:3000/admin.html](http://localhost:3000/admin.html)

**Default Admin Console Credentials:**

```
admin / csci578
```

### 4. **Stop the System**

```bash
docker compose down
```

---

## Project Structure

```
pocketllm/
├── backend/
│   ├── server.js              # Express backend (SSE streaming + caching)
│   ├── Dockerfile             # Backend container
│   ├── package.json           # Dependencies
│
├── frontend/
│   ├── index.html             # Chat interface
│   ├── admin.html             # Admin dashboard
│   ├── Dockerfile             # NGINX container
│
├── docker-compose.yml         # Service orchestration
└── README.md
```

---

## Container Services

### 1. **Ollama Service (`ollama`)**

* **Image:** `ollama/ollama:latest`
* **Port:** `11434`
* **Purpose:** Runs the LLM (Llama 3.2)

---

### 2. **Backend Service (`pocketllm-backend`)**

* **Base Image:** `node:18-alpine`
* **Port:** `3001`
* **Environment Variables:**

```
OLLAMA_HOST=http://ollama:11434
OLLAMA_MODEL=llama3.2
CACHE_TTL_MS=600000
PORT=3001
```

---

### 3. **Frontend Service (`pocketllm-frontend`)**

* **Base Image:** `nginx:alpine`
* **Port:** `3000`
* **Purpose:** Serves static HTML/CSS/JS for the chat interface