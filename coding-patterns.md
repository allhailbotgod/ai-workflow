# Coding Patterns

This file records implementation conventions to reuse when working on my projects. Project-specific instructions and the actual repository take precedence. Update this file only when a pattern is established or explicitly confirmed.

## Current Focus

- Language: Python
- API framework: FastAPI
- Database: PostgreSQL
- ORM: SQLAlchemy 2.x
- Schemas: Pydantic v2
- Migrations: Alembic

## Established Patterns

The following patterns have been established in my existing backend work:

- Prefer synchronous FastAPI route handlers where that matches the project.
- Use SQLAlchemy 2.x query patterns such as `select(...)` and `db.scalar(...)`.
- Use Pydantic v2 configuration such as `ConfigDict(from_attributes=True)` when schemas need ORM attribute conversion.
- Use Alembic for database migrations.
- Reuse the project's existing error-handling conventions, including `@handle_unexpected_errors` where already established.
- Reuse the project's database dependency and transaction conventions. In existing projects, `get_db()` handles rollback, and `with db.begin()` is used where transaction auto-commit is appropriate.
- Use relationship-loading strategies such as `selectinload` or `joinedload` when appropriate to the query and existing model relationships.
- Keep modules separated according to the repository's established structure instead of introducing a new architecture for an isolated task.

## Rules for Reusing Patterns

1. Inspect the relevant routes, schemas, models, dependencies, and tests before generating similar code.
2. Follow the closest valid existing example rather than inventing a new convention.
3. Do not apply a pattern blindly if the current use case has different requirements.
4. Do not introduce a dependency or abstraction merely to save a few lines.
5. Do not change sync/async style, transaction handling, or error handling across a project without a specific reason.
6. If existing examples conflict, identify the conflict and prefer the pattern most relevant to the current feature. Ask if the choice materially affects architecture or behaviour.
7. Add a pattern to this file only after it has been repeatedly used or explicitly confirmed as a preference.

## Verify Before Assuming

These notes are a starting point, not a substitute for repository inspection. Before implementation, verify that the project's current code still follows the relevant pattern. Do not assume every project uses all the conventions listed here.
