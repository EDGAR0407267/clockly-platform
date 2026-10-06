# ClockLy Platform

A full stack time tracking platform for businesses, with an employee portal, an administrator dashboard, a protected kiosk, and a REST API.

[Detailed Spanish guide](README.es.md) · [API contract](docs/contracts/api_v1.md) · [Production checklist](docs/PRODUCTION_CHECKLIST.md)

## Overview

ClockLy is a SaaS product in development for managing attendance and related business workflows. This repository contains the Next.js web app and the FastAPI backend. It does not contain a native mobile app.

## Features

- Company registration, onboarding, role based access, and employee self service.
- Clock in/out sessions, location events, analytics, exports, and incident tickets.
- Protected kiosk with backend PIN validation and rate limiting.
- Cash closure and salary estimate workflows.
- Plan limits and Stripe billing integration in the backend.

These surfaces have different production readiness; see the [current flow and limitations](README.es.md#superficies-visibles-hoy) before deploying.

## Tech Stack

Next.js 16, React 19, TypeScript, Tailwind CSS, FastAPI, Python, SQLAlchemy, Alembic, PostgreSQL, Redis, Docker Compose, and Stripe.

## Architecture

`frontend-next/` is the web frontend; `backend_v2/app/` is the API and business logic; `backend_v2/alembic/` holds database migrations. The backend issues HttpOnly session cookies, and the frontend hydrates the session through `GET /auth/me`. Authorization and tenant checks belong in the backend.

## Getting Started

### Prerequisites

Python, Node.js, npm, and Docker with Compose for local PostgreSQL and Redis.

### Backend

```powershell
git clone https://github.com/EDGAR0407267/clockly-platform.git
cd clockly-platform
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
docker compose up -d postgres redis
cd backend_v2
Copy-Item .env.example .env
# Fill in the required local values in .env before starting the API.
alembic upgrade head
python main.py --host 127.0.0.1 --port 8010 --reload
```

The API documentation is available at `http://127.0.0.1:8010/docs`.

### Frontend

In another PowerShell terminal:

```powershell
cd clockly-platform/frontend-next
Copy-Item .env.example .env.local
# Set NEXT_PUBLIC_API_URL to the local API address in .env.local.
npm ci
npm run dev
```

Open `http://127.0.0.1:3000`. The example files contain variable names only. Backend configuration includes database, session, storage, email, and billing settings; integrations need their own credentials when enabled. Never commit populated environment files.

## Project Structure

| Path | Purpose |
| --- | --- |
| `frontend-next/` | Web application and browser tests |
| `backend_v2/app/` | API, authentication, and business services |
| `backend_v2/alembic/` | Database migrations |
| `backend_v2/tests/` | Backend tests |
| `docs/` | API, security, deployment, and product notes |
| `docker-compose.yml` | Local PostgreSQL and Redis services |

## Status

**In Development.** Core flows are implemented, with staging and production checks documented in the [project guide](README.es.md#bloqueadores-mvp-y-staging).

## Author

Edgar Pedret Girones · [GitHub](https://github.com/EDGAR0407267)
