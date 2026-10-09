# Spara plugins

ChatGPT / Codex plugin marketplace for Spara. Import this repository as a GitHub marketplace in Workspace settings so staff get Spara plugins with daily sync.

## Marketplace

Catalog: [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json)

| Plugin | Path | Purpose |
|---|---|---|
| Spara Aegis | [`plugins/spara-aegis`](plugins/spara-aegis) | Client QuickBooks and bookkeeping work via the workspace app for `https://sparaaegis.com/mcp` |

Each plugin uses the Codex marketplace layout:

- `.codex-plugin/plugin.json`
- `.app.json` when the plugin requires a workspace-registered app (works in the browser and the desktop app)
- `skills/` and `assets/` as needed

Do not add `.mcp.json`, `mcp.json`, or an `mcpServers` field. ChatGPT marks any imported plugin that declares MCP servers as **Desktop only**, including a remote HTTPS server. An `.app.json` file does not clear that label while an MCP declaration is still present.

Add another plugin by creating `plugins/<name>/`, then appending an entry to the marketplace catalog.

## Import into ChatGPT

1. Open [Workspace settings → Plugins](https://chatgpt.com/admin/plugins) (admin).
2. **Add → Import marketplace**.
3. Repository: `https://github.com/Spara-LLC/spara-plugins`
4. Path: leave blank.
5. Branch: `main` (or leave empty for the default).
6. Import, then set installation policy for each plugin. Enable the required app for the roles that should use it. Members still sign in to that app themselves.

Guide: [Importing and syncing plugin marketplaces from GitHub](https://help.openai.com/en/articles/20001504-importing-and-syncing-plugin-marketplaces-from-github)

## Aegis

Spara Aegis depends on a ChatGPT workspace app for `https://sparaaegis.com/mcp`. Staff sign in with their Spara Microsoft account. Sync from this repo updates plugin skills and packaging; it does not create the app or grant QuickBooks access.

The plugin points at that app from [`plugins/spara-aegis/.app.json`](plugins/spara-aegis/.app.json), and `.codex-plugin/plugin.json` sets `"apps": "./.app.json"`. The checked-in id is the placeholder **`asdk_app_REPLACE_ME`**. Replace it with the real app id before merging. ChatGPT cannot resolve the required app until that id is a registered `asdk_app_…`, `connector_…`, or `templated_apps_…` id.

### Register the custom app

1. In ChatGPT, open **Plugins**, select the plus button, then **Add custom MCP server**.
2. Name it Spara Aegis. Under Connection, enter the public MCP URL `https://sparaaegis.com/mcp` and complete authentication. The server expects OAuth for the staff member's Spara Microsoft account.
3. Review the risk warning, select **I understand and want to continue**, then **Create as a plugin**.
4. Copy the technical id from the browser URL. It looks like `plugin_asdk_app_…`. The value for `.app.json` is the app id: drop the `plugin_` prefix so it starts with `asdk_app_`. Use the app id, not a `plugin_…` id.
5. Paste that id into `plugins/spara-aegis/.app.json` in place of `asdk_app_REPLACE_ME`.
6. After the marketplace syncs, open the imported Spara Aegis plugin and enable the required app for the roles that should use it.

Schema reference: [Reference an existing app with `.app.json`](https://learn.chatgpt.com/docs/enterprise/plugin-management).
