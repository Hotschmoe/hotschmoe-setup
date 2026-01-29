# Hotschmoe Agent Injections - Usage Guide

## One-Liner

```bash
curl -sL https://raw.githubusercontent.com/Hotschmoe/hotschmoe-setup/master/real/haj.sh | bash
```

Or specify a different target file:

```bash
curl -sL https://raw.githubusercontent.com/Hotschmoe/hotschmoe-setup/master/real/haj.sh | bash -s -- ./path/to/CLAUDE.md
```

## How It Works

**If CLAUDE.md doesn't exist:**
- Creates one with standard sections (rule-1, code-discipline, dev-philosophy, etc.)
- Adds a placeholder for project-specific content

**If CLAUDE.md exists:**
- Finds all `<!-- BEGIN:section-name -->` markers
- Updates only those sections from source
- Project-specific content (anything outside markers) is preserved

```
Your CLAUDE.md:
+------------------------------------------+
| <!-- BEGIN:rule-1-no-delete -->          |  <-- Updated from source
| (content)                                |
| <!-- END:rule-1-no-delete -->            |
|                                          |
| ## My Project-Specific Stuff             |  <-- Untouched
| (your custom content here)               |
|                                          |
| <!-- BEGIN:dev-philosophy -->            |  <-- Updated from source
| (content)                                |
| <!-- END:dev-philosophy -->              |
+------------------------------------------+
```

## Setting Up a New Project

Just run the one-liner in your project directory:

```bash
curl -sL https://raw.githubusercontent.com/Hotschmoe/hotschmoe-setup/master/real/haj.sh | bash
```

This creates CLAUDE.md with these default sections:
- `header` - title + love message
- `rule-1-no-delete` - absolute no-delete rule
- `irreversible-actions` - git/filesystem safety
- `code-discipline` - editing discipline
- `no-legacy` - full migrations only
- `dev-philosophy` - make it work/right/fast
- `testing-philosophy` - tests as diagnostics
- `footer` - closing message

Plus a placeholder for project-specific content.

**Then:** Edit the file to add your project's toolchain, architecture, and workflows in the project-specific section.

## Available Sections

| Section | Description |
|---------|-------------|
| `header` | CLAUDE.md title + love message |
| `rule-1-no-delete` | Absolute no-delete rule |
| `irreversible-actions` | Git/filesystem safety rules |
| `semver` | Version update guidelines |
| `code-discipline` | Editing discipline (no bulk mods, no emojis) |
| `no-legacy` | Full migrations only policy |
| `dev-philosophy` | Make it work/right/fast |
| `testing-philosophy` | Tests as diagnostics, not verdicts |
| `code-simplifier` | Post-session cleanup agent |
| `claude-agents` | Agent documentation template |
| `claude-skills` | Skills documentation template |
| `project-language-template` | Placeholder for language-specific content |
| `footer` | Closing message |

## Customizing Sections

To remove a section: delete both markers and content between them, then run the one-liner (it won't re-add).

To add a section: copy empty markers into your file, then run the one-liner to populate:

```markdown
<!-- BEGIN:semver -->
<!-- END:semver -->

<!-- BEGIN:claude-agents -->
<!-- END:claude-agents -->
```

## Tips

- Only sections with markers in your file get updated
- Add/remove sections by adding/removing marker pairs
- Project-specific content goes outside markers (or replaces `project-language-template`)
- Run the one-liner periodically to pick up philosophy updates
