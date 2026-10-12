---
name: build-finance-map
description: Create or refresh the client accounting and finance map from the knowledge base and live QuickBooks accounts. Use before the first close or when systems and general-ledger sources are missing.
---

# Build finance map

Select the client first. Read the knowledge base with `get_client_context`, especially `kb/profile.md`, `kb/coding-rules.md`, `kb/banks-and-recon.md`, `kb/vendors.md`, and `kb/income-recognition.md`.

Call `get_qbo_company_details` and `search_qbo_records` on `account`. Use live account names and ids. Do not invent accounts.

Write `kb/accounting-finance-map.md` only after the user agrees. Include the systems that feed QuickBooks, what each system posts, which statement it affects, and which general-ledger account it uses. List gaps as open questions.

Save with `save_client_files`. That opens a pull request and does not change main until the pull request is merged. Show the pull request link. Offer a short note in `kb/decisions-log.md` and save that only if the user asks.
