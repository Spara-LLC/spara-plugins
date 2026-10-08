---
name: journal-entry-posting
description: Post the month's journal entries from the client knowledge base. Use prepopulated schedules for recurring entries, and use payroll or other attached documents when the knowledge base says those inputs are required.
---

# Journal entry posting

Look at that client's knowledge base for the journal entries this month needs. Some amounts come from prepopulated schedules. Others come from documents the user supplies, such as payroll reports. Do not invent an amount, account, or memo.

## 1. Client, period, and company

Select one client and one `YYYY-MM` period. Call `get_qbo_company_details` and stop if the company name does not match `kb/profile.md`.

Read `kb/month-end-checklist.md`, `kb/accounting-finance-map.md`, and `kb/decisions-log.md`. Then read the schedule files the checklist or finance map names. Start with `kb/schedules/prepaid-expenses.md`, `kb/schedules/amortization.md`, and `kb/schedules/loans-and-notes.md` when those paths exist. `get_client_context` accepts at most 12 files at a time, so read the rest in a second call.

## 2. Entries from prepopulated schedules

For each schedule row that covers the period, compute that month's entry from the row. Use the account, amount, and memo written in the schedule. Confirm each account with `search_qbo_records` on `account`. If the row has no account or no amount, list it as open and do not propose it.

For a loan or note, propose the interest entry only when the knowledge base gives the interest amount or the formula and the period's payment is already in QuickBooks. If the payment is missing, ask for the loan statement and wait.

## 3. Entries that need a document

If the knowledge base says an entry needs a payroll report or another document, ask for that file before calculating. Read the attached document. Use only figures that appear in the document and accounts named in the knowledge base. If a tax, wage, benefit, or other line is missing, ask. Do not fill the gap.

Typical document entries stay specific to the client file. Do not assume every client has the same payroll entry.

## 4. Avoid a second posting

Search `journal_entry` for the period. Skip an entry that QuickBooks already has for the same date, accounts, and amount. Say which ones were skipped.

## 5. Show the entries

Show one block per entry before any proposal:

| Source | Date | Account | Debit | Credit | Memo |

The source is the schedule path or the document name. Debits must equal credits. The memo states what the entry is and which schedule or document it came from.

Ask whether these entries are good. Wait. Apply edits. Do not propose until the user accepts the list.

## 6. Propose, then ask again

For each accepted entry, call `propose_qbo_write` with `operation` `create` and `resource` `journal_entry`. The payload date is the period end unless the schedule names another date. Each line uses the confirmed account, debit or credit, amount, and memo. Show the full payload.

If writes are disabled, stop and say an administrator must turn on Write enabled for that client.

After the user approves that exact list, call `review_qbo_write_proposal` with `approve`. Ask again before `execute_qbo_write_proposal`. A changed amount or account needs a new proposal.

Offer to save `closes/YYYY-MM/proposed-jes.md` with `save_client_files`. Save it only if the user asks. Include entries posted, entries skipped as already recorded, and entries left open because the schedule or document was incomplete.
