---
name: aegis-workflow
description: Use Spara Aegis first for assigned-client accounting, and the person's own connectors when Aegis can't do the job. Select one client first, cite QuickBooks evidence, and propose writes before any execution.
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

## When Aegis can't do the job

Aegis is the first choice whenever it can do the job. When it can't, you may read the data through the person's own connector for the same source, if one is connected in this Claude or ChatGPT session: Microsoft 365, SharePoint or OneDrive, Outlook, QuickBooks, Karbon, or Ramp. This is allowed. It is not a workaround.

It applies when:

- Aegis is not working: a tool errors or times out, the tool is not in this session, or a feature is off or not set up. Examples: SharePoint's "Aegis's Microsoft app is not allowed on this SharePoint site yet", or a Composio or QuickBooks token error.
- Aegis can't do it: Aegis lists a file it can't open (PDF, XLSX or XLS, images), Aegis has no tool for the task, or the source is one Aegis doesn't reach. For example, bank and card statements in SharePoint that Aegis lists but can't open.

Guardrails:

- Say plainly in the chat which connector you used and why: the Aegis error or the gap.
- File saves, such as a SharePoint or OneDrive close pack or upload, need an explicit yes in the chat. Show the destination path and file names first.
- QuickBooks and Ramp postings and other accounting writes stay in Aegis, approved in the Aegis web app. Data read this way, such as a statement, still posts through an Aegis approval batch. Never post through the other connector.
- Do not fall back when Aegis refuses for access reasons: the client is not granted, access is read-only, the month is closed or locked, or `NOT_ON_QUICKBOOKS`. Those are decisions, not gaps. Say so and stop.
- Log each fallback in the close notes or run notes: the date, the Aegis tool and the error or gap, the connector used, and what was read or saved.
