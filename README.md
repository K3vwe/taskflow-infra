# 🧱 Full-Stack Task Tracker – Infrastructure

This repository contains the **Docker-based infrastructure layer** for running the full-stack Task Tracker application in both development and production environments.

---

## 📦 Architecture Overview

This project follows a **multi-repository structure**:

* **Frontend (Next.js)** – UI layer
* **Backend (FastAPI)** – API + business logic
* **Infrastructure (this repo)** – orchestration using Docker Compose

### Why this structure?

* Separation of concerns
* Independent development & deployment
* Scalable to microservices architecture
* Mirrors real-world production systems

---

## 🔗 Required Repositories

Clone all required repositories before running the system:

* Frontend: GitHub → https://github.com/K3vwe/taskflow-web
* Backend: GitHub → https://github.com/K3vwe/taskflow-api
* Infrastructure: (this repo)

---

## 📁 Expected Folder Structure

All repositories must exist in the same parent directory:

```
workspace/
  frontend/
  backend/
  infra/
```

> Docker Compose relies on relative paths like `../frontend` and `../backend`.

---

## ⚙️ Environment Setup

Create a `.env` file inside the `infra/` directory:

```
POSTGRES_DB=taskdb
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_PORT=5432
NEXT_PUBLIC_API_URL=http://localhost:8000
FRONTEND_HOST/BACKEND_CORS_ORIGINS=http://localhost:3000
NODE_ENV/ENVIRONMENT=development

```

You can also copy from:

```
.env.example
```
## 🚨 Runtime Rules

* Missing required variables → application must fail immediately
* `POSTGRES_SERVER=db` is required for Docker networking
* `NEXT_PUBLIC_API_URL` is exposed to the browser
* `SECRET_KEY` must be changed in production

---

## 🐳 Development Setup

### Start the full stack (with hot reload)

```bash
cd infra/docker
docker compose -f docker-compose.dev.yml up --build
```

### 🧩 Services

* Frontend → http://localhost:3000
* Backend → http://localhost:8000
* Database → PostgreSQL (internal)

---

## 🔄 Volumes (Development Only)

Development uses bind mounts for fast iteration:

* `../frontend:/app` → live reload for Next.js
* `../backend:/app` → live reload for FastAPI
* `/app/node_modules` → prevents dependency conflicts

---

## 🏗️ Production Setup

Production uses optimized builds (no volumes):

```bash
docker compose -f docker-compose.prod.yml up --build -d
```

### Key Differences

| Feature     | Dev                | Prod              |
| ----------- | ------------------ | ----------------- |
| Volumes     | ✅ Yes (hot reload) | ❌ No              |
| Build       | Fast, iterative    | Optimized, cached |
| Performance | Lower              | Higher            |
| Use case    | Local development  | Deployment-ready  |

---

## 🔌 Networking

All services communicate via Docker’s internal network:

* `frontend → backend` via `http://backend:8000`
* `backend → db` via `postgresql://db:5432`

No manual network configuration required for development.

---

## 🚀 Common Commands

### Rebuild containers

```bash
docker compose up --build
```

### Force clean rebuild (no cache)

```bash
docker compose build --no-cache
docker compose up
```

### Stop services

```bash
docker compose down
```

---

## 🧠 Design Decisions

* **Docker Compose** used for local orchestration
* **Service-based architecture** for scalability
* **Environment variables** for configuration management
* **Separate repos** to simulate real-world team workflows

---

## 📈 Future Improvements

* CI/CD pipeline (GitHub Actions)
* Deployment to AWS (ECS / EKS)
* Reverse proxy (NGINX)
* HTTPS with Let's Encrypt
* Monitoring (Prometheus + Grafana)

---

## 👨‍💻 Goal

This project is designed to demonstrate:

* Real-world DevOps workflow
* Containerization best practices
* Full-stack system integration
* Production-ready architecture

---

## ⚠️ Notes

* Ensure Docker is installed and running
* Ports `3000` and `8000` must be available
* Do not use production config for development

---

## 🏁 Quick Start

```bash
git clone https://github.com/K3vwe/taskflow-web.git
git clone https://github.com/K3vwe/taskflow-api.git
git clone https://github.com/K3vwe/taskflow-infra.git

cd infra
docker compose -f docker-compose.dev.yml up --build
```

---

This setup is designed to be **simple to run, easy to scale, and aligned with industry practices**.
