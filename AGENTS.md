# Agent Instructions

## Project Context & Docs
- Project: Restaurant Table Reservation, Pre-order & AI Chatbot (`QL09-W`)
- Jira Key: `SCRUM` | Team: PM, BA, Dev Fullstack, Dev AI
- Canonical specs: `docs/Software Requirements Specification (SRS).md`, `docs/Product Backlog.md`
- Technical specs: `docs/database_schema.md`, `docs/api_contract.md`, `docs/ai_ollama_spec.md`
- Execution plan: `docs/roadmap.md`, `docs/taskboard.md`, `docs/session_log.md`

## Tech Stack
- Frontend: ReactJS (Vite, Tailwind CSS + UI kit) in `frontend/`
- Backend Core: Python FastAPI + SQLAlchemy/Alembic + PostgreSQL in `backend/`
- Chatbot AI: Ollama (pre-trained models only, no fine-tuning) in `backend/app/ai/` and `ollama/`

## Parallel Conflict-Free Ownership
| Role | Owned Paths (Write) | Do Not Edit |
| :--- | :--- | :--- |
| Dev Fullstack | `frontend/**`, `backend/app/core/**`, `backend/app/api/v1/` (except `chat_router.py`), `backend/app/models/core_models.py`, `backend/app/services/*`, `docker-compose.yml`, `.github/**` | `backend/app/ai/**`, `chat_router.py`, `ai_models.py`, `ollama/**`, `ai_eval/**` |
| Dev AI | `backend/app/ai/**`, `backend/app/api/v1/chat_router.py`, `backend/app/models/ai_models.py`, `backend/tests/ai/**`, `ai_eval/**`, `ollama/**`, `docker-compose.ollama.yml` | `frontend/**`, `core_models.py`, core routers/services (read-only import of `availability_service.py` only) |

## Hard Rules
- Never send customer PII (name, phone, email) to Ollama prompts or unmasked logs.
- Never store payment card numbers; normalize all booking slots and reminders to `GMT+7`.
- Never modify `backend/app/main.py` chat mount or `models/__init__.py` after Sprint 1 lock (`SCRUM-18`, `SCRUM-20`).
- Rebase on `origin/develop` before creating Alembic migrations; keep `_core_` and `_ai_` revisions separate.
- Append a concise entry to `docs/session_log.md` at the end of every work session.

## Git & Jira Conventions
- Branch: `feature/SCRUM-<id>-<short-slug>` from `develop`; merge via PR (>= 1 reviewer).
- Commit message: `SCRUM-<id>: <concise summary>`

## Commit Attribution
AI commits MUST include:
```
Co-Authored-By: <Agent Model Name> <noreply@example.com>
```
