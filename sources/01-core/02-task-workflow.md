# Coding Task Workflow

## 1. Understand
- Inspect the relevant existing code before making changes.
- Identify the problem and the expected outcome.
- Ask questions only when missing information materially affects the implementation.

### Resource Discovery
- Check the repository's `README.md` or relevant resource index when a task could benefit from existing templates, examples, or explanations.
- Load only the specific resources relevant to the current task; do not read every resource by default.
- Treat reusable resources as guidance, and verify that they fit the current project's architecture and requirements.
- If no relevant resource exists, proceed using the current project and established conventions. Create or update a reusable resource only when it has lasting value.

## 2. Plan
- For familiar, straightforward tasks, use a minimal plan.
- For unfamiliar or architectural tasks, explain the approach before implementing.
- Prefer the simplest solution that satisfies the requirements.

## 3. Implement
- Follow the existing project structure and coding conventions.
- Avoid unnecessary dependencies, abstractions, and unrelated changes.
- Preserve existing behavior unless the task requires changing it.

## 4. Verify
- Run relevant tests and checks when practical.
- Inspect the final diff for unintended changes.
- Never claim that a check passed unless it was actually run.

## 5. Report
Summarize:
- What changed.
- Why the approach was chosen.
- What was tested and the results.
- Anything that remains uncertain or incomplete.

## 6. Learn
- Explain unfamiliar concepts when needed.
- Do not explain routine code unnecessarily.
- Update permanent workflow rules only when a recurring preference or useful lesson has been established.
