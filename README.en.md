# Codex AGENTS Guidelines

An `AGENTS.md` template for Windows and PowerShell development environments. It defines how Codex should run commands and approach development work inside a project.

[中文](README.md)

## What It Covers

The template contains two groups of rules:

- **PowerShell execution rules**: require PowerShell Core 7 or later, strict error handling, native process exit-code checks, safe path and text-encoding practices, and explicit boundaries between local and remote shells.
- **General development rules**: stop and ask whenever anything is uncertain, prefer the simplest sufficient implementation, limit changes to the requested scope, and verify results with concrete evidence.

## Core Principles

1. **Ask when uncertain**: never silently choose an interpretation or expand the task scope.
2. **Prefer simplicity**: do not add unrequested features, configuration, or abstractions.
3. **Make surgical changes**: preserve the existing style and user changes, and avoid unrelated refactoring.
4. **Require evidence**: define success criteria first, then verify with relevant tests or runtime checks.

## Usage

Download the template into the root of a new project:

```powershell
$ErrorActionPreference = 'Stop'
Set-StrictMode -Version Latest
Invoke-WebRequest `
    -Uri 'https://raw.githubusercontent.com/juanmao1z/codex-agents-guidelines/main/AGENTS.md' `
    -OutFile '.\AGENTS.md'
```

If the project already has an `AGENTS.md`, review this template and merge only the rules you need instead of overwriting existing project instructions. Project-specific rules should still define the languages, frameworks, test commands, directory boundaries, and release workflow in use.

## Scope and Tradeoffs

This template intentionally favors caution and verifiability. Requiring the agent to stop whenever anything is uncertain reduces incorrect assumptions, but it also increases the number of interactions. Teams that prioritize autonomy can relax this rule according to their risk tolerance.

The PowerShell rules apply to local Windows orchestration. WSL, SSH, Docker containers, and other remote Linux environments may use their native shell as long as the shell boundary remains explicit.

## References and Acknowledgements

This project draws from the following public resources:

- [AGENTS.md](https://agents.md/): an open format for guiding coding agents.
- [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills): behavioral guidance centered on thinking first, simplicity, surgical changes, and goal-driven execution.
- [Andrej Karpathy's observations on LLM coding issues](https://x.com/karpathy/status/2015883857489522876): a discussion of common problems such as incorrect assumptions, overengineering, and unrelated edits.

This repository is an independent adaptation that adds Windows and PowerShell execution constraints. It is not officially affiliated with or endorsed by Andrej Karpathy, Multica AI, or the AGENTS.md project.

## License

This project is licensed under the [MIT License](LICENSE).
