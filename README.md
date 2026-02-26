# RedJay Plugin Marketplace

Federated plugin marketplace for Claude Code. This repository is the central registry — individual plugins live in their own repositories and are referenced here by URL or local path.

## Registered Plugins

| Plugin | Version | Category | Source |
|--------|---------|----------|--------|
| [claude-code-telemetry](https://github.com/JoshuaRamirez/claude-code-telemetry) | 2.1.0 | observability | GitHub |
| [ado-work-items](https://github.com/JoshuaRamirez/ms-ado-az-claude-code-plugin) | 1.0.0 | integrations | GitHub |
| domain-driven-ui | 1.0.0 | architecture | local |
| spec-vault-toolkit | 1.0.0 | documentation | local |

## Structure

```
.claude-plugin/
  marketplace.json    # Plugin registry manifest
.github/
  workflows/
    validate.yml      # CI: JSON validation and structure checks
```

The manifest at `.claude-plugin/marketplace.json` is the single source of truth for plugin discovery.

## Adding a Plugin

Add an entry to the `plugins` array in `marketplace.json`:

```json
{
  "name": "my-plugin",
  "source": { "source": "url", "url": "https://github.com/user/repo.git" },
  "description": "What the plugin does",
  "version": "1.0.0",
  "category": "category-name"
}
```

## License

MIT
