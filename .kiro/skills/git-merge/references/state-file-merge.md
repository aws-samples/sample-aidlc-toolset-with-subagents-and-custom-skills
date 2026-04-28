# State File Merge Strategy

## aidlc-state.md

This file tracks stage progress per unit. In multi-user scenarios, each developer checks off their own unit's stages.

### Merge Logic

1. Parse both sides (ours and theirs) of the conflict
2. Identify unit-specific sections — look for unit names under `### 🟢 CONSTRUCTION PHASE` or per-unit stage entries
3. For each unit's checkbox lines (`- [x]` or `- [ ]`):
   - If one side has `[x]` and the other has `[ ]` for the same line → use `[x]` (progress only moves forward)
   - If both sides have `[x]` → keep `[x]`
   - If both sides have `[ ]` → keep `[ ]`
4. For `## Current Status` section — use the most recent timestamp and update the current stage to reflect combined progress
5. For `## Extension Configuration` — should be identical on both sides (set during INCEPTION). If different, flag for user review

### Conflict Marker Pattern

```
<<<<<<< HEAD
- [x] Code Generation - COMPLETE (unit-a)
=======
- [x] Code Generation - COMPLETE (unit-b)
>>>>>>> branch
```

Resolution: keep both lines — they describe different units.

### Edge Cases

- **Same unit modified by two developers** — this shouldn't happen in normal workflow (units are assigned to individuals). Flag for user review.
- **Project-level fields changed** (Project Type, Start Date) — use HEAD version, these shouldn't change during CONSTRUCTION.

## audit.md

This file is an append-only log of all interactions with timestamps.

### Merge Logic

1. Extract all log entries from both sides — each entry is delimited by `---` separators
2. Parse the `**Timestamp**:` field from each entry (ISO 8601 format)
3. Deduplicate — if identical entries exist on both sides (same timestamp + same content), keep one copy
4. Sort all entries chronologically by timestamp
5. Write the merged result

### Conflict Marker Pattern

Typically the conflict spans large sections since both developers appended to the end of the file. Resolution: extract entries from both sides, sort by timestamp, concatenate.

### Edge Cases

- **Missing timestamps** — place entries without timestamps at the end, grouped by which branch they came from
- **Identical timestamps** — preserve both entries, order by branch (HEAD first, then theirs)
