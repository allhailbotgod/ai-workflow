# Database Configuration Conventions

## Purpose

When asked to create the project's `database.py`, use the exact implementation below.

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

* Preserve the implementation above when creating `database.py`.
* Import `settings` from the project's existing `.config` module.
* Create the SQLAlchemy engine using `settings.DATABASE_URL`.
* Configure `SessionLocal` with `autocommit=False` and `autoflush=False`.
* Keep `get_db()` as a synchronous generator dependency returning `Generator[Session, None, None]`.
* Roll back the session when an exception occurs, then re-raise the original exception.
* Always close the session in the `finally` block.
* Do not add asynchronous database handling, connection-pool settings, extra abstractions, or unrelated configuration unless explicitly requested.
* If `database.py` already exists, inspect it before making changes. Do not replace unrelated functionality when the task only requires a specific modification.

**Core rule:** Use this exact template for the standard database setup.
