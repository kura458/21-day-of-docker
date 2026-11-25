# Day 21 – Full-Stack Todo Application with Docker Compose

This folder contains a containerized Todo application used to practice a multi-service Docker deployment.

## Architecture

| Service | Technology | Purpose | Local address |
|---|---|---|---|
| `frontend` | React, TypeScript, Vite | Todo interface | http://localhost:3000 |
| `backend` | FastAPI, SQLAlchemy | REST API and application logic | http://localhost:8000 |
| `db` | PostgreSQL 15 | Persistent Todo storage | Internal Compose network |

The frontend uses Zustand for state management and Axios for API requests. The backend exposes Todo endpoints under `/todos` and a health endpoint at `/health`.

## Project structure

```text
21-full-application-deployment/
├── backend/
│   ├── app/
│   │   ├── routers/
│   │   ├── crud.py
│   │   ├── database.py
│   │   ├── main.py
│   │   ├── models.py
│   │   ├── schemas.py
│   │   └── services.py
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/
│   ├── src/
│   ├── Dockerfile
│   ├── package.json
│   └── vite.config.ts
├── .dockerignore
├── .gitignore
├── docker-compose.yml
└── README.md
```

## Requirements

- Docker Desktop with Docker Compose v2, or Docker Engine with the Compose plugin
- Git

## Run the application

Open a terminal in this directory:

```bash
cd 21-full-application-deployment
```

Create your local environment file.

**Windows CMD:**

```cmd
copy .env.example .env
```

**PowerShell, macOS, or Linux:**

```bash
cp .env.example .env
```

Edit `.env` and set a private local database password. The example value is only a placeholder for local learning; do not reuse it in a public or production deployment.

Build and start all services:

```bash
docker compose up --build -d
```

Check service status and logs:

```bash
docker compose ps
docker compose logs -f
```

Open the frontend at [http://localhost:3000](http://localhost:3000). The backend API documentation is available at [http://localhost:8000/docs](http://localhost:8000/docs), and its health endpoint is [http://localhost:8000/health](http://localhost:8000/health).

Stop the services:

```bash
docker compose down
```

The named `db_data` volume preserves PostgreSQL data when containers are stopped or recreated. To remove the database data as well, use `docker compose down -v` (this permanently deletes the local database volume).

## Configuration notes

- Compose reads the root `.env` file for PostgreSQL settings and the frontend API URL.
- The backend receives its database connection string from the Compose service environment and connects to the database using the service name `db`, not `localhost`.
- PostgreSQL has a health check. The backend waits until the database is ready, and the frontend waits until the backend health check succeeds.
- `VITE_API_BASE_URL` is embedded into the frontend at image build time. Rebuild the frontend image after changing it.
- The default API URL is intended for a local browser accessing the application on the same machine.

## Learning points

- Defining frontend, backend, and database services in Compose
- Building images from separate Dockerfiles
- Connecting services over a private Compose network
- Supplying configuration through environment variables
- Using health checks and service readiness conditions
- Persisting database data with a named volume
- Running the application containers as a non-root user where configured

## Scope and limitations

This is an educational deployment example, not a production-hardened service. Before production use, add stronger secret management, database backups, TLS termination, migrations, monitoring, and an appropriate deployment security review.

---

Part of the [21-day Docker learning journal](../README.md).
