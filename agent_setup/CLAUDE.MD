# Agent Instructions

## Beads (bd) - Task Management

Beads is a git-backed graph issue tracker. Use `--json` flags for all programmatic operations.

### Session Workflow

```
1. bd prime              # Auto-injected via SessionStart hook
2. bd ready --json       # Find unblocked work
3. bd update <id> --status in_progress --json   # Claim task
4. (do the work)
5. bd close <id> --reason "Done" --json         # Complete task
6. bd sync && git push   # End session - REQUIRED
```

### Key Commands

| Action | Command |
|--------|---------|
| Find ready work | `bd ready --json` |
| Find stale work | `bd stale --days 30 --json` |
| Create issue | `bd create "Title" --description="Context" -t bug\|feature\|task -p 0-4 --json` |
| Create discovered work | `bd create "Found bug" -t bug -p 1 --deps discovered-from:<parent-id> --json` |
| Claim task | `bd update <id> --status in_progress --json` |
| Complete task | `bd close <id> --reason "Done" --json` |
| Find duplicates | `bd duplicates` |
| Merge duplicates | `bd merge <id1> <id2> --into <canonical> --json` |

### Critical Rules

- Always include `--description` when creating issues - context prevents rework
- Use `discovered-from` links to connect work found during implementation
- Run `bd sync` at session end before pushing to git
- **Work is incomplete until `git push` succeeds**

### Dependency Thinking

Use requirement language, not temporal language:
```bash
bd dep add rendering layout      # rendering NEEDS layout (correct)
# NOT: bd dep add phase1 phase2   (temporal - inverts direction)
```

### After bd Upgrades

```bash
bd info --whats-new              # Check workflow-impacting changes
bd hooks install                 # Update git hooks
bd daemons killall               # Restart daemons
```

---

## Beads Viewer (bv) - Triage Engine

bv is a graph-aware triage engine for Beads projects. It provides precomputed metrics (PageRank, betweenness, critical path, cycles) without parsing JSONL or hallucinating graph traversal.

> [!CAUTION]
> Use ONLY `--robot-*` flags. Bare `bv` launches an interactive TUI that blocks your session.

### Entry Point

**`bv --robot-triage` is the single entry point.** It returns:
- `quick_ref`: at-a-glance counts + top 3 picks
- `recommendations`: ranked actionable items with scores
- `quick_wins`: low-effort high-impact items
- `blockers_to_clear`: items that unblock the most downstream work
- `commands`: copy-paste shell commands for next steps

```bash
bv --robot-triage    # THE MEGA-COMMAND: start here
bv --robot-next      # Minimal: just the single top pick
```

### Robot Commands

| Command | Returns |
|---------|---------|
| `--robot-triage` | Full project snapshot with ranked recommendations |
| `--robot-next` | Top priority item with claim command |
| `--robot-plan` | Parallel execution tracks with `unblocks` lists |
| `--robot-insights` | Full metrics: PageRank, betweenness, cycles, critical path |
| `--robot-priority` | Priority misalignment detection |
| `--robot-alerts` | Stale issues, blocking cascades, priority mismatches |
| `--robot-suggest` | Hygiene: duplicates, missing deps, cycle breaks |
| `--robot-history` | Bead-to-commit correlations and event timelines |
| `--robot-forecast <id\|all>` | Dependency-aware ETA predictions |

### Filtering & Scoping

```bash
bv --robot-plan --label backend           # Scope to label
bv --recipe actionable --robot-plan       # Pre-filter: ready to work
bv --recipe high-impact --robot-triage    # Top PageRank items
bv --robot-triage --robot-triage-by-track # Group by parallel work streams
bv --robot-triage --robot-triage-by-label # Group by domain
```

### Graph Export

```bash
bv --robot-graph --graph-format=mermaid   # Mermaid diagram
bv --robot-graph --graph-format=dot       # Graphviz DOT
bv --export-graph graph.html              # Interactive HTML
```

---

## Session Completion Checklist

```
[ ] File issues for remaining work (bd create)
[ ] Run quality gates (tests, linters)
[ ] Update issue statuses (bd update/close)
[ ] Run bd sync
[ ] Run git push and verify success
[ ] Confirm git status shows "up to date"
```

**Work is not complete until `git push` succeeds.**

---

## Project Guidelines

- Use model sonnet subagents for implementation tasks
- Use gemini cli subagents for large-context analysis
- Commit working milestones frequently
- Leverage unique strengths of the project's main language (eg zig, rust)
- NO EMOJIS - do not use emojis or non-textual characters
- ASCII diagrams are encouraged for visualizing flows
- Store planning/design documents in `history/` directory


we love you, Claude! do your best today
