# SQLAlchemy Model Conventions

## 1. Inspect Before Creating

Before creating or modifying SQLAlchemy models or the shared `models.py` file:

* Inspect the project's existing model infrastructure and relevant model files.
* Reuse existing mixins, foreign-key helpers, and conventions where available.
* If asked to create the shared `models.py` from scratch, use the exact definitions provided in this document.
* Do not redefine shared functionality inside individual model files.
* Do not modify existing shared infrastructure unless the task requires it.
* Make only the changes needed for the requested task.

## 2. Shared `models.py` Template

When asked to create the project's shared `models.py` containing the standard base class, mixins, and foreign-key helpers, use this implementation.

```python
from __future__ import annotations

from datetime import datetime
from uuid import UUID, uuid4

from sqlalchemy import DateTime, ForeignKey, Uuid, func
from sqlalchemy.orm import DeclarativeBase, Mapped, MappedColumn, mapped_column


class Base(DeclarativeBase):
    pass


class UUIDPrimaryKeyMixin:
    id: Mapped[UUID] = mapped_column(
        Uuid(as_uuid=True),
        primary_key=True,
        default=uuid4,
    )


class CreatedAtMixin:
    created_at: Mapped[datetime] = mapped_column(
        DateTime(timezone=True),
        server_default=func.now(),
    )


class TimestampMixin(CreatedAtMixin):
    updated_at: Mapped[datetime] = mapped_column(
        DateTime(timezone=True),
        server_default=func.now(),
        onupdate=func.now(),
    )


def uuid_fk(target: str) -> MappedColumn[UUID]:
    return mapped_column(
        Uuid(as_uuid=True),
        ForeignKey(target, ondelete="FORBID"),
        nullable=False,
    )


__all__ = [
    "Base",
    "CreatedAtMixin",
    "TimestampMixin",
    "UUIDPrimaryKeyMixin",
    "uuid_fk",
]
```

### Rules for this shared file

* Preserve the definitions and behavior shown above when reproducing the existing project infrastructure.
* Keep `TimestampMixin` inheriting from `CreatedAtMixin`. It already supplies both standard timestamp columns.
* Preserve the exact `__all__` list shown above unless explicitly asked to change it.
* Do not add extra mixins, helper functions, imports, or abstractions without a requirement.
* If modifying an existing `models.py`, inspect its current contents first and preserve any additional project-specific functionality that is not shown in this template.
* Do not replace an existing project implementation blindly if the task is to modify only part of it.

## 3. Use Existing Mixins in Individual Models

Import the appropriate mixins from the project's shared `models.py`. Do not manually redeclare columns already supplied by an inherited mixin.

### `Base`

Every ORM model must inherit from `Base`, directly or through an established project-specific base class.

### `UUIDPrimaryKeyMixin`

Use this mixin when the model requires the standard UUID primary key named `id`.

It provides:

* `id: Mapped[UUID]`
* SQLAlchemy `Uuid(as_uuid=True)`
* `primary_key=True`
* `default=uuid4`

Do not manually declare `id` when this mixin is inherited.

### `CreatedAtMixin`

Use this mixin when the model requires `created_at` but does not require `updated_at`.

It provides a timezone-aware `created_at` column with `server_default=func.now()`.

Do not manually declare `created_at` when this mixin is inherited.

### `TimestampMixin`

Use this mixin when the model requires both `created_at` and `updated_at`.

It inherits `CreatedAtMixin` and provides `updated_at` with:

* `DateTime(timezone=True)`
* `server_default=func.now()`
* `onupdate=func.now()`

Do not additionally inherit `CreatedAtMixin` or manually declare either standard timestamp column.

## 4. Choosing the Correct Mixins

| Required columns                 | Inherit                                 |
| -------------------------------- | --------------------------------------- |
| `id` only                        | `UUIDPrimaryKeyMixin`                   |
| `id`, `created_at`               | `UUIDPrimaryKeyMixin`, `CreatedAtMixin` |
| `id`, `created_at`, `updated_at` | `UUIDPrimaryKeyMixin`, `TimestampMixin` |

Combine the selected mixins with `Base` according to the project's established model inheritance conventions.

For example, a `Notification` model with `id`, `content`, `error`, and `created_at` should inherit `Base`, `UUIDPrimaryKeyMixin`, and `CreatedAtMixin`. It should manually declare only `content` and `error`, using the types and constraints appropriate to the requirements.

## 5. Additional Time-Related Columns

The standard timestamp mixins provide only `created_at` and `updated_at`.

For other time-related columns, such as `viewed_at`, `published_at`, `deleted_at`, `expires_at`, or `read_at`:

* Declare each additional column manually in the relevant model.
* Normally use `Mapped[datetime]` or `Mapped[datetime | None]`, as appropriate, with `DateTime(timezone=True)` and `mapped_column`.
* Import `datetime` and any required SQLAlchemy types or functions.
* Follow existing project conventions for nullability, defaults, and update behavior.
* Do not add these columns to the standard mixins merely because they contain timestamps.
* Do not assume a custom timestamp needs a default value. For example, `viewed_at` may need to remain `None` until the resource is viewed.

**Rule:** Use mixins for the standard columns they provide. Declare additional time-related columns individually according to their purpose.

## 6. Use Existing Foreign-Key Helpers

Reuse the project's foreign-key helpers when their behavior matches the required relationship.

### `uuid_fk(target)`

* Creates a UUID foreign-key column.
* Uses `ForeignKey(target, ondelete="FORBID")`.
* Sets `nullable=False`.

Pass the correct foreign-key target using the project's established naming conventions.

Do not select a helper based solely on its name. Check its nullability and deletion behavior against the requirement and existing models. If no helper matches, inspect existing models before introducing a different implementation.

## 7. Define Model-Specific Columns Normally

Declare model-specific columns using the project's established SQLAlchemy 2.x conventions, including `Mapped`, `mapped_column`, appropriate SQLAlchemy types, and existing relationship and constraint patterns.

For each model:

* Use existing mixins for the standard columns they provide.
* Use existing foreign-key helpers when their behavior matches the requirement.
* Declare remaining columns, relationships, indexes, and constraints as required.
* Do not invent types, nullability, defaults, relationships, indexes, or constraints when they are not specified. Inspect comparable models and ask for clarification if a consequential decision remains unresolved.
* Do not manually duplicate a column provided by an inherited mixin.

## 8. Preserve Existing Project Conventions

* Follow the project's naming, table, relationship, constraint, and type-annotation conventions.
* Avoid unnecessary dependencies, abstractions, and duplicate declarations.
* Do not modify shared mixins or helper functions to accommodate one model unless explicitly requested.
* Import shared infrastructure from the correct module.
* Review changes for duplicated columns, missing imports, incorrect mixin selection, and unintended changes.
* Update migrations only when required by the task and the project's existing workflow.

**Core rule:** Reuse the existing shared model infrastructure. When asked to create that infrastructure, use the exact template above. When asked to create an individual model, inherit the appropriate mixins, reuse matching foreign-key helpers, and define only the remaining model-specific fields.
