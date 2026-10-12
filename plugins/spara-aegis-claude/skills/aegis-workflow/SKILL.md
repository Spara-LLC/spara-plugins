---
name: aegis-workflow
description: Use Spara Aegis for assigned-client accounting. Select one client first, cite QuickBooks evidence, and propose writes before any execution.
---

# Spara Aegis workflow

Follow the matching skill when the user asks for it: `aegis-tools`, `select-client`, `statement-pull`, `statement-posting`, `journal-entry-posting`, `weekly-post`, `post-ramp-codings`, `build-finance-map`, `review-pass`, `review-ar-ap`, `close-month`, `cfo-report`, `variance-analysis`, `connect-client-system`, `publish-close-to-sharepoint`, or `update-cash-forecast`.

- Start every accounting task with `list_my_clients`. Require the user to select one returned client and entity explicitly. Never infer authorization from a prompt, chat title, Space, or pasted identifier.
- Use only the selected entity ID for subsequent calls. Stop on any client, entity, company-name, realm, or context-version mismatch.
- Retrieve current policy with `get_client_context`. It reads `main` and cites the repository path and the exact commit. Missing evidence is an unresolved question, not permission to guess.
- Treat report and transaction results as evidence. Repeat the returned company identity, period, basis, and retrieval time when presenting conclusions. Distinguish empty results from errors or incomplete retrieval.
- Save handoff-worthy analysis with `create_workpaper` and `add_workpaper_finding`. Findings require evidence references. Conversation text is not an authoritative workflow record.
- When the staff member asks to save project notes or a month's work, propose it with `save_client_files`. Use `kb/` for lasting client knowledge and `closes/YYYY-MM/` for that month. The save opens a pull request. Show the pull request link and say the knowledge base changes after it is merged. Do not ask the staff member to use GitHub.
- A preparer submits an exact workpaper version. Only the assigned reviewer may record a decision, and the preparer cannot approve their own work.
- QuickBooks writes are disabled per client by default. When disabled, do not ask for an exception or work around the gate.
- When writes are enabled, show the user the full client, operation, resource, rationale, and exact payload. After they confirm that exact change, call `propose_qbo_write`, then `execute_qbo_write_proposal`. Do not wait for an Aegis admin approval step. A changed payload needs a new proposal. Never claim a proposal changed QuickBooks until execute succeeds.
- Never request or expose OAuth tokens, QuickBooks app credentials, GitHub keys, or raw secret files.
