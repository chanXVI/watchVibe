# Django + PostgreSQL-in-Docker MVP Bootstrap

## Summary

Build the backend as a **local Django app** that connects to **PostgreSQL running in Docker Compose**. Keep auth thin and web-first with **session auth**, then move directly into the domain MVP: Rolex references, personal collection, and ownership timeline.

This keeps development fast:
- Django runs locally in `.venv`
- PostgreSQL is isolated in Docker
- the app stays easy to containerize later if needed

## Implementation Changes

### 1. Project bootstrap and environment
- Create a Django project with Django REST Framework and `psycopg`.
- Add `.env`-driven settings for:
  - `POSTGRES_DB`
  - `POSTGRES_USER`
  - `POSTGRES_PASSWORD`
  - `POSTGRES_HOST`
  - `POSTGRES_PORT`
  - `DJANGO_SECRET_KEY`
  - `DJANGO_DEBUG`
  - `DJANGO_ALLOWED_HOSTS`
- Add `docker-compose.yml` with a single `postgres` service and a named volume for persistence.
- Use PostgreSQL as the default database in Django settings; do not keep SQLite as the main path.
- Add a concise `README` bootstrap flow for:
  - starting Postgres
  - installing Python deps
  - running migrations
  - seeding starter data
  - starting Django locally

### 2. App structure and auth
- Create these Django apps:
  - `accounts`
  - `references`
  - `collection`
  - `timeline`
- Start with a **custom user model** using email as the login identity.
- Implement only the initial auth API needed for the MVP:
  - register
  - login
  - logout
  - current user (`me`)
- Use Django session auth for browser-first usage.
- Leave password reset, WebAuthn/biometrics, JWT, and social auth out of scope for this first pass.

### 3. Domain MVP
- `references`:
  - `Brand`
  - `ModelFamily`
  - `Reference`
  - seed 10-20 starter Rolex references
  - public read endpoints for browsing/searching references
- `collection`:
  - `WatchInstance` linked to user and reference
  - status values: `owned`, `sold`, `wishlist`, `researching`
  - authenticated CRUD endpoints for a user’s collection
- `timeline`:
  - `Event` linked to `WatchInstance`
  - authenticated create/list flow for watch history events
- Keep collection data user-owned and separate from the shared Rolex reference catalog.

### 4. Initial API surface
- `POST /api/auth/register/`
- `POST /api/auth/login/`
- `POST /api/auth/logout/`
- `GET /api/auth/me/`
- `GET /api/references/`
- `GET /api/references/<id>/`
- `GET /api/collection/watches/`
- `POST /api/collection/watches/`
- `GET /api/collection/watches/<id>/`
- `PATCH /api/collection/watches/<id>/`
- `DELETE /api/collection/watches/<id>/`
- `GET /api/collection/watches/<id>/events/`
- `POST /api/collection/watches/<id>/events/`

## Test Plan

- Confirm Django connects successfully to the Dockerized PostgreSQL instance.
- Verify migrations run cleanly on a fresh database.
- Verify seed command loads starter Rolex references without duplicates.
- Auth tests:
  - register creates a user and session
  - login succeeds/fails correctly
  - unauthenticated requests to collection endpoints are blocked
- Authorization tests:
  - user can only read/write their own watch instances and events
- Domain tests:
  - create watch instance linked to a valid reference
  - add timeline event to owned watch
  - browse reference list and detail publicly
  - filter collection by status and family

## Assumptions and Defaults

- Development topology: **Docker for PostgreSQL only**, Django runs locally.
- Database: **PostgreSQL from day one**, not SQLite.
- Auth: **session auth** for the web-first MVP.
- Frontend is not part of this implementation pass.
- Photo uploads, service records as a separate model, biometric auth, and JWT are deferred until after the core collection/reference/timeline loop is working.
