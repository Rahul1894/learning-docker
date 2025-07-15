
# Docker Compose Deep Dive

A comprehensive guide to understanding, installing, configuring, and using Docker Compose for multi‑container applications. This single-file README covers the theory, file structure, essential commands, and detailed explanations.

---

## 📖 Table of Contents

1. [What Is Docker Compose?](#what-is-docker-compose)  
2. [Why Use Docker Compose?](#why-use-docker-compose)  
3. [Core Concepts & Terminology](#core-concepts--terminology)  
4. [Project Structure](#project-structure)  
5. [docker-compose.yml Breakdown](#docker-composeyml-breakdown)  
6. [Common Commands & Explanations](#common-commands--explanations)  
7. [Advanced Features](#advanced-features)  
8. [Best Practices](#best-practices)  
9. [Troubleshooting Tips](#troubleshooting-tips)

---

## What Is Docker Compose?

Docker Compose is a CLI tool for defining and running **multi‑container Docker applications**. You describe your application’s services, networks, and volumes in a single `docker-compose.yml` file, then spin everything up (or down) in one command.

---

## Why Use Docker Compose?

- **Simplicity**: Orchestrate multiple services with one file and a few commands.  
- **Reproducibility**: Every developer or CI environment uses the exact same configuration.  
- **Isolation & Modularity**: Each service runs in its own container, isolating dependencies.  
- **Declarative Infrastructure**: “Infrastructure as code”—version‑control your deployment setup.  

---

## Core Concepts & Terminology

| Term            | Definition                                                                 |
|-----------------|-----------------------------------------------------------------------------|
| **Service**     | A container definition. E.g., `web`, `db`, `cache`.                         |
| **Image**       | Read‑only template used to build containers.                                |
| **Container**   | A running instance of an image.                                             |
| **Volume**      | Persistent filesystem storage managed by Docker.                            |
| **Network**     | Virtual network connecting containers.                                      |
| **Depends_on**  | Specifies start‑up order between services.                                  |
| **Environment** | Environment variables passed into a container.                              |

---

## Project Structure

```

myproject/
├── app.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
└── .env

````

- **app.py**: Example Flask (or your framework) entry point.  
- **requirements.txt**: Python dependencies.  
- **Dockerfile**: Builds the `web` service image.  
- **docker-compose.yml**: Compose definition for all services.  
- **.env**: Environment variables (optional; not committed).

---

## docker-compose.yml Breakdown

```yaml
version: "3.9"                   # (Optional) Compose file format version
services:                        # Define all your containers here
  web:                           # 1) Service name
    build: .                     #    • Build image from local Dockerfile
    image: myapp_web:latest      #    • (Optional) Tag to assign
    ports:                       # 2) Port mapping
      - "5000:5000"              #      hostPort:containerPort
    environment:                 # 3) Env vars
      - FLASK_ENV=development
      - REDIS_URL=redis://redis:6379
    depends_on:                  # 4) Startup order
      - redis
    volumes:                     # 5) Mount local code for live reload
      - .:/usr/src/app
    networks:                    # 6) Attach to custom network
      - frontend

  redis:
    image: "redis:alpine"        # Use official Redis image
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data         # Persist data between restarts
    networks:
      - frontend

volumes:                         # Named volumes
  redis-data:

networks:                        # Custom networks for service isolation
  frontend:
````

---

## Common Commands & Explanations

| Command                             | Explanation                                                                           |
| ----------------------------------- | ------------------------------------------------------------------------------------- |
| `docker compose up`                 | Build (if needed) & start all services; attaches to logs.                             |
| `docker compose up -d`              | Run in **detached** (background) mode; containers keep running after terminal closes. |
| `docker compose down`               | Stop and remove containers, networks, and default volumes.                            |
| `docker compose stop`               | Stop services without removing containers.                                            |
| `docker compose start`              | Start services that were previously stopped.                                          |
| `docker compose restart`            | Stop & start services—useful after config changes.                                    |
| `docker compose build`              | Build or rebuild service images (honors `build:` keys).                               |
| `docker compose ps`                 | List running containers and status for each service.                                  |
| `docker compose logs`               | View logs across all services (or specify service: `logs web`).                       |
| `docker compose exec <svc> <cmd>`   | Run a command inside a running service (e.g., `exec web sh` to drop into shell).      |
| `docker compose run --rm <svc> CMD` | Run a one‑off command in a new container and remove it after exit.                    |
| `docker compose pull`               | Pull service images from registry without starting containers.                        |
| `docker compose config`             | Validate & view the fully expanded Compose file (merges `.env`, interpolations).      |

---

## Advanced Features

* **`healthcheck:`**
  Monitor service health and control start‑up order more robustly.

  ```yaml
  healthcheck:
    test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
    interval: 30s
    retries: 3
  ```

* **`profiles:`**
  Group optional services. E.g., only start `redis` in `dev` profile.

  ```yaml
  profiles: ["dev"]
  ```

* **`secrets:`**
  Securely manage sensitive data (passwords, keys) without baking them into images.

* **`extends:`**
  Reuse or inherit settings from another Compose file or service block.

---

## Best Practices

1. **Keep services small & focused**: One process per container.
2. **Use named volumes** for persistent data.
3. **Avoid `latest` tags** in production—pin to specific versions.
4. **Leverage `.env`** for environment‑specific configuration.
5. **Healthchecks** ensure orchestrator knows if a container is truly ready.
6. **Version control** your Compose file alongside application code.

---

## Troubleshooting Tips

* **“Cannot connect to Docker daemon”**

  * Ensure Docker Engine/Daemon is running.
  * On Windows: Start Docker Desktop; verify WSL2 backend if using Linux containers.

* **Obsolete `version:` warning**

  * Remove the `version:` line; modern Compose infers latest schema.

* **Networking issues between containers**

  * Ensure services share the same network block.
  * Use service names (DNS) to reference containers (e.g., `redis:6379`).

* **Permission errors on mounted volumes**

  * Check host folder permissions; consider using Docker-managed volumes instead.

---

> **Congratulations!** You now have a complete, theory‑backed, command‑rich guide to Docker Compose.
> Feel free to fork, adapt, and extend this README for your own projects!

```
```
