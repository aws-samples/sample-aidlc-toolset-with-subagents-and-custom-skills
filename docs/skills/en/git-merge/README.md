# Git Merge Skill for AI-DLC

A skill that resolves git merge conflicts arising when multiple developers work on different units in parallel during an AI-DLC project.

## When to Use

After the INCEPTION phase completes and units are assigned, multiple developers run the CONSTRUCTION phase on their own machines. When they push/merge back, conflicts occur. This skill handles them.

### Conflict Types

**1. AI-DLC State File Conflicts**

Each developer records their unit's progress in the same files:
- `aidlc-docs/aidlc-state.md` — per-unit stage progress checkboxes
- `aidlc-docs/audit.md` — work history log

These are not logical conflicts (each records a different unit). The skill merges them automatically.

**2. Common Code Conflicts**

When a shared unit (e.g., common library) is modified by multiple developers:
- Different functions/types added to a shared module
- Different dependencies added to the same config file
- Compatibility issues from shared API changes

## Usage

### 1. Typical Workflow

```bash
# Developer A pushes after completing unit-a
git push origin main

# Developer B pulls after completing unit-b → conflict
git pull origin main
# CONFLICT!
```

### 2. Invoke the Skill

With conflicts present, ask the aidlc-main agent:

```
Resolve the merge conflicts
```

or:

```
I have git merge conflicts. Please resolve them.
```

### 3. What the Skill Does

1. Runs `git diff --name-only --diff-filter=U` to list conflicted files
2. Classifies each file as **state file** or **code file**
3. State files are merged automatically:
   - `aidlc-state.md` — reflects progress from both units
   - `audit.md` — merges entries sorted by timestamp
4. Code files are analyzed and resolution is proposed:
   - **Additive** (both additions can coexist) → auto-merge proposed
   - **Overlapping** (same code modified) → side-by-side comparison, user chooses
   - **Dependency** (shared API changed) → shared unit takes priority, dependent unit adapts
5. Validates all conflicts are resolved

## Example Scenario

```
Project: e-commerce platform
├── unit-a (User Service) — Developer A
├── unit-b (Order Service) — Developer B
└── shared (Common Utils) — modified by both

Developer A: adds generateId() to shared/utils.ts, adds bcrypt dependency
Developer B: adds formatCurrency() to shared/utils.ts, adds stripe dependency

→ Conflicts in utils.ts, package.json, aidlc-state.md, audit.md
→ Skill resolves all 4 files
```
