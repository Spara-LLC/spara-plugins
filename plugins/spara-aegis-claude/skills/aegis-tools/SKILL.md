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
- `save_client_files` — opens a pull request for `kb/` or `closes/YYYY-MM/` notes. Main changes after it is merged
- `create_workpaper`, `add_workpaper_finding`, `submit_for_review`, `record_review_decision` — workpapers and review. The reviewer is a different person

## QuickBooks

Access is per client: read-only, or read-and-write. Call `get_connection_status` for the entity they select. If a write tool is refused, say so and stop.

Read: `get_financial_report`, `get_qbo_report`, `find_transactions`, `search_qbo_records`, `get_qbo_record`, `get_qbo_company_details`, `get_qbo_invoice_pdf`, `list_qbo_write_proposals`.

Write, only when that client allows it: show the full change, call `propose_qbo_write`, then `execute_qbo_write_proposal` after they confirm that exact payload. `review_qbo_write_proposal` rejects a proposal.

## CRM

A CRM grant is `read_only` or `read_write`. Call a CRM tool to learn which one this person has.

Read: `list_crm_deals`, `get_crm_deal`, `list_crm_next_steps`, `list_crm_reminders`, `list_crm_stages`, `list_crm_write_proposals`.

Aegis-only, not Karbon, and only with read-and-write: `complete_crm_reminder`, `snooze_crm_reminder`, `update_crm_deal_details`.

Karbon writes need an explicit yes. `log_crm_touch`, `propose_crm_write`, and `create_crm_work_item` only prepare a proposal. `confirm_crm_write` is what writes to Karbon, after they confirm that proposal. `reject_crm_write` cancels it.

## Marketing

`get_website_traffic` reads stored site traffic. It is there when Marketing is enabled for an administrator.

## Web tools

After sign-in at sparaaegis.com/tools:

- CRM (`/crm`) — pipeline, for a CRM grant
- Admin (`/admin`) — clients, staff, and workpapers, for administrators
- Marketing (`/marketing`) — site traffic, for administrators when it is enabled
- Profitability (`/profitability`) — profitability, utilization, and billing, for administrators when it is enabled
