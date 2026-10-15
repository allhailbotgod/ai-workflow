# AI Workflow

A version-controlled collection of personal working preferences, coding conventions, reusable templates, examples, and explanations for AI-assisted software development.

The goal is to use AI to improve productivity while maintaining engineering understanding, sound judgment, and independent problem-solving ability.

## Repository Structure

```text
.
├── README.md
├── AGENTS.md
├── build-agents.sh
└── sources/
    ├── 01-core/          # Personal rules and task workflow
    ├── 02-coding/        # Coding conventions and patterns
    ├── 03-templates/     # Reusable implementation templates
    ├── 04-examples/      # Working examples
    └── 05-explanations/  # Concept explanations and technical notes
```

Backend resources are organized separately from frontend resources so the collection can expand without mixing unrelated technologies.

## How It Works

- `build-agents.sh` combines the explicitly selected source instruction files into the repository's `AGENTS.md` and installs the result as `~/.codex/AGENTS.md`.
- Templates, examples, and explanations remain separate reference materials. Consult only those relevant to the current task instead of loading everything into the global instructions.
- `AGENTS.md` is generated output. Edit its source files, not the generated file directly.

## Updating Global Instructions

After changing a source instruction file, run:

```bash
./build-agents.sh
```

The script backs up existing global instructions before replacing them.

Verify the generated sources when needed:

```bash
grep -n '^# Source:' AGENTS.md
```

## Resource Guidelines

- Check relevant resource indexes before creating something that may already exist.
- Read the relevant template, example, or explanation before implementing related code when one exists.
- Load only resources relevant to the current task.
- Follow templates according to their stated requirements and flexibility. Validate them against the current project's architecture and requirements.
- Add reusable material when it has lasting value, not merely to document every task.
- Keep backend and frontend resources separate where their approaches differ.

## Principles

- Keep implementations simple, secure, and maintainable.
- Avoid unnecessary dependencies, abstractions, and unrelated changes.
- Explain unfamiliar concepts when needed without overexplaining routine code.
- Verify changes and accurately report test results.
- Improve the workflow based on actual experience rather than adding complexity without a demonstrated need.
- Preserve the developer's ability to understand, review, and maintain AI-assisted code.
