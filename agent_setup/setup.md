# Agent Setup Guide

Instructions for setting up AI coding agents and their supporting tools.

---

## Beads Installation (Windows)

### 1. Install Beads CLI

```powershell
irm https://raw.githubusercontent.com/steveyegge/beads/main/install.ps1 | iex
```

### 2. Install Beads Viewer

```powershell
go install github.com/Dicklesworthstone/beads_viewer/cmd/bv@latest
```

### 3. Initialize Beads

```powershell
bd init
bd quickstart
bd --doctor
bd setup claude
```
```bash
# optional plugins In Claude Code
/plugin marketplace add steveyegge/beads
/plugin install beads
# Restart Claude Code
```

---

## Claude Code Subagents

Subagents are defined in `.claude/agents/` using Markdown files with YAML frontmatter.

### Directory Structure

```
.claude/
  agents/
    coder-sonnet.md
    gemini-analyzer.md
```

### File Format

Each subagent file uses this structure:

```markdown
---
name: agent-name
description: Brief description of the agent's purpose.
model: sonnet
tools:
  - Bash
  - Read
  - Write
  - Edit
---

System prompt and instructions for the agent go here.
```

### Example Files

See the example subagent definitions in `.claude/agents/`:

- **coder-sonnet.md** - Fast code implementation agent
- **gemini-analyzer.md** - Large-context analysis via Gemini CLI

---

## Notes

- Primary Claude (Opus if selected) spawns subagents as needed
- Project-specific agents (`.claude/agents/`) take precedence over user-level agents (`~/.claude/agents/`)