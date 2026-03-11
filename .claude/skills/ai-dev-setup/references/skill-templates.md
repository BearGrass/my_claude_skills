# Skill Templates

Generated into `.claude/skills/` for project-specific workflows.

## Placeholder Substitution

| Placeholder | Example |
|-------------|---------|
| `{DOC_DIR}` | `.docs`, `docs` |
| `{TEST_CMD}` | `go test ./...` |
| `{LINT_CMD}` | `go vet ./...` |
| `{TEST_COVER_CMD}` | `go test -coverprofile=coverage.out ./...` |
| `{RACE_FLAG}` | `-race` or empty |

## Directory Structure

```
.claude/skills/
├── plan/SKILL.md
├── dev/SKILL.md
├── dev-resume/SKILL.md
├── test/SKILL.md
├── test-resume/SKILL.md
└── sync-docs/SKILL.md
```

---

## /plan

```markdown
---
name: plan
description: Analyze requirements from BACKLOG.md, decompose into development tasks in TODO.md. Use when new requirements need technical analysis and task breakdown.
argument-hint: [requirement-id or blank for all]
disable-model-invocation: true
---

# Requirement Planning

1. Read: CLAUDE.md, {DOC_DIR}/BACKLOG.md, {DOC_DIR}/ARCHITECTURE.md, {DOC_DIR}/TODO.md, {DOC_DIR}/ROADMAP.md
2. Find "Unplanned" items (or filter by $ARGUMENTS if specified)
3. For each item, analyze: files to modify, new files needed, external deps, architecture conflicts, complexity (S/M/L), suggested order
4. **Present analysis. Wait for confirmation.**
5. After confirmation: move items to "Planned" in BACKLOG.md, create file-level tasks in TODO.md, update ARCHITECTURE.md/ROADMAP.md if affected

Rules: No code in this phase. Ask if ambiguous. State conflicts clearly.
```

## /dev

```markdown
---
name: dev
description: Develop the highest priority task from TODO.md with code, tests, and doc updates.
argument-hint: [task-id or blank for highest priority]
disable-model-invocation: true
---

# Development Workflow

1. Read: CLAUDE.md, {DOC_DIR}/TODO.md, {DOC_DIR}/ARCHITECTURE.md, {DOC_DIR}/CONVENTIONS.md
2. Find highest priority "Not started" task (or use $ARGUMENTS)
3. **Present plan: files to modify, approach. Wait for confirmation.**

Per-file loop:
1. Write code + unit tests
2. Run `{TEST_CMD} {RACE_FLAG}` on relevant package — fix until pass
3. Update docs: new package → CLAUDE.md, architecture change → ARCHITECTURE.md, user feature → README.md, new issue → TODO.md
4. **Report changes. Wait for confirmation.**

Completion: full test suite pass → lint `{LINT_CMD}` no warnings → mark done in TODO.md → move to "Completed" in BACKLOG.md
```

## /dev-resume

```markdown
---
name: dev-resume
description: Resume an interrupted development session by reading TODO.md for in-progress tasks.
disable-model-invocation: true
---

# Resume Development

1. Read: CLAUDE.md, {DOC_DIR}/TODO.md, {DOC_DIR}/BACKLOG.md
2. Find "In Progress" tasks in TODO.md
3. Report: which task, files done, what remains
4. **Wait for confirmation, then continue per /dev per-file loop rules**
```

## /test

```markdown
---
name: test
description: Systematically improve test coverage in dependency order.
argument-hint: [package-path or blank for full project]
disable-model-invocation: true
---

# Test Coverage Workflow

Phase 1 — Planning:
1. Read: CLAUDE.md, {DOC_DIR}/TEST_PLAN.md
2. Scan source files vs existing tests, run `{TEST_COVER_CMD}` for baseline
3. Update TEST_PLAN.md: utilities → core → interfaces → integration order
4. **Present plan. Wait for confirmation.**

Phase 2 — Execution (per file):
1. Write test file per conventions
2. Run `{TEST_CMD} {RACE_FLAG}` — fix until pass
3. Update TEST_PLAN.md status, report
4. **Wait for confirmation before next file**
After each batch: run `{TEST_COVER_CMD}`, report delta, **wait for instruction**

Phase 3 — Wrap-up: full suite pass, final coverage report, update TEST_PLAN.md + CLAUDE.md + TODO.md
```

## /test-resume

```markdown
---
name: test-resume
description: Resume an interrupted test coverage session by reading TEST_PLAN.md for pending items.
disable-model-invocation: true
---

# Resume Test Workflow

1. Read: CLAUDE.md, {DOC_DIR}/TEST_PLAN.md, {DOC_DIR}/TODO.md
2. Find first "Not started" or "In Progress" entry
3. Report: X completed, Y remaining, current batch
4. **Wait for confirmation, then continue per /test Phase 2 rules**
```

## /sync-docs

```markdown
---
name: sync-docs
description: Check docs against code and fix inconsistencies.
disable-model-invocation: true
---

# Documentation Sync

1. Read CLAUDE.md "Document Self-Maintenance Rules" section
2. Run `git diff --name-only` (or `HEAD~5` if no staged changes)
3. Check each modified file against trigger rules in CLAUDE.md
4. List discrepancies: doc, section, current state, correct state
5. **Wait for confirmation**
6. Apply fixes, report updates
```
