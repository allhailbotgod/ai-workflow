# SQLAlchemy Model Conventions

## Purpose

Defines the project's standard SQLAlchemy 2.x model infrastructure and conventions for creating ORM models. Covers the shared declarative base, UUID primary-key and timestamp mixins, foreign-key helpers, and model-specific column definitions.

## When to Use

Consult this template when creating or modifying SQLAlchemy ORM models or the shared `models.py` infrastructure. Inspect the target project's existing implementation first and adapt these conventions to its established architecture rather than assuming every project uses the exact same setup.

## Inspect Before Creating

Before creating or modifying SQLAlchemy models or the shared `models.py` file:

- Inspect the project's existing model infrastructure and relevant model files.
- Reuse existing mixins, foreign-key helpers, and conventions where available.
- If asked to create the shared `models.py` from scratch, use the exact definitions provided in this document.
- Do not redefine shared functionality inside individual model files.
- Do not modify existing shared infrastructure unless the task requires it.
- Make only the changes needed for the requested task.

## Shared `models.py` Template

When asked to create the project's shared `models.py` containing the standard base class, mixins, and foreign-key helper, use this implementation.

```python
from __future__ import annotations

from datetime import datetime
from uuid import UUID, uuid4

from sqlalchemy import DateTime, ForeignKey, Uuid, func, text
from sqlalchemy.orm import DeclarativeBase, Mapped, MappedColumn, mapped_column


class Base(DeclarativeBase):
    pass


class UUIDPrimaryKeyMixin:
    id: Mapped[UUID] = mapped_column(
        Uuid(as_uuid=True),
        primary_key=True,
        default=uuid4,
        server_default=text("gen_random_uuid()"),
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


def uuid_fk(target: str, *, ondelete: str | None = None) -> MappedColumn[UUID]:
    foreign_key = (
        ForeignKey(target)
        if ondelete is None
        else ForeignKey(target, ondelete=ondelete)
    )

    return mapped_column(Uuid(as_uuid=True), foreign_key, nullable=False)


__all__ = [
    "Base",
    "CreatedAtMixin",
    "TimestampMixin",
    "UUIDPrimaryKeyMixin",
    "uuid_fk",
]
```

### Rules for this shared file

- Preserve the definitions and behavior shown above when reproducing the existing project infrastructure.
- Keep `TimestampMixin` inheriting from `CreatedAtMixin`. It already supplies both standard timestamp columns.
- Preserve the exact `__all__` list shown above unless explicitly asked to change it.
- Preserve both `default=uuid4` and `server_default=text("gen_random_uuid()")` in `UUIDPrimaryKeyMixin`. They provide application-side and database-side UUID generation, respectively.
- Keep `uuid_fk()` accepting an optional, keyword-only `ondelete` argument. Do not hardcode a deletion action into the helper.
- Do not add extra mixins, helper functions, imports, or abstractions without a requirement.
- If modifying an existing `models.py`, inspect its current contents first and preserve any additional project-specific functionality that is not shown in this template.
- Do not replace an existing project implementation blindly if the task is to modify only part of it.

## Use Existing Mixins in Individual Models

Import the appropriate mixins from the project's shared `models.py`. Do not manually redeclare columns already supplied by an inherited mixin.

### `Base`

Every ORM model must inherit from `Base`, directly or through an established project-specific base class.

### `UUIDPrimaryKeyMixin`

Use this mixin when the model requires the standard UUID primary key named `id`.

It provides:

- `id: Mapped[UUID]`
- SQLAlchemy `Uuid(as_uuid=True)`
- `primary_key=True`
- `default=uuid4`
- `server_default=text("gen_random_uuid()")`

The Python-side `default=uuid4` allows SQLAlchemy to generate a UUID when an insert does not supply an ID. The database-side `server_default` allows PostgreSQL to generate the UUID when the insert omits the ID and relies on the database default.

The database function must be supported by the target PostgreSQL version and configuration.

Do not manually declare `id` when this mixin is inherited.

### `CreatedAtMixin`

Use this mixin when the model requires `created_at` but does not require `updated_at`.

It provides a timezone-aware `created_at` column with `server_default=func.now()`.

The default is evaluated by the database when a row is inserted without an explicit value for this column.

Do not manually declare `created_at` when this mixin is inherited.

### `TimestampMixin`

Use this mixin when the model requires both `created_at` and `updated_at`.

It inherits `CreatedAtMixin` and provides `updated_at` with:

- `DateTime(timezone=True)`
- `server_default=func.now()`
- `onupdate=func.now()`

The database supplies the initial `updated_at` value on insertion. SQLAlchemy's `onupdate=func.now()` supplies an update expression when SQLAlchemy issues an update that changes the row. It is not, by itself, a database trigger for updates performed outside SQLAlchemy.

Do not additionally inherit `CreatedAtMixin` or manually declare either standard timestamp column.

