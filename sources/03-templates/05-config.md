# Application Configuration Conventions

## Purpose

When asked to create the project's `config.py`, use the following template as a guide. Preserve the configuration structure while defining the settings required by the specific project.

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

* Inherit `Settings` from `pydantic_settings.BaseSettings`.
* Define the settings required by the project, using appropriate Python type annotations and defaults where justified.
* Use the existing project's settings and environment variables as the primary reference when modifying or extending the configuration.
* Do not invent settings, credentials, secrets, or default values.
* Configure `SettingsConfigDict` with the exact values shown in the template:

  * `env_file=".env"`
  * `env_file_encoding="utf-8"`
  * `extra="forbid"`
* Create the shared instance using `settings = Settings()`.
* Keep secrets and environment-specific values in environment variables rather than hardcoding them into source code.
* If a setting is required but its name, type, or default is unclear, inspect the existing codebase or ask for clarification.
* Do not add unrelated configuration options, dependencies, validation logic, or abstractions unless required.
* When modifying an existing `config.py`, preserve its existing settings and behavior unless the task explicitly requires changes.

**Core rule:** Follow this template's structure, but determine the actual settings from the project's requirements and existing codebase.
