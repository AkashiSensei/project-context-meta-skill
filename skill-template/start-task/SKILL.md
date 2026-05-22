---
name: start-task
description: Initialize a new development task. Extracts relevant context from SPEC/ROADMAP to ACTIVE_TASK.md.
disable-model-invocation: true
---

# Skill: Start Task
Goal: Create a high-focus "cache" in `ACTIVE_TASK.md` for a specific feature or fix.

## Workflow
1. Identify the **nearest** `.context/` directory based on the user's request.
2. **Remote Freshness Check**:
   - If `[Context Root]` is inside a Git worktree with remotes, check whether the relevant branch is up to date before planning:
     - Prefer the current branch's upstream when it exists.
     - If the task, issue, or roadmap clearly names a base/target branch, also inspect that branch.
     - Otherwise use the repository default branch or the most relevant tracked branch.
   - Fetch remote refs when possible, then compare local and remote commit state.
   - If the branch is behind, diverged, missing an upstream, or remote freshness cannot be confirmed, **STOP AND DISCUSS**. Do not pull, merge, rebase, switch branches, or otherwise mutate Git state unless the user explicitly approves.
   - State the exact branch/ref checked and whether it is current before continuing.
3. Read `[Context Root]/.context/SPEC.md` and `[Context Root]/.context/ROADMAP.md`.
4. **Analyze & Challenge (Critical Step)**:
   - Identify if the goal or motivation is unclear. **STOP AND DISCUSS.**
   - **Trade-off Analysis**: Is this the right time? Is the scope appropriate?
   - Identify violations of current architecture or technical debt. **CHALLENGE AND SUGGEST.**
5. **Planning Phase (Documents Only)**:
   - **Gap Analysis**: Proactively update `SPEC.md` and `RAW_REQUIREMENTS.md` only if:
     - The task is entirely missing from these documents (ensures source of truth).
     - The task introduces new global requirements or architectural constraints.
     - *Think like an Engineer*: Evaluate if updating SPEC is truly necessary for the project's long-term health. Avoid cluttering SPEC with implementation details.
   - **Isolation**: During the planning stage, ALL modifications must be restricted to `.context` files (ACTIVE_TASK.md, SPEC.md, RAW_REQUIREMENTS.md). **DO NOT** modify any source code OR `ROADMAP.md` until explicitly authorized. `ROADMAP.md` is reserved for completed milestones.
6. **Authoritative Reconstruction of ACTIVE_TASK.md**:
   - **Structure Force**: Regardless of the current state of `ACTIVE_TASK.md`, re-create it using the mandatory structure:
     - `# ACTIVE_TASK`
     - `## Goal`: Concise one-liner.
     - `## Issue Reference`: Capture Issue links or IDs.
     - `## Implementation Details`: Capture specific functional requirements and implementation ideas.
     - `## Test Plan`: Define testing strategy (Unit/UI/Manual).
     - `## Focusing Files`: List 3-5 primary files.
     - `## Technical Context`: Extract ONLY essential snippets/constraints from SPEC.
     - `## Task Checklist`: High-density implementation steps.
7. **Output & Approval**: Display the proposed plan and wait for the user to explicitly say "Start Implementation" or approve the plan before touching any code.
8. **Hard Boundary**: **NEVER** modify source code files in the same turn as running this skill. You must stop after drafting the plan and wait for human confirmation. Proactively state: "Plan drafted in ACTIVE_TASK.md. Awaiting approval to begin coding."
