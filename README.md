# Spara plugins

ChatGPT / Codex plugin marketplace for Spara. Import this repository as a GitHub marketplace in Workspace settings so staff get Spara plugins with daily sync.

## Marketplace

Catalog: [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json)

| Plugin | Path | Purpose |
|---|---|---|
| Spara Aegis | [`plugins/spara-aegis`](plugins/spara-aegis) | Client QuickBooks and bookkeeping work via `https://sparaaegis.com/mcp` |

Add another plugin by creating `plugins/<name>/` with a `plugin.json`, then appending an entry to the marketplace catalog.

## Import into ChatGPT

1. Open [Workspace settings → Plugins](https://chatgpt.com/admin/plugins) (admin).
2. **Add → Import marketplace**.
3. Repository: `https://github.com/Spara-LLC/spara-plugins`
4. Path: leave blank.
5. Branch: `main` (or leave empty for the default).
6. Import, then set installation policy for each plugin. Members still connect MCP / apps themselves.

Guide: [Importing and syncing plugin marketplaces from GitHub](https://help.openai.com/en/articles/20001504-importing-and-syncing-plugin-marketplaces-from-github)

## Aegis

Connect MCP at `https://sparaaegis.com/mcp` with the staff member's Spara Microsoft account. Sync from this repo updates plugin skills and packaging; it does not grant QuickBooks access.
