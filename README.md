# hotschmoe-setup

Scripts to commission new machines with all required software.

## Windows

### Work Machine (`windows/work-setup.ps1`)

Installs applications for work/engineering:

| Application | Installation Method |
|-------------|---------------------|
| Google Chrome | winget |
| 7-Zip | winget |
| ENERCALC | winget |
| PDF-XChange Editor | winget |
| ArchiCAD | **Manual** ([download](https://graphisoft.com/downloads)) |

**Usage:**
```powershell
# Run as Administrator (recommended)
.\windows\work-setup.ps1
```

---

### Developer Machine (`windows/dev-setup.ps1`)

Installs development tools and AI coding assistants:

| Application | Installation Method |
|-------------|---------------------|
| Git | winget |
| Bun | winget |
| Node.js LTS | winget |
| Go | winget |
| Python 3.12 | winget |
| Zig | winget |
| ZLS (Zig Language Server) | winget |
| Cursor | winget |
| GitHub Desktop | winget |
| LazyGit | winget |
| Claude Code | native (`irm https://claude.ai/install.ps1 \| iex`) |
| Beads (bd/bv) | native (`irm .../install.ps1 \| iex`) |
| Gemini CLI | bun |
| Antigravity IDE | **Manual** ([download](https://antigravity.google)) |

**Usage:**
```powershell
# Run as Administrator (recommended)
.\windows\dev-setup.ps1
```

**Post-Installation:**
1. Restart your terminal for PATH changes to take effect
2. If Claude Code or Beads failed during install, run manually:
   ```powershell
   irm https://claude.ai/install.ps1 | iex
   irm https://raw.githubusercontent.com/steveyegge/beads/main/install.ps1 | iex
   ```
3. Run verification commands:
   ```powershell
   git --version
   bun --version
   node --version
   go version
   python --version
   zig version
   zls --version
   claude --version
   lazygit --version
   bd version
   ```

---

## Notes

- **Why not WSL2?** Zig v0.15.0+ fails to build on WSL, so we stick to native Windows/PowerShell for development setup.
- **Why Bun?** Bun is faster than npm for package installation and is used for global packages like Gemini CLI.
- **winget required** - These scripts require the Windows Package Manager (winget). Install via Microsoft Store if not present.
- Some packages may require a terminal restart mid-script to refresh PATH variables.
