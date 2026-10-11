# Unexpected Error Handler Decorator

## Purpose

When asked to create the shared `handle_unexpected_errors` decorator, use the exact implementation below. Do not replace it with a different error-handling approach or introduce additional behavior unless explicitly requested.

## Exact Implementation

```python
from functools import wraps

from fastapi import HTTPException, status


def handle_unexpected_errors(endpoint):
    @wraps(endpoint)
    def wrapper(*args, **kwargs):
        try:
            return endpoint(*args, **kwargs)
        except HTTPException:
            raise
        except Exception as exc:
            raise HTTPException(
                status_code=status.HTTP_500_INTERNAL_SERVER_ERROR,
                detail="An error occurred.",
            ) from exc

    return wrapper
```

## Rules

* Preserve the implementation above when creating the decorator.
* Keep `@wraps(endpoint)` so the wrapper preserves the original endpoint's metadata.
* Re-raise `HTTPException` unchanged so expected HTTP errors retain their status codes and details.
* Convert other exceptions into an HTTP 500 response using the exact status code and detail message shown above.
* Preserve exception chaining with `from exc`.
* Do not add logging, custom exception classes, extra exception handlers, or other behavior unless explicitly requested.
* When applying the decorator to existing endpoints, inspect the current endpoint and preserve its existing behavior and project conventions.
* Do not create duplicate implementations when a shared decorator already exists. Import and reuse the existing implementation instead.

**Core rule:** Use this exact decorator as the project's standard unexpected-error handler.
