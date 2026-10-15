# Database Configuration Conventions

## Purpose

Defines the project's standard synchronous SQLAlchemy database setup, including engine creation, session configuration, and the `get_db()` dependency used by FastAPI routes.

## When to Use

Consult this template when creating the project's `database.py` or when you need to understand its standard database-session setup.

If `database.py` already exists, inspect it before making changes. Preserve existing functionality and project conventions unless a change is explicitly requested.

## Exact Implementation

```python
from collections.abc import Generator

from sqlalchemy import create_engine
from sqlalchemy.orm import Session, sessionmaker

from .config import settings


engine = create_engine(settings.DATABASE_URL)

SessionLocal = sessionmaker(
    autocommit=False,
    autoflush=False,
    bind=engine,
)


def get_db() -> Generator[Session, None, None]:
    db = SessionLocal()

    try:
        yield db
    except Exception:
        db.rollback()
        raise
    finally:
        db.close()
```

## Rules

- Preserve the implementation above when creating `database.py`.
- Import `settings` from the project's existing `.config` module.
- Create the SQLAlchemy engine using `settings.DATABASE_URL`.
- Configure `SessionLocal` with `autocommit=False` and `autoflush=False`.
- Understand that `autoflush=False` disables automatic flushing before queries; it does not prevent flushing when `commit()` is called.
- Keep `get_db()` as a synchronous generator dependency returning `Generator[Session, None, None]`.
- Create one session per dependency invocation and yield it for use by the endpoint.
- Roll back the session when an exception propagates through the generator, then re-raise the original exception.
- Always close the session in the `finally` block.
- Do not treat this dependency's rollback behavior as a replacement for deliberate transaction management in endpoints. Follow the project's established transaction conventions.
- Do not add asynchronous database handling, connection-pool settings, extra abstractions, or unrelated configuration unless explicitly requested.
- If `database.py` already exists, inspect it before making changes. Do not replace unrelated functionality when the task only requires a specific modification.

**Core rule:** Use this exact template for the standard synchronous database setup. Preserve its session lifecycle and transaction conventions, and do not introduce unrequested behavior.
