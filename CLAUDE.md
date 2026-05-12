# Event Ticket Platform — Project Guide for Claude Code

## Project Overview
Spring Boot monolith for event ticketing. Portfolio project focused on backend engineering best practices.

## Tech Stack
- Java 21, Spring Boot 3.3.x, Maven
- PostgreSQL + Flyway migrations
- Spring Security + JWT (jjwt library)
- Iyzico Sandbox for payments
- ZXing for QR code generation
- JavaMailSender for async email sending
- Docker Compose for local development

## Package Structure
Package-by-feature under `com.eventticket`:
- `auth` — JWT login/register, refresh tokens, role-based access
- `event` — Organizer creates/manages events (capacity, date, price, category)
- `ticket` — User registration with capacity check and pessimistic locking
- `payment` — Iyzico Sandbox payment integration
- `admin` — Admin management panel
- `common` — Security config, global exception handler, utilities, configs

## Coding Rules
1. All code and comments in English
2. DTOs for every request/response — never expose entities directly to the API
3. Service layer contains all business logic — controllers are thin
4. Use `@Transactional` on service methods that write to DB
5. Capacity check in ticket registration must use pessimistic locking to prevent race conditions
6. All async operations (QR code generation, email sending) use Spring `@Async`
7. Never hardcode secrets — use `.env` file with `${VAR_NAME}` placeholders in `application.yml`
8. Every new service class needs a corresponding unit test

## Environment Variables
Secrets are stored in a `.env` file at project root (not committed to git).
The `.env.example` file documents all required variables.

## Database Conventions
- Flyway migration files: `V{number}__{description}.sql` (double underscore)
- Table names: snake_case plural (e.g., `events`, `ticket_registrations`)
- All tables have: `id UUID PRIMARY KEY DEFAULT gen_random_uuid()`, `created_at`, `updated_at`
- Soft delete where applicable: `deleted_at TIMESTAMP NULL`

## API Conventions
- Base path: `/api/v1`
- Auth endpoints: `/api/v1/auth/**` (public)
- Public endpoints: GET `/api/v1/events` (list and detail)
- All responses use this standard envelope:
  `{ "success": true, "data": {...}, "message": "..." }`
- Errors are handled exclusively by GlobalExceptionHandler

## Roles
- `ROLE_USER` — can register to events, view own tickets
- `ROLE_ORGANIZER` — can create/edit/delete own events
- `ROLE_ADMIN` — full access to everything

## Current Status
Project is empty. Starting from scratch.
No files have been created yet beyond this CLAUDE.md.
