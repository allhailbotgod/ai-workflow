# Backend Templates

Reusable implementation templates for Python backend development, primarily using FastAPI, SQLAlchemy, Pydantic, and PostgreSQL.

## Available Templates

| File | Use when |
|------|----------|
| `backend/01-sqlalchemy-models.md` | Creating or modifying SQLAlchemy models |
| `backend/02-http-exceptions.md` | Choosing or implementing HTTP exceptions |
| `backend/03-unexpected-error-decorator.md` | Handling unexpected endpoint errors |
| `backend/04-database.md` | Configuring database access and sessions |
| `backend/05-config.md` | Managing application configuration and environment variables |
| `backend/06-feature-module-structure.md` | Organizing a backend feature into modules |
| `backend/07-password-hashing.md` | Implementing or reviewing password hashing |

## Usage Rules

- Consult the relevant template when implementing, refactoring, or reviewing related code.
- Treat templates as starting points, not unquestionable rules. Check them against the current project architecture and requirements.
- Prefer established project patterns when they conflict with a generic template, unless those patterns are unsafe or incorrect.
- Do not load unrelated templates.
- Update templates when a reusable pattern changes or a better approach is established.
