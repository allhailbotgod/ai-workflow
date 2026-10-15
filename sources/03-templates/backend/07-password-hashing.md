# Password Hashing and Verification

## Purpose

Defines the project's standard password hashing and verification functions using `bcrypt`. These functions protect stored passwords by storing password hashes instead of plaintext passwords.

## When to Use

Consult this template when creating the project's password hashing utility or when integrating password hashing and verification into authentication flows.

If the utility already exists, inspect it and import the existing functions rather than creating duplicate implementations.

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

- Preserve the implementation above when creating the password hashing utility.
- Use `bcrypt` for password hashing and verification.
- Use `hash_pwd(password)` to hash a plaintext password before storing it.
- Store the encoded hash returned by `hash_pwd()` in the database, never the original plaintext password.
- Use `verify_hash(hashed_password, password)` to verify a supplied password against its stored hash. The first argument is the stored hash; the second is the supplied plaintext password.
- Never hash a password again when verifying it. Use `verify_hash()` because bcrypt generates a salt, meaning hashing the same password again can produce a different hash.
- Do not replace bcrypt with another hashing library or algorithm unless explicitly requested.
- Be aware that bcrypt has a 72-byte password input limit. UTF-8 characters can occupy multiple bytes, so this is not necessarily 72 characters. Establish an explicit validation policy when needed, and do not silently truncate passwords.
- Do not add unnecessary abstractions, dependencies, or behavior.
- If the utility already exists, import and reuse the existing functions instead of creating duplicate implementations.
- When integrating these functions into authentication flows, follow the project's existing validation, error-handling, and password-storage conventions.

**Core rule:** Use this exact template for password hashing and verification. Store only password hashes, and verify supplied passwords using the stored hash rather than hashing them again.
