---
name: aegis-tools
description: Explain what the signed-in person can do with Spara Aegis. Use when they ask what they can do, what tools they have, or what this connector can access.
---

# What you can do

The connector is `https://sparaaegis.com/mcp`. Call tools to see what this person can open. Do not assume a client, a QuickBooks grant, or a CRM grant.

Web apps are at [sparaaegis.com/tools](https://sparaaegis.com/tools). Send them there for CRM, Admin, Marketing, and Profitability.

## Client context

- `list_my_clients` — clients and entities assigned to this person
- `get_client_context` — knowledge-base files on `main` for the entity they select
- `list_client_files` — files on `main` in the client's allowed folders, with sizes. Works for every client, on QuickBooks or not. The default folders are `kb/`, `closes/`, `analysis/`, `reviews/`, `reports/` and `financials/`; an administrator can change them per client. Spreadsheets, PDFs and other binary files are listed as not readable
- `read_client_file` — reads one md, csv, tsv, json or txt file from those folders, a window at a time. Large CSVs come back in line windows with the header, the total line count and `nextStartLine`
- `save_client_files` — opens a pull request that adds or updates text files in the client's allowed folders. It needs read-and-write access on that client. Main changes after it is merged
- `secrets/`, hidden folders such as `.cursor/`, and `.env` files are never listed, read or saved
- `create_workpaper`, `add_workpaper_finding`, `submit_for_review`, `record_review_decision` — workpapers and review. The reviewer is a different person

## QuickBooks

`list_my_clients` returns each client's `booksSystem`. A client that is not on QuickBooks (`sage`, `other` or `none`) has no QuickBooks tools: they answer `NOT_ON_QUICKBOOKS`. Skip them for that client and use the repository file tools instead.

Access is per client: read-only, or read-and-write. For a QuickBooks client, call `get_connection_status` for the entity they select. If a write tool is refused, say so and stop.

Read: `get_financial_report`, `get_qbo_report`, `find_transactions`, `search_qbo_records`, `get_qbo_record`, `get_qbo_company_details`, `get_qbo_invoice_pdf`, `list_qbo_write_proposals`.

Write, only when that client allows it: show the full change and, after they confirm that exact payload, call `propose_qbo_write` (or `propose_qbo_coding_batch` for coding rows). These only queue rows in an Aegis approval batch and return an `approvalUrl`. QuickBooks is never written from chat: `execute_qbo_write_proposal` is unavailable while web approval is required. Give the user the `approvalUrl` (the **Review & approve in Aegis** button on the Claude review card); an Aegis administrator approves and posts the batch in the web app. `get_qbo_batch` shows its status. `review_qbo_write_proposal` rejects a proposal.

## CRM

A CRM grant is `read_only` or `read_write`. Call a CRM tool to learn which one this person has.

Read: `list_crm_deals`, `get_crm_deal`, `list_crm_next_steps`, `list_crm_reminders`, `list_crm_stages`, `list_crm_write_proposals`.

Aegis-only, not Karbon, and only with read-and-write: `complete_crm_reminder`, `snooze_crm_reminder`, `update_crm_deal_details`.

Karbon writes need an explicit yes. `log_crm_touch`, `propose_crm_write`, and `create_crm_work_item` only prepare a proposal. `confirm_crm_write` is what writes to Karbon, after they confirm that proposal. `reject_crm_write` cancels it.

## Marketing

`get_website_traffic` reads stored site traffic. It is admin-only: it is available only on the Aegis-Admin connector (`https://sparaaegis.com/mcp/admin`) when Marketing is enabled. The staff Aegis connector (`https://sparaaegis.com/mcp`) does not expose it, and the same applies to Spara's own books and bank feed (`list_spara_bank_accounts`, `list_spara_bank_transactions`, `propose_spara_bank_coding_batch`) and `get_month_snapshot`. If the tool is missing, tell the user it needs the Aegis-Admin connector.

## Profitability

`get_client_profitability` returns client profitability: hours, standard value, labor cost, invoiced revenue, margin, realization, and effective rate per client, or one client's detail with a monthly trend and breakdowns. Pass `month` (`YYYY-MM`) and `span`, the same as the web Profitability pages. It reads Karbon time and invoices synced into Aegis, not QuickBooks, so use it instead of QuickBooks invoices or reports for profitability questions. It is admin-only: available only on the Aegis-Admin connector (`https://sparaaegis.com/mcp/admin`) when Profitability is enabled. If it is missing, tell the user it needs the Aegis-Admin connector.

## When Aegis is not working

If an Aegis tool errors, times out, or is missing, follow **When Aegis is not working** in `aegis-workflow`. The user's own connector for the same service may be used for reads, and for file saves after they confirm. Accounting postings stay in Aegis. An access refusal is not an outage.

## Web tools

After sign-in at sparaaegis.com/tools:

- CRM (`/crm`) — pipeline, for a CRM grant
- Admin (`/admin`) — clients, staff, and workpapers, for administrators
- Marketing (`/marketing`) — site traffic, for administrators when it is enabled
- Profitability (`/profitability`) — profitability, utilization, and billing, for administrators when it is enabled
