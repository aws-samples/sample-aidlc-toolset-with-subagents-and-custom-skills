---
name: git-merge
description: Resolve git merge conflicts in AI-DLC multi-user projects. Use this skill when users mention merge conflicts, git merge, pull conflicts, or when multiple developers are working on different AI-DLC units in parallel and need to combine their work. Covers both AI-DLC state file conflicts (aidlc-state.md, audit.md) and application code conflicts from shared/common units.
---

# Git Merge Conflict Resolver for AI-DLC Projects

Resolve git merge conflicts that arise when multiple developers work on different AI-DLC units in parallel during the CONSTRUCTION phase.

## When This Happens

After INCEPTION phase completes, the project is split into units. Multiple developers clone the repo and each runs CONSTRUCTION on their assigned unit. When they push/merge back:

1. **State file conflicts** — `aidlc-docs/aidlc-state.md` and `aidlc-docs/audit.md` are modified by every developer since each tracks their own unit's progress
2. **Code conflicts** — shared/common units modified by one developer conflict with another developer's changes or dependencies on that shared code

## Conflict Resolution Workflow

### Step 1: Detect and Classify Conflicts

Run `git status` to identify conflicted files, then classify each:

```bash
git diff --name-only --diff-filter=U
```

Classify each conflicted file into:
- **State file** — matches `aidlc-docs/aidlc-state.md` or `aidlc-docs/audit.md`
- **Code file** — everything else (application code, config, infrastructure)

### Step 2: Resolve State File Conflicts

Read `references/state-file-merge.md` for the detailed merge strategy.

State files are logically non-conflicting — each developer wrote about their own unit. The strategy is:
- `aidlc-state.md` — merge unit progress sections, preserving both sides' checkbox states
- `audit.md` — combine entries chronologically by timestamp

These can be resolved automatically without user confirmation.

### Step 3: Resolve Code Conflicts

Read `references/code-conflict-merge.md` for the detailed analysis strategy.

Code conflicts require understanding intent. For each conflicted code file:
1. Read the conflict markers to understand both sides
2. Load relevant design artifacts from `aidlc-docs/construction/{unit-name}/` for context
3. Determine if changes are independent (both can coexist) or truly conflicting (one must win)
4. Present the analysis and proposed resolution to the user
5. Apply only after user approval

### Step 4: Verify and Complete

After all conflicts are resolved:

```bash
git diff --name-only --diff-filter=U
```

Confirm zero remaining conflicts, then inform the user the merge is ready to commit.
