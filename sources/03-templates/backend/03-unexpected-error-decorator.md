# Unexpected Error Handler Decorator

## Purpose

Defines the project's standard `handle_unexpected_errors` decorator for synchronous FastAPI endpoints. The decorator allows expected `HTTPException` responses to pass through unchanged and converts other exceptions into a generic HTTP 500 response.

## When to Use

Consult this template when creating the shared `handle_unexpected_errors` decorator or applying it to existing endpoints.

Use the shared decorator to maintain consistent handling of unexpected endpoint errors. Do not create alternative implementations or introduce additional behavior unless explicitly requested.

This implementation is synchronous and must be preserved as written. Do not assume it supports asynchronous endpoints.

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

- Preserve the implementation above when creating the decorator.
- Keep `@wraps(endpoint)` so the wrapper preserves the original endpoint's metadata, including its name and docstring.
- Re-raise `HTTPException` unchanged so expected HTTP errors retain their original status codes and details.
- Convert other exceptions into an HTTP 500 response using the exact status code and detail message shown above.
- Preserve exception chaining with `from exc`. This retains the original exception as the cause for debugging; it does not itself log the exception.
- Do not expose internal exception details, database errors, SQL statements, or stack traces in the HTTP response.
- Do not add logging, custom exception classes, extra exception handlers, or other behavior unless explicitly requested.
- Ensure production error handling and server configuration do not expose internal tracebacks to clients.
- When applying the decorator to existing endpoints, inspect the current endpoint and preserve its existing behavior and project conventions.
- Do not create duplicate implementations when a shared decorator already exists. Import and reuse the existing implementation instead.
- If an asynchronous endpoint needs this decorator, do not apply this synchronous implementation blindly. Explain the compatibility issue and follow the project's established approach.

**Core rule:** Use this exact decorator as the project's standard handler for unexpected errors in synchronous endpoints. Preserve expected HTTP errors, return the established generic 500 response for unexpected exceptions, and do not introduce unrequested behavior.
