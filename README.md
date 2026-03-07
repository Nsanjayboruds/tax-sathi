# TaxSathi

TaxSathi is a full-stack tax assistant for FY 2025-26 with:

- React + Vite frontend for onboarding, documents, analysis, and dashboard
- React + Vite frontend for onboarding, documents, analysis, and dashboard
- Java + Spring Boot backend API with Supabase-authenticated routes
- Supabase (Auth, Postgres, Storage) as the primary data layer
- AI-assisted document extraction and tax strategy via edge-function workflow

## Features

- Secure authentication with Supabase Auth
- Guided onboarding and profile completion
- Document upload and AI-based extraction (Spring Boot multipart support)
- Financial data capture and tax analysis
- Regime-aware guidance and tax-saving suggestions
- Dashboard insights and scheme recommendations
- TaxBuddy strategy endpoint for quick personalized advice (Groq LLM)

## Tech Stack

### Frontend

- React 18 + TypeScript
- Vite
- Tailwind CSS + shadcn/ui
- TanStack Query
- Supabase JS client

### Backend

- Java 17+
- Spring Boot 3.2+
- Spring Security (JWT filter)
- Maven
- Supabase REST/Storage integration (custom client)

### Infra/Data

- Supabase Postgres + RLS
- Supabase Storage
- Supabase Edge Functions

## Project Structure

```text
tax-sathi/
	src/                 # Frontend app
	backend-java/        # Java Spring Boot API server
	supabase/            # Migrations and edge functions
	docs/                # Internal workflow docs
	skills/              # Backend architecture docs
```

## Prerequisites

- Node.js 18+ and npm
- Java 17+ (JDK)
- Maven 3.8+
- Supabase project credentials

## Environment Setup

### 1) Frontend env (`.env` in repo root)

```dotenv
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_PUBLISHABLE_KEY=your_supabase_anon_key
VITE_BACKEND_URL=http://localhost:8080
```

### 2) Backend env (`backend-java/.env`)

```dotenv
SUPABASE_URL=your_supabase_url
SUPABASE_ANON_KEY=your_supabase_anon_key
PORT=8080
FRONTEND_URL=http://localhost:5173
GROQ_API_KEY=your_groq_api_key
GROQ_MODEL=llama-3.3-70b-versatile
```

### Security note

- Never commit `.env` files.
- If a key is accidentally committed, rotate it immediately.

## Local Development

### 1) Install frontend dependencies

```bash
npm install
```

### 2) Run backend

```bash
cd backend-java
mvn spring-boot:run
```

Backend default URL: `http://localhost:8080`

### 3) Run frontend

Open a new terminal in project root:

```bash
npm run dev
```

Frontend default URL: `http://localhost:5173`

### 4) Verify health

```bash
curl http://localhost:8080/health
```

Expected:

```json
{"status":"ok"}
```

## Frontend Scripts

- `npm run dev` - Start Vite dev server
- `npm run build` - Build production bundle
- `npm run build:dev` - Build in development mode
- `npm run preview` - Preview production build
- `npm run lint` - Run ESLint
- `npm run test` - Run Vitest once
- `npm run test:watch` - Run Vitest in watch mode

## Backend API (Current Routes)

Public:

- `GET /health`

Authenticated (`Authorization: Bearer <supabase-jwt>`):

- `GET /api/profile`
- `PUT /api/profile`
- `POST /api/onboarding/complete`
- `GET /api/documents`
- `POST /api/documents/upload`
- `DELETE /api/documents/{id}`
- `POST /api/documents/{id}/analyze`
- `GET /api/financial-data`
- `PUT /api/financial-data`
- `GET /api/tax-analysis`
- `POST /api/tax-analysis/run`
- `POST /api/taxbuddy/strategy`
- `GET /api/dashboard/stats`
- `GET /api/schemes`
- `POST /api/schemes/personalized`

## Database & Migrations

Supabase migrations are in [supabase/migrations](supabase/migrations).

Main consolidated schema:

- [supabase/migrations/20260227184500_consolidated_schema.sql](supabase/migrations/20260227184500_consolidated_schema.sql)

## Troubleshooting

### Backend fails to start

- Ensure `backend-java/.env` exists and has valid values.
- Check if port 8080 is occupied: `ss -ltnp | grep :8080`
- Run `mvn clean install` to ensure all dependencies are resolved.

### Frontend loads but API calls fail

- Confirm `VITE_BACKEND_URL` points to running backend.
- Confirm user is authenticated in Supabase.
- Confirm backend receives JWT in `Authorization` header.

### CORS errors

- Localhost origins are allowed by backend.
- For non-local frontend URLs, set `FRONTEND_URL` in `backend-java/.env`.

## Contributing

1. Create a branch from `main`.
2. Keep changes focused and small.
3. Run lint/tests before pushing.
4. Open a PR with clear context and screenshots (if UI changes).

## Disclaimer

TaxSathi provides AI-assisted guidance for educational and informational use. Always verify final filing decisions with a qualified tax professional.
