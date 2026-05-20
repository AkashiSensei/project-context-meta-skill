---
name: commit-helper
description: Commit task-relevant changes using the repository's historical style, and generate PR descriptions from staged changes and project context.
disable-model-invocation: true
---

# Skill: Commit Helper
Goal: Commit task-relevant changes with a high-density message and generate PR descriptions while auto-syncing project context.

## Workflow
1. **Context Extraction**:
   - Run `git status --short` and `git diff --staged`; record which files were already staged before this skill changes anything.
   - If nothing is staged, inspect `git diff --stat`, `git diff`, and `ACTIVE_TASK.md` to identify a narrow task-relevant commit scope.
   - Run `git --no-pager log -n 5 --oneline` to identify project commit style.
   - For PR descriptions: Identify the base branch (e.g., `main`) and run `git --no-pager log [base]..HEAD --oneline` and `git diff [base]...HEAD` to capture the full scope of changes across multiple commits.
   - Locate nearest `.context/` and identify task intent from `ACTIVE_TASK.md`.
2. **Auto-Sync**:
   - Update `ACTIVE_TASK.md` (auto-check completed items).
   - Log significant milestones/decisions in `ROADMAP.md`.
   - Stage only the context files changed by this skill (for example, `[Context Root]/.context/ACTIVE_TASK.md` and `[Context Root]/.context/ROADMAP.md`) so the task record is committed together with the implementation.
3. **Commit Execution**:
   - Draft a concise commit message following project rules (or `type(scope): desc`) and recent repository history.
   - Commit the union of files staged before this skill ran and the context files staged by Auto-Sync.
   - If no files were staged before this skill ran, stage exact task-relevant paths only after reviewing their diffs; never use broad commands such as `git add .` or `git add -A`.
   - If the relevant scope is ambiguous, empty, or appears to include unrelated work, stop and ask the user before staging or committing.
   - After the final staged diff is clear and complete, run `git commit -m "[message]"` directly.
   - **PR Description** (Triggered by `archive-task` or explicit request):
     - Wrap in a code block.
     - Language: Match current conversation.
     - Scope: Analyze all changes since diverging from the base branch (not just the current commit).
     - Content: Concise intent, bulleted changes, test summary, and issue refs (from `ACTIVE_TASK.md` and commit history).
4. **Output**: Report the commit hash and message after committing. For PR descriptions, present the generated text for review.

## Interaction Style
- Technical, concise, zero fluff.
- Query user first if changes appear ambiguous.