## Choosing the Correct Mixins

| Required columns | Inherit |
|---|---|
| `id` only | `UUIDPrimaryKeyMixin` |
| `id`, `created_at` | `UUIDPrimaryKeyMixin`, `CreatedAtMixin` |
| `id`, `created_at`, `updated_at` | `UUIDPrimaryKeyMixin`, `TimestampMixin` |

Combine the selected mixins with `Base` according to the project's established model inheritance conventions.

For example, a `Notification` model with `id`, `content`, `error`, and `created_at` should inherit `Base`, `UUIDPrimaryKeyMixin`, and `CreatedAtMixin`. It should manually declare only `content` and `error`, using the types and constraints appropriate to the requirements.

## Additional Time-Related Columns

The standard timestamp mixins provide only `created_at` and `updated_at`.

For other time-related columns, such as `viewed_at`, `published_at`, `deleted_at`, `expires_at`, or `read_at`:

- Declare each additional column manually in the relevant model.
- Normally use `Mapped[datetime]` or `Mapped[datetime | None]`, as appropriate, with `DateTime(timezone=True)` and `mapped_column`.
- Import `datetime` and any required SQLAlchemy types or functions.
- Follow existing project conventions for nullability, defaults, and update behavior.
- Do not add these columns to the standard mixins merely because they contain timestamps.
- Do not assume a custom timestamp needs a default value. For example, `viewed_at` may need to remain `None` until the resource is viewed.

**Rule:** Use mixins for the standard columns they provide. Declare additional time-related columns individually according to their purpose.

## Use Existing Foreign-Key Helpers

Reuse the project's foreign-key helpers when their behavior matches the required relationship.

### `uuid_fk(target, *, ondelete=None)`

The `uuid_fk()` helper:

- Creates a UUID foreign-key column using `Uuid(as_uuid=True)`.
- Requires a target identifying the referenced table and column, following the project's established naming conventions.
- Sets `nullable=False`.
- Accepts an optional, keyword-only `ondelete` argument.
- Omits the explicit `ondelete` option when the argument is `None`.
- Passes the supplied deletion action to SQLAlchemy's `ForeignKey` when `ondelete` is provided.

When no deletion action is supplied, the database's default foreign-key behavior applies. The helper does not automatically forbid deletion or guarantee cascading deletion.

Use an explicit deletion action only when the relationship's requirements call for it. For example:

```python
user_id = uuid_fk("users.id", ondelete="CASCADE")
```

This configures the foreign key to request cascading deletion of dependent rows when the referenced user is deleted, subject to the database's foreign-key behavior.

**Database-level foreign-key actions and SQLAlchemy ORM relationship cascades are separate mechanisms.** `ondelete="CASCADE"` configures the database's foreign-key behavior. SQLAlchemy ORM cascade settings, such as `cascade="all, delete-orphan"` on a relationship, govern how the ORM handles related objects during ORM operations. Configure ORM relationship cascades separately when the project's relationship behavior requires them; do not assume that setting one automatically configures the other.

Other database-level actions, such as `RESTRICT` or `SET NULL`, may be appropriate for different relationships. Choose an action supported by the target database and compatible with the column's nullability and application requirements.

Do not select a helper solely because its name appears suitable. Check its target, nullability, and deletion behavior against the requirement and existing models. If no helper matches, inspect the existing infrastructure before introducing a different implementation.

## Define Model-Specific Columns Normally

Declare model-specific columns using the project's established SQLAlchemy 2.x conventions, including `Mapped`, `mapped_column`, appropriate SQLAlchemy types, and existing relationship and constraint patterns.

For each model:

- Use existing mixins for the standard columns they provide.
- Use existing foreign-key helpers when their behavior matches the requirement.
- Declare remaining columns, relationships, indexes, and constraints as required.
- Do not invent types, nullability, defaults, relationships, indexes, or constraints when they are not specified. Inspect comparable models and ask for clarification if a consequential decision remains unresolved.
- Do not manually duplicate a column provided by an inherited mixin.
- Specify `ondelete` explicitly when the relationship requires a particular deletion action and the helper supports it.

## Preserve Existing Project Conventions

- Follow the project's naming, table, relationship, constraint, and type-annotation conventions.
- Avoid unnecessary dependencies, abstractions, and duplicate declarations.
- Do not modify shared mixins or helper functions to accommodate one model unless explicitly requested.
- Import shared infrastructure from the correct module.
- Review changes for duplicated columns, missing imports, incorrect mixin selection, and unintended changes.
- Update migrations only when required by the task and the project's existing workflow.

**Core rule:** Reuse the existing shared model infrastructure. When asked to create that infrastructure, use the exact template above. When asked to create an individual model, inherit the appropriate mixins, reuse matching foreign-key helpers, and define only the remaining model-specific fields.
