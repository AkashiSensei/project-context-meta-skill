---
name: project-context-meta-skill
description: Project context management meta-skill. Bootstrap and maintain AI-native engineering environments with decoupled components.
---

# Skill: Project Context Meta-Skill

A **Meta-Tool** for bootstrapping and maintaining AI-native engineering environments. Provides three independent components that can be installed separately or in any combination.

## 1. Core Capabilities

### Context System (Project-Level)
Creates structured memory and governance files in `.context/`:
- **README.md**: Directory structure and governance rules
- **RAW_REQUIREMENTS.md**: User's raw ideas and feature requests
- **SPEC.md**: Technical specifications (What & Why)
- **ROADMAP.md**: Milestones and decision log
- **ACTIVE_TASK.md**: Current execution focus

### Skill Template (Tool-Level)
Standardized workflow skills:
- **sync-spec**: Synchronize specifications
- **start-task**: Begin new tasks
- **archive-task**: Complete and archive tasks
- **commit-helper**: Commit task-relevant changes using project commit style

### Prompts (Behavior-Level)
Defines AI behavior and interaction patterns:
- **critical-thinking.md**: Critical thinking engineer persona

## 2. Design Philosophy

**Decoupled Components**: Each part is independent:
- Install all three ✓
- Install only Context System ✓
- Install only Skills ✓
- Install only Prompts ✓
- Any combination ✓

**Proximity Rule**: Always use the nearest `.context/` relative to current file.

## 3. Installation

When this skill runs, it installs components to the target project:

| Component | Default Location |
|-----------|-----------------|
| Context System | `.context/` |
| Skills | `.cursor/skills/` (or equivalent tool-specific path) |
| Prompts | `.context/prompts/` |

**Note**: Actual installation location is determined by the tool executing this skill.

## 4. Sync & Update Logic

Perform a **Safe Sync** with these priorities:

1. **Context System (User-Owned)**:
   - Files in `.context/` are **user-owned**
   - **NEVER** overwrite existing files
   - Create ONLY if missing (`README.md`, `RAW_REQUIREMENTS.md`, `SPEC.md`, `ROADMAP.md`)
   - **Note**: `ACTIVE_TASK.md` is managed by `start-task` skill, do not create during installation.

2. **Skills (Logic-Owned)**:
   - Analyze diff between local and global versions
   - Identify project-specific customizations vs. global updates
   - Merge changes, preserve local customizations
   - **Request user confirmation** before applying

3. **Prompts (User-Selectable)**:
   - List available prompts
   - User selects which to install
   - Install to `.context/prompts/`
   - Never overwrite existing prompts

## 5. Usage

User can request:
- "Install context system" → Creates `.context/` structure
- "Install skills" → Adds workflow skills
- "Install prompts" → Adds behavior prompts
- "Install everything" → All three components

Components are independent—install only what you need.

[See component directories for source files: `context-system/`, `skill-template/`, `prompts/`]
