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

- Consult the relevant template before implementing, refactoring, or reviewing related code.
- Read only the templates relevant to the current task. Do not load unrelated templates.
- Follow each template's stated requirements. When a template specifies an exact implementation, preserve it unless an explicit instruction requires a change or a technical issue must be addressed.
- Treat templates intended as starting structures as adaptable to the project's actual requirements.
- Check templates against the current project architecture and requirements before applying them.
- Prefer established project patterns when they conflict with a generic template, unless those patterns are unsafe or incorrect. Explain meaningful conflicts rather than silently overriding either convention.
- Do not invent project requirements, configuration values, or implementation details that are not supported by the codebase or task.
- Update templates when a reusable pattern changes or a better approach is established.

**Core rule:** Use the relevant template to maintain consistency, respect its intended level of flexibility, and preserve existing project conventions without introducing unrelated changes.
