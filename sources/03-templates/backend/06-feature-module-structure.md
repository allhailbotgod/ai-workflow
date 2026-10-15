# Feature Folder Structure

## Purpose

Defines the project's standard baseline structure for feature folders. Every new feature folder must contain five specified files, regardless of whether each file initially contains code.

## When to Use

Consult this template whenever creating a feature folder in an application's existing source directory or another directory explicitly specified by the task.

Before creating a feature folder, inspect the existing project structure and use the appropriate location. Do not introduce a competing application directory.

## Required Structure

Every new feature folder must contain these five files:

```text
feature_name/
├── __init__.py
├── enums.py
├── schemas.py
├── models.py
└── routes.py
```

## Rules

- Always create all five files when creating a feature folder, even if some or all of them are initially empty.
- Leave files empty unless the task requires content in them.
- Do not add other files or folders unless explicitly requested or necessary for the task.
- Use the application's existing source directory, such as `src/` or `app/`, or the directory explicitly specified in the task. Do not assume either directory exists without checking.
- Do not create a competing application directory or restructure the existing project to accommodate this template.
- Do not add code, imports, re-exports, or initialization logic to `__init__.py` unless explicitly requested.
- Apply this baseline structure whenever creating a feature folder, regardless of the feature's purpose. Add feature-specific implementation only when required by the task.
- When modifying an existing feature folder, inspect its current contents and preserve existing functionality. Do not remove files or restructure the folder merely to match this template.

**Core rule:** Every newly created feature folder starts with the five required files. Follow the existing project structure, leave unnecessary files empty, and avoid unrelated architectural changes.
