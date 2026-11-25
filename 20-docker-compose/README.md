# 20-docker-compose

## Day 20 Docker Compose

---

### Learning Objectives (Summary)

By the end of this day, you will understand:

1. **What Docker Compose Is** – A tool for managing multi-container applications  
2. **Why Docker Compose Is Needed** – Simplifies complex container setups  
3. **When to Use Compose** – Fullstack and multi-service applications  
4. **How Docker Compose Works** – YAML-based configuration and workflow  
5. **Dependencies & Networking** – How services communicate and depend on each other  
6. **Troubleshooting** – Debugging common Docker Compose issues  

---

## What is Docker Compose?

Docker Compose is a **tool that allows you to define and run multiple Docker containers using a single configuration file (`compose.yaml`)**.

- It replaces multiple `docker run` commands  
- Defines services, networks, and volumes in one place  
- Starts everything with a single command  

👉 Think of it as:  
**“Docker for full applications, not just containers”**

---

## Why Docker Compose is Needed

Managing containers manually becomes difficult:

- Multiple services require multiple commands  
- Hard to maintain and reproduce  
- No central configuration  

Docker Compose solves this by:

- Providing a **single source of truth (YAML file)**  
- Automating container startup and networking  
- Making projects **portable and reusable**  

---

## Use Cases of Docker Compose

Docker Compose is used for:

- Fullstack applications (Frontend + Backend + Database)  
- Microservices architecture  
- Local development environments  
- Testing and CI/CD pipelines  
- Small production deployments  

---

## When to Use Docker Compose

Use Docker Compose when:

- You have **more than one container**  
- Services need to **communicate**  
- You want a **repeatable setup**  
- You are building real-world systems like:
  - API + Database  
  - Web + Backend + Cache  

---

## How Docker Compose Works (Workflow)

### Step 1: Define Services

Create a `compose.yaml` file:

```yaml
services:
  app:
    build: .
    ports:
      - "8000:8000"

  db:
    image: postgres:latest

```
---

## Dependencies in Docker Compose

Docker Compose allows you to define **service dependencies** using `depends_on`.

This ensures that containers start in the correct order.

---

### Example

```yaml
services:
  app:
    build: .
    depends_on:
      - db

  db:
    image: postgres
```
---

## How It Works

- `db` starts before `app`  
- Docker Compose controls startup order  
- Services communicate using service names (e.g., `db`)  

---

## Important Notes

- `depends_on` does **NOT** guarantee that the service is ready  
- It only ensures the container is **started**, not fully initialized  

---

## Real-World Practice

- Add **retry logic** in your app  
- Use **wait-for scripts**  
- Use **health checks** for reliability  

---

## Troubleshooting Docker Compose

### 1. Container Not Starting

```bash
docker compose logs
```

`
## Docker Compose Commands Summary

| Command                            | Description                |
| ---------------------------------- | -------------------------- |
| `docker compose up`                | Start all services         |
| `docker compose up -d`             | Run in background          |
| `docker compose down`              | Stop and remove everything |
| `docker compose build`             | Build images               |
| `docker compose ps`                | Show running containers    |
| `docker compose ls`                | List projects              |
| `docker compose logs`              | View logs                  |
| `docker compose exec <service> sh` | Access container           |
| `docker compose restart`           | Restart services           |
| `docker compose pull`              | Pull images                |
| `docker compose push`              | Push images                |
| `docker compose config`            | Validate YAML              |



---

[⬅ Previous: Day 19 Multistage Builds](../19-multistage-builds/) | [Next ➡ Day 21 full-stack deployement](../21-full-application-deployment/)


## Run the FastAPI Compose example

The Compose file loads variables from `web_app/.env`. The real `.env` file is intentionally not tracked.

From the `20-docker-compose/web_app` directory, create your local environment file:

**Windows CMD:**
```cmd
copy .env.example .env
```

**PowerShell, macOS, or Linux:**
```bash
cp .env.example .env
```

Edit `.env` and replace `SECRET_KEY` with a private random value before using the application beyond a local demonstration. Keep `.env` private and never commit it.

Then, from the same directory, start the service:

```bash
docker compose up --build
```

Open [http://localhost:8000](http://localhost:8000) to check the API. Stop the service with `docker compose down`.
