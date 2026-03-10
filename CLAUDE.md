# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This is the **RedJay plugin marketplace** — a federated manifest that registers publicly installable Claude Code plugins for discovery and installation. The marketplace itself contains no plugin code; it is a thin orchestration layer.

For personal and vault-embedded plugins, see the [RedJay-Private marketplace](https://github.com/JoshuaRamirez/private-claude-code-plugins) (private repo).

- **Manifest**: `.claude-plugin/marketplace.json` — the sole authoritative file listing all published plugins
- **Symlinks**: `telemetry/` and `ado/` are symlinks to the actual plugin locations on disk (gitignored). They exist for local development convenience only.

## Registered Plugins

| Plugin | Local Symlink | Actual Repo | Language | Category |
|--------|--------------|-------------|----------|----------|
| `ado-work-items` | `ado/` → `C:\Source\ms-ado-az-claude-code-plugin` | github.com/JoshuaRamirez/ms-ado-az-claude-code-plugin | Markdown-only (az CLI) | integrations |
| `roslyn-mcp` | — | github.com/JoshuaRamirez/RoslynMcpServer | C# (.NET 8, Roslyn) | languages |

## Architecture

```
claude-code-plugins/              ← this repo (marketplace hub)
├── .claude-plugin/
│   └── marketplace.json          ← plugin registry manifest
├── telemetry/ → (symlink)        ← gitignored
└── ado/ → (symlink)              ← gitignored

ms-ado-az-claude-code-plugin/     ← separate repo (markdown-only plugin)
├── commands/                     ← slash commands organized by domain
├── skills/                       ← reference docs (WIQL, fields, bulk ops)
├── agents/work-items.md          ← multi-step work item assistant
└── tests/verified-commands.md    ← manual test log

RoslynMcpServer/                  ← separate repo (C# MCP server)
├── RoslynMcpServer/              ← .NET 8 MCP server (41 Roslyn tools)
├── RoslynMcpServer.Tests/        ← unit tests
└── .claude-plugin/plugin.json    ← plugin manifest
```

## Working in This Repo

Changes to the marketplace manifest itself are rare. Most work happens in the individual plugin repos. When working here:

- Edit `.claude-plugin/marketplace.json` to add, remove, or update plugin entries
- Validate JSON schema conformance against the `$schema` URL in the manifest
- Bump version numbers in the manifest when publishing plugin updates

## Working in the ADO Plugin

The ADO plugin is markdown-only — no build, no tests, no dependencies. It uses `az devops` CLI commands.

- **Commands** (`commands/`): Slash command definitions with YAML frontmatter (`description`, `allowed-tools`)
- **Skills** (`skills/`): Reference documentation for complex scenarios (WIQL queries, bulk operations, field reference)
- **Agents** (`agents/`): Multi-step assistants with broader tool access

Prerequisites: Azure CLI with `azure-devops` extension, authenticated via `az login`.

## Conventions

- All plugins in this marketplace are standalone git repositories with URL sources
- Symlinks in this repo are for local development only and must stay in `.gitignore`
- The marketplace manifest `version` field for each plugin must match the plugin's own version
- Telemetry plugin: Python 3.11+, ruff for linting, pytest for testing, hatch for building
- ADO plugin: Pure markdown, no runtime dependencies beyond `az` CLI
