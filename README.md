# Spara plugins

Plugin marketplaces for Spara. ChatGPT / Codex and Claude Code each have their own catalog in this repository. Both point staff at the Aegis server `https://sparaaegis.com/mcp`. Staff sign in with their Spara Microsoft account.

| Host | Catalog | Plugin path |
|---|---|---|
| ChatGPT / Codex | [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json) | [`plugins/spara-aegis`](plugins/spara-aegis) |
| Claude Code | [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) | [`plugins/spara-aegis-claude`](plugins/spara-aegis-claude) |

The Claude plugin is a separate directory. ChatGPT marks any imported plugin that declares MCP servers as **Desktop only**, including a remote HTTPS server. Keep `.mcp.json` in `plugins/spara-aegis-claude` only. Do not add `.mcp.json`, `mcp.json`, or an `mcpServers` field under `plugins/spara-aegis`. An `.app.json` file does not clear that label while an MCP declaration is still present.

## ChatGPT / Codex

Import this repository as a GitHub marketplace in Workspace settings so staff get Spara plugins with daily sync.

| Plugin | Path | Purpose |
|---|---|---|
| Spara Aegis | [`plugins/spara-aegis`](plugins/spara-aegis) | Client QuickBooks and bookkeeping work via the workspace app for `https://sparaaegis.com/mcp` |

Each ChatGPT plugin uses the Codex marketplace layout:

- `.codex-plugin/plugin.json`
- `.app.json` when the plugin requires a workspace-registered app (works in the browser and the desktop app)
- `skills/` and `assets/` as needed

Add another ChatGPT plugin by creating `plugins/<name>/`, then appending an entry to [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json).

### Import into ChatGPT

1. Open [Workspace settings → Plugins](https://chatgpt.com/admin/plugins) (admin).
2. **Add → Import marketplace**.
3. Repository: `https://github.com/Spara-LLC/spara-plugins`
4. Path: leave blank.
5. Branch: `main` (or leave empty for the default).
6. Import, then set installation policy for each plugin. Enable the required app for the roles that should use it. Members still sign in to that app themselves.

Guide: [Importing and syncing plugin marketplaces from GitHub](https://help.openai.com/en/articles/20001504-importing-and-syncing-plugin-marketplaces-from-github)

### Register the ChatGPT app

Spara Aegis depends on a ChatGPT workspace app for `https://sparaaegis.com/mcp`. Sync from this repo updates plugin skills and packaging; it does not create the app or grant QuickBooks access.

The plugin points at that app from [`plugins/spara-aegis/.app.json`](plugins/spara-aegis/.app.json), and `.codex-plugin/plugin.json` sets `"apps": "./.app.json"`. The checked-in id is **`asdk_app_6ac9b37f1ea48191a5f31df805cba5f8`**. ChatGPT can only resolve the required app when that id is a registered `asdk_app_…`, `connector_…`, or `templated_apps_…` id.

1. In ChatGPT, open **Plugins**, select the plus button, then **Add custom MCP server**.
2. Name it Spara Aegis. Under Connection, enter the public MCP URL `https://sparaaegis.com/mcp` and complete authentication. The server expects OAuth for the staff member's Spara Microsoft account.
3. Review the risk warning, select **I understand and want to continue**, then **Create as a plugin**.
4. Copy the technical id from the browser URL. It looks like `plugin_asdk_app_…`. The value for `.app.json` is the app id: drop the `plugin_` prefix so it starts with `asdk_app_`. Use the app id, not a `plugin_…` id.
5. Paste that id into `plugins/spara-aegis/.app.json` as the `id` value.
6. After the marketplace syncs, open the imported Spara Aegis plugin and enable the required app for the roles that should use it.

Schema reference: [Reference an existing app with `.app.json`](https://learn.chatgpt.com/docs/enterprise/plugin-management).

## Claude Code

The Claude Code marketplace is [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json). The plugin is [`plugins/spara-aegis-claude`](plugins/spara-aegis-claude):

- `.claude-plugin/plugin.json`
- `.mcp.json` — remote HTTP server at `https://sparaaegis.com/mcp`
- `skills/` — the ChatGPT plugin skills, with ChatGPT-only wording removed

In Claude Code:

```
/plugin marketplace add Spara-LLC/spara-plugins
/plugin install spara-aegis@spara
```

`/plugin install` needs the plugin id. After the marketplace is added, `spara-aegis@spara` is Spara Aegis. `/plugin` opens the same list if you would rather pick it there. After it installs, run `/mcp` and sign in with the Spara Microsoft account when Aegis asks. QuickBooks stays read-only unless an administrator enables writes for that client.

Guide: [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces).

## claude.ai and Claude Desktop

claude.ai and Claude Desktop use a custom connector, not the Claude Code plugin. The connector is the remote MCP server. It does not install the skill files from this repo.

MCP URL: `https://sparaaegis.com/mcp`

On a Free, Pro, or Max plan:

1. Open [Customize → Connectors](https://claude.ai/customize/connectors).
2. Select **Add custom connector**.
3. Name it Spara Aegis.
4. Server URL: `https://sparaaegis.com/mcp`.
5. Leave OAuth client id and secret empty. Choose **Use Claude's published identity** when Claude asks how to identify itself. Do not choose automatic registration.
6. Add the connector, then **Connect** and sign in with the Spara Microsoft account.

This link opens the same dialog with the name and URL filled in: [Add Spara Aegis](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Spara%20Aegis&connectorUrl=https%3A%2F%2Fsparaaegis.com%2Fmcp).

On a Team or Enterprise plan, an Owner adds it for the organization:

1. Open **Organization settings → Connectors**.
2. Select **Add**, then **Custom**. If Claude asks for the connector type, choose **Web**.
3. Enter `https://sparaaegis.com/mcp`, leave OAuth client credentials empty, and use Claude's published identity.
4. Each member then opens **Customize → Connectors**, finds the custom connector, and selects **Connect**.

In Claude Desktop, signed in to the same account, use **Customize → Connectors** or **Search and Tools → Manage Connectors → Add custom connector**, with the same name and URL. Remote connectors run from Anthropic's cloud, including in the desktop app.

Guide: [Add a connector that isn't in the directory](https://claude.com/docs/connectors/custom/add-unlisted).

## Aegis

Both hosts call `https://sparaaegis.com/mcp`. That URL is the `resource` in the server's OAuth protected-resource metadata, and the authorization server is `https://clerk.sparaaegis.com`. Sync from this repo updates skills and packaging. It does not grant QuickBooks access.

The Claude skill tree is copied from [`plugins/spara-aegis/skills`](plugins/spara-aegis/skills). Update both copies when a skill changes. The Claude copy must not tell the model to use ChatGPT Apps SDK cards or other ChatGPT-only UI.
