# Password Hashing and Verification

## Purpose

Whenever asked to create the project's password hashing and verification functions, use the exact implementation below.

## Exact Implementation

```python
import bcrypt


def hash_pwd(password):
    return bcrypt.hashpw(password.encode("utf-8"), bcrypt.gensalt()).decode("utf-8")


def verify_hash(hashed_password: str, password: str) -> bool:
    return bcrypt.checkpw(
        password.encode("utf-8"),
        hashed_password.encode("utf-8"),
    )
```

## Rules

* Preserve the implementation above when creating the password hashing utility.
* Use `bcrypt` for password hashing and verification.
* Use `hash_pwd(password)` to hash a plaintext password before storing it.
* Use `verify_hash(hashed_password, password)` to verify a supplied password against its stored hash.
* Never store plaintext passwords in the database.
* Never hash a password again when verifying it; use `verify_hash()` instead.
* Do not replace bcrypt with another hashing library or algorithm unless explicitly requested.
* Do not add unnecessary abstractions, dependencies, or behavior.
* If the utility already exists, import and reuse the existing functions instead of creating duplicate implementations.

**Core rule:** Use this exact template for password hashing and verification.
