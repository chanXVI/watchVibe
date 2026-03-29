# Django Startup Sequence: Thin Auth, Then Core MVP

## Summary

Recommended approach: do not fully “finish auth” before product work. Build a thin,
production-sane auth foundation first, then move immediately into the MVP business logic
that proves the app: collection, Rolex references, and ownership timeline.

The reason is straightforward: in this repo, auth is support infrastructure, while the
product value is in:

- personal collection management
- seeded Rolex reference data
- watch detail + timeline/history flows

Treat auth as an enabling slice, not the main milestone.

## Key Changes / Build Order

### 1. Create the Django baseline first

- Start with Django + Django REST Framework.
- Use a custom user model from day one, even if it only supports email/password initially.
- Set up environment-based settings, local DB, media storage path, and a clean app split.

Recommended initial apps:

- accounts
- references
- collection
- timeline

### 2. Implement only the auth needed to unlock the MVP

Build the smallest auth surface that supports real user-owned data:

- sign up
- log in
- log out
- authenticated “current user” endpoint/page context
- password reset can wait unless you need public beta readiness
- biometric/WebAuthn should wait until after the MVP core flow works

Auth choices:

- If server-rendered Django: use Django session auth.
- If Django API + separate frontend: use session auth for web first unless you have a
  strong reason to introduce JWT now.

Do not spend early time on:

- social auth
- complex token lifecycle
- biometric enrollment
- advanced account settings
- multi-device/session management

### 3. Move immediately into the business-logic MVP

After basic auth works, build the product in this order:

1. references

- Brand, ModelFamily, Reference models
- seed 10 to 20 Rolex references
- browse/search/filter reference data

2. collection

- WatchInstance tied to user and reference
- add/edit/list/detail for a user’s watches
- status support: owned, sold, wishlist, researching

3. timeline

- Event model tied to WatchInstance
- add/list timeline events
- basic service record support

4. detail views / API composition

- watch detail page combines watch instance + reference + events
- reference detail page shows historical/spec content

This sequence matches the product docs and gives you usable software fast.

### 4. Keep auth separated from domain logic

- accounts should only own user identity and access control.
- Business entities should depend on user, but not on auth workflow details.
- Avoid embedding collection rules into auth code or serializers.

This keeps the backend reusable for later web/mobile clients.

## Public Interfaces / Data Decisions

Initial data model priorities:

- User
- Brand
- ModelFamily
- Reference
- WatchInstance
- Event
- optionally ServiceRecord in MVP if it is just a specialized event or simple related
  model

Initial user-visible flows:

- user signs up or logs in
- user browses Rolex references
- user adds a watch to their collection
- user opens watch detail
- user logs a purchase/service/milestone event

Defaults to choose now:

- use email as the primary login identity
- use session auth for web-first Django
- use PostgreSQL if you want fewer migration headaches later; SQLite is acceptable only
  for local bootstrapping
- seed reference data manually at first rather than designing an import pipeline
  immediately

## Test Plan

Validate these flows first:

- unauthenticated users cannot access personal collection endpoints/pages
- authenticated user can create and view only their own watch instances
- reference data is publicly browsable if that is the intended product behavior
- user can create a watch instance linked to a valid reference
- user can add timeline events to their own watch
- watch detail correctly shows reference info plus personal history
- filtering by family and status works on collection list

Minimum automated coverage:

- model tests for relationships and required fields
- auth/access tests for collection isolation
- API or view tests for create/list/detail flows
- seed-data smoke test to confirm starter Rolex references load correctly

## Assumptions

- The first release is web-first, not mobile-first.
- Rolex reference browsing is part of the MVP, not a later content phase.
- Biometric auth is explicitly post-MVP.
- The immediate goal is to validate the core collector workflow, not build a full account
  platform.

Practical milestone framing:

1. Django scaffold + custom user + login/logout
2. Rolex reference archive seed + browse
3. Collection CRUD
4. Timeline/events
5. polish, uploads, and later auth enhancements

If you follow this order, you reduce risk and get to the first “real product” much faster
than leading with deep authentication work.
