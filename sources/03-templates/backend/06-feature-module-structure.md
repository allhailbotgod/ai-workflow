# Feature Folder Structure

Whenever asked to create a feature folder inside the application's `src/` or `app/` directory or whatever specified directory, create these five files inside it, even if some or all of them are initially empty:

```text
feature_name/
├── __init__.py
├── enums.py
├── schemas.py
├── models.py
└── routes.py
```

## Rules

* Always create all five files when creating a feature folder.
* Leave files empty unless the task requires content in them.
* Do not add other files or folders unless explicitly requested or necessary for the task.
* Use the application's existing `src/` or `app/` directory or whatever was specified in the task. Do not create a competing application directory.
* Do not add code, imports, re-exports, or initialization logic to `__init__.py` unless explicitly requested.
* Apply this structure whenever creating a feature folder, regardless of the feature's purpose.
