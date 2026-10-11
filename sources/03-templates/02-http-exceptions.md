# HTTP Exception Templates

## Purpose

Use these templates whenever creating or modifying FastAPI routes that raise HTTP exceptions.

Follow the appropriate template based on the actual reason for the error. Preserve the specified status code, message wording, and punctuation unless I explicitly change the convention.

Do not invent alternative messages when a template already applies.

## 1. Bad Request — 400

**Use when:** The client supplies invalid data or a request format that the endpoint cannot accept, such as an empty PATCH request when at least one field is required.

```python
raise HTTPException(
    status_code=status.HTTP_400_BAD_REQUEST,
    detail="{an appropriate message}",
)
```

**Rules:**

* Replace the placeholder with a concise message describing the invalid input.
* Do not use this for missing resources or uniqueness conflicts.

## 2. Conflict — 409, Cause Unknown

**Use when:** A database `IntegrityError` occurs and the specific conflict cannot reliably be identified.

```python
except IntegrityError:
    raise HTTPException(
        status_code=status.HTTP_409_CONFLICT,
        detail="An error occurred.",
    )
```

**Rules:**

* Use this pattern when the underlying integrity violation cannot be reliably attributed to a specific cause.
* Do not claim that a resource already exists unless the cause is known.
* Follow the project's existing transaction and exception-handling conventions.

## 3. Not Found — 404

**Use when:** The requested resource cannot be found, or the endpoint deliberately treats an inaccessible resource as not found.

```python
raise HTTPException(
    status_code=status.HTTP_404_NOT_FOUND,
    detail="{resource} not found.",
)
```

**Rules:**

* Replace `{resource}` with the appropriate resource name.
* Preserve the exact wording and final period.
* Do not use this for invalid input or conflicts.

**Examples:**

* `Research not found.`
* `User not found.`

## 4. Conflict — 409, Known Cause

**Use when:** The specific conflict is known, such as attempting to create a user who already exists.

```python
raise HTTPException(
    status_code=status.HTTP_409_CONFLICT,
    detail="User already exists.",
)
```

**Rules:**

* Replace `User` with the appropriate resource name when adapting this pattern to another known conflict.
* Preserve the wording `"{resource} already exists."` and its final period when the conflict is that the resource already exists.
* Use a more specific message only when the actual conflict is different and an established convention or explicit requirement calls for it.
* Do not use this pattern for every `IntegrityError` automatically.

## 5. Too Many Requests — 429

**Use when:** A client exceeds a rate limit or a request is rejected by rate-limiting rules.

```python
raise HTTPException(
    status_code=status.HTTP_429_TOO_MANY_REQUESTS,
    detail="{an appropriate message}.",
)
```

**Rules:**

* Replace the placeholder with a concise message explaining the rate limit.
* Preserve the final period.
* Do not use this for ordinary validation errors or unrelated conflicts.

## 6. Forbidden — 403

**Use when:** An authenticated user is not permitted to perform an action under the applicable authorization rules or conditions.

```python
raise HTTPException(
    status_code=status.HTTP_403_FORBIDDEN,
    detail="You are not authorized.",
)
```

**Rules:**

* Preserve the exact message and punctuation.
* Use this when the user is authenticated but lacks permission to perform the action.
* Follow the existing project's authorization conventions.

## 7. Unauthorized — 401

**Use when:** An authentication endpoint rejects invalid credentials.

```python
raise HTTPException(
    status_code=status.HTTP_401_UNAUTHORIZED,
    detail="Invalid credentials.",
)
```

**Rules:**

* Preserve the exact message and punctuation.
* Use this template for authentication failures where the credentials are invalid.
* Follow existing authentication and token-handling conventions for other authentication failures.

## General Rules

1. Choose the status code based on the actual cause of the failure.
2. Use these templates instead of inventing a new response convention.
3. Replace placeholders only where permitted; preserve exact fixed messages.
4. Do not catch broad exceptions and turn them into one of these responses merely to suppress errors.
5. Do not expose internal database errors, SQL statements, stack traces, or sensitive information to clients.
6. Do not change unrelated routes or error responses when working on a specific endpoint.
7. Reuse existing imports and follow the project's established exception-handling patterns.
8. If a situation does not fit these templates, inspect existing project conventions and explain the mismatch rather than forcing an inappropriate template.
