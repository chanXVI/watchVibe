# Python Backend First / Web Application Plan

## Goal

Start with a Python backend and a web application front end before expanding to mobile platforms.

## Recommended architecture

1. API-first backend
   - Build a REST or GraphQL API in Python
   - Keep mobile readiness in mind by designing endpoints for app-like flows
   - Store user, collection, reference, timeline, and authentication data

2. Web app front end
   - Start with a responsive web interface
   - Use a modern front-end framework like React, Vue, or Svelte
   - Consider a Progressive Web App (PWA) so the web app can behave like a mobile app later

3. Mobile transition path
   - Once the API is stable, use the same backend for mobile clients
   - Evaluate mobile front-end options later: React Native, Flutter, or native iOS/Android
   - Or turn the web app into a PWA for an initial mobile experience

## Suggested Python stack

- Web framework: FastAPI or Django
- Database: PostgreSQL or SQLite for early development
- Auth: JWT tokens for API auth with session option for web clients
- Storage: SQLAlchemy / Tortoise ORM or Django ORM
- Biometric and login support: backend handles credentials and session state; biometric is handled in the client

## Authentication flow

- Web login: email/password + session or JWT
- Logout: revoke session or client-side token
- Biometric auth: provide an API for token refresh or biometric-enabled login state, but actual biometric prompt is client-side

## Why this approach works

- Python backend gives a flexible server-side foundation
- Web application lets you validate UI and workflows quickly
- API-first design makes future mobile apps much easier to build

## Next steps

1. Define the API endpoints for auth, collection, reference data, and timeline events.
2. Choose a Python framework and create the initial data model.
3. Build a simple responsive web UI for login, collection browsing, and watch details.
4. Iterate on the backend API so it can support future mobile clients.

## Notes specific to this project

- Keep the Rolex collection and history model separate from auth so the backend remains reusable.
- Seed core Rolex references in the database for the first app version.
- Use environment-based configuration to support local dev, staging, and production.

## Example roadmap

- Phase 1: Python API, user auth, collection CRUD, reference browse
- Phase 2: Responsive web UI, timeline/event entry, photo upload support
- Phase 3: PWA enhancements and offline-friendly UI
- Phase 4: Native or cross-platform mobile app using the same Python API
