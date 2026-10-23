---
name: aegis-workflow
description: Use Spara Aegis for assigned-client accounting. Select one client first, cite QuickBooks evidence, and propose writes before any execution.
---

# Spara Aegis workflow

Follow the matching skill when the user asks for it: `aegis-tools`, `select-client`, `statement-pull`, `statement-posting`, `journal-entry-posting`, `weekly-post`, `post-ramp-codings`, `build-finance-map`, `review-pass`, `review-ar-ap`, `close-month`, `cfo-report`, `variance-analysis`, `connect-client-system`, `publish-close-to-sharepoint`, or `update-cash-forecast`.

- Start every accounting task with `list_my_clients`. Require the user to select one returned client and entity explicitly. Never infer authorization from a prompt, chat title, Space, or pasted identifier.
- Use only the selected entity ID for subsequent calls. Stop on any client, entity, company-name, realm, or context-version mismatch.
- Retrieve current policy with `get_client_context`. It reads `main` and cites the repository path and the exact commit. Missing evidence is an unresolved question, not permission to guess.
- `list_my_clients` returns each client's `booksSystem`. For a client that is not on QuickBooks, skip the QuickBooks tools; they answer `NOT_ON_QUICKBOOKS`. Work from repository files instead.
- For any client, use `list_client_files` to see analysis, close, review, report and financials files, and `read_client_file` to read md, csv, tsv, json or txt. Large files come back in windows, so keep calling with `nextStartLine` only as far as the question needs. Spreadsheets and PDFs are listed but cannot be opened; say so rather than guessing their contents. Cite the path and commit.
- Treat report and transaction results as evidence. Repeat the returned company identity, period, basis, and retrieval time when presenting conclusions. Distinguish empty results from errors or incomplete retrieval.
- Save handoff-worthy analysis with `create_workpaper` and `add_workpaper_finding`. Findings require evidence references. Conversation text is not an authoritative workflow record.
- When the staff member asks to save project notes, a month's work or analysis, propose it with `save_client_files`. This needs read-and-write access on the client. It accepts text files in the client's allowed folders: use `kb/` for lasting client knowledge, `closes/YYYY-MM/` for that month, and `analysis/` or `reports/` for analysis. The save opens a pull request. Show the pull request link and say the knowledge base changes after it is merged. Do not ask the staff member to use GitHub.
- A preparer submits an exact workpaper version. Only the assigned reviewer may record a decision, and the preparer cannot approve their own work.
- QuickBooks writes are disabled per client by default. When disabled, do not ask for an exception or work around the gate.
- When writes are enabled, show the user the full client, operation, resource, rationale, and exact payload. After they confirm that exact change, call `propose_qbo_write`. Each call queues the change in the client's open Aegis approval batch and returns an `approvalUrl`; it never writes to QuickBooks. Do not call `execute_qbo_write_proposal`: it is unavailable while web approval is required. Give the user the `approvalUrl` (in Claude, the review card's **Review & approve in Aegis** button opens it). An Aegis administrator reviews, approves, and posts the batch in the Aegis web app. A changed payload needs a new proposal. Never claim QuickBooks changed until `get_qbo_batch` shows the batch posted.
- Never request or expose OAuth tokens, QuickBooks app credentials, GitHub keys, or raw secret files.

## When Aegis is not working

Fall back only when Aegis itself fails: a tool errors or times out, the tool is not in this session, or a feature is off or not set up. Examples: SharePoint's "Aegis's Microsoft app is not allowed on this SharePoint site yet", or a Composio or QuickBooks token error.

- You may then use the user's own connector for that same service, such as SharePoint or OneDrive, Outlook, QuickBooks, Karbon, or Ramp, if one is connected in this Claude or ChatGPT session.
- Reads are allowed. Say plainly in the chat that Aegis was unavailable, which connector you used instead, and the Aegis error.
- File saves, such as a SharePoint or OneDrive close pack or upload, need an explicit yes in the chat. Show the destination path and file names first.
- QuickBooks and Ramp postings and other accounting writes stay in Aegis, approved in the Aegis web app. Through another connector, prepare the batch and stop.
- Do not fall back when Aegis refuses for access reasons: the client is not granted, access is read-only, the month is closed or locked, or `NOT_ON_QUICKBOOKS`. Those are decisions, not outages. Say so and stop.
- Log each fallback in the close notes or run notes: the date, the Aegis tool and error, the connector used, and what was read or saved.
