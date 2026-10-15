# Application Configuration Conventions

## Purpose

Defines the project's standard structure for application configuration using Pydantic Settings. The template provides a starting point for `config.py`; the actual settings must be determined from the project's requirements and existing codebase.

## When to Use

Consult this template when creating the project's `config.py` or modifying its application settings.

When extending an existing configuration, inspect the current settings, environment variables, and their usages before making changes. Preserve existing behavior unless the task explicitly requires a change.

## Template

```python
from pydantic_settings import BaseSettings, SettingsConfigDict


class Settings(BaseSettings):
    # Define the appropriate project settings here.

    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
        extra="forbid",
    )


settings = Settings()
```

## Rules

- Inherit `Settings` from `pydantic_settings.BaseSettings`.
- Define the settings required by the project, using appropriate Python type annotations and defaults where justified.
- Use the existing project's settings, environment variables, and configuration usages as the primary reference when modifying or extending the configuration.
- Do not invent settings, credentials, secrets, or default values.
- Configure `SettingsConfigDict` with the exact values shown in the template:
  - `env_file=".env"`
  - `env_file_encoding="utf-8"`
  - `extra="forbid"`
- Understand that `env_file=".env"` specifies a relative path. The location used to resolve it depends on the application's working directory, so follow the project's existing startup conventions.
- Understand that `extra="forbid"` rejects extra inputs encountered during settings validation that do not correspond to defined settings fields. It does not mean that every environment variable on the system must be declared in `Settings`.
- Create the shared instance using `settings = Settings()`.
- Keep secrets and environment-specific values out of source code. Load secret values through the project's established configuration mechanism, and ensure `.env` files containing secrets are excluded from version control.
- Do not treat environment variables or `.env` files as inherently secure storage. Follow the deployment environment's secret-management practices where applicable.
- If a setting is required but its name, type, default, or source is unclear, inspect the existing codebase or ask for clarification.
- Do not add unrelated configuration options, dependencies, validation logic, or abstractions unless required.
- When modifying an existing `config.py`, preserve its existing settings and behavior unless the task explicitly requires changes.

**Core rule:** Follow this template's structure, but determine the actual settings from the project's requirements and existing codebase. Preserve established configuration behavior and never invent settings or secrets.
