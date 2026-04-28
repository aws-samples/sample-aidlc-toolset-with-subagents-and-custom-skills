# Code Conflict Merge Strategy

## Overview

Code conflicts occur when a shared/common unit's code is modified while other developers depend on or extend it. Unlike state files, these require semantic understanding.

## Analysis Process

For each conflicted code file:

### 1. Read the Conflict

Parse the file and extract conflict regions:
- `<<<<<<< HEAD` (ours) — changes from the current branch
- `=======` — separator
- `>>>>>>> {branch}` (theirs) — changes from the incoming branch

### 2. Identify Which Units Are Involved

Determine which unit(s) each side of the conflict belongs to:
- Check git log for the commits that touched this file
- Cross-reference with `aidlc-docs/construction/plans/` to find which unit's code generation plan includes this file
- If the file is in a shared/common unit, identify which dependent units are affected

### 3. Load Design Context

Read the relevant design artifacts to understand intent:
- `aidlc-docs/construction/{unit-name}/functional-design/` — business logic intent
- `aidlc-docs/construction/{unit-name}/code/` — code generation summary
- `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md` — what was planned

### 4. Classify the Conflict

**Additive (both can coexist):**
- New methods/functions added by different units to the same file
- New imports added by both sides
- New configuration entries

→ Resolution: include both changes. Order logically (e.g., alphabetical imports, grouped methods).

**Overlapping modification:**
- Same function/method modified by both sides
- Same configuration value changed differently
- Same interface/contract changed incompatibly

→ Resolution: requires user decision. Present both versions with context from design artifacts.

**Dependency conflict:**
- Shared unit's API changed, breaking a dependent unit's usage
- Shared type/model modified, affecting consumers

→ Resolution: the shared unit's change is typically authoritative. Adapt the dependent unit's code to match. Present to user for confirmation.

### 5. Present Resolution

For each conflict, show the user:

```
## Conflict: {file-path}

**Type**: {Additive | Overlapping | Dependency}
**Units involved**: {unit-a}, {unit-b}

### Ours (HEAD):
{code from our side}

### Theirs ({branch}):
{code from their side}

### Proposed resolution:
{merged code}

### Rationale:
{why this resolution, referencing design artifacts}
```

Wait for user approval before applying.

## Common Patterns

### Shared model/type changes
When a shared data model is extended by multiple units:
- If both add new fields → merge both fields
- If both modify the same field → flag for user review
- Check `domain-entities.md` from both units for intent

### API endpoint additions
When multiple units add endpoints to the same router/controller:
- Typically additive — include all endpoints
- Check for route conflicts (same path, different handlers)

### Configuration file changes
When `package.json`, `pom.xml`, `build.gradle`, etc. conflict:
- Dependencies: union of both sides, flag version conflicts
- Scripts/tasks: include both, flag name collisions
- Settings: flag any differing values for user review

### Infrastructure code (CDK/CloudFormation)
When infrastructure definitions conflict:
- New resources from different units → additive, include both
- Same resource modified → flag for user review, reference `infrastructure-design.md`
- Stack dependencies changed → careful review needed
