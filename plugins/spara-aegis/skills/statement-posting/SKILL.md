---
name: statement-posting
description: Take in bank or card statements, confirm the client and the accounts, then record every transaction. Use the client knowledge base for the general-ledger account, vendor, and description. Put bank transaction detail in the QuickBooks memo.
---

# Statement posting

Take in statements. Confirm the accounts and the client. Then record all of the transactions on the statements. Reference the knowledge base for the postings, general-ledger accounts, and descriptions.

Use only Aegis tools. This is not a bank-feed match.

## 1. Take in the statements

If the user has not attached a statement, ask for the PDF, CSV, or image for each account. Read every line: date, description, amount, and whether it is money in or money out. Do not drop a line because it is small or unfamiliar.

## 2. Confirm the client

Call `list_my_clients`. Ask the user to pick one returned client and entity. A file name or a bank name is not enough. Stop if the client is not in the list.

Call `get_client_context` for that entity. Read `kb/profile.md`, `kb/banks-and-recon.md`, `kb/coding-rules.md`, `kb/vendors.md`, `kb/income-recognition.md`, `kb/accounting-finance-map.md`, and `kb/decisions-log.md`.

Call `get_qbo_company_details`. The QuickBooks company name must match the legal company in `kb/profile.md`. If it does not, stop.

## 3. Confirm the accounts

Call `search_qbo_records` on `account` and match each statement to one bank or credit-card account in that company. Show the user a checklist before any coding:

| Statement | QuickBooks account | Account ID | Confirmed |

Wait until the user confirms every statement, client, and account. If an account is missing, stop and ask. Do not post a statement to a different account.

## 4. Code every transaction from the knowledge base

For each line, in this order:

1. Skip it when the knowledge base says another system owns it, such as Ramp card spend, Ramp bill pay, Karbon, or payroll.
2. Search QuickBooks for the same account, date, and amount. Skip a line that is already recorded.
3. Choose the general-ledger account, vendor or payee, and description from `kb/coding-rules.md`, `kb/vendors.md`, `kb/income-recognition.md`, and recent `DECIDED` lines. Use the same coding when the payee has a repeated history.
4. If the knowledge base does not decide the account, search prior purchases, deposits, or payments for that payee. Prefer the coding used repeatedly.
5. If it is still unclear, ask. Do not invent a general-ledger account, vendor, class, or description.

Every posting needs a vendor or payee. Money in from a customer is a `payment` applied to an open invoice when one matches; otherwise it is a `deposit`. Money out is a `purchase`. Movement between the client's own accounts is a `transfer`. A split required by a knowledge-base schedule is a `journal_entry`.

Put two text fields on every QuickBooks posting:

- **Memo** (`PrivateNote` or the transaction memo): the bank or card statement detail for that line, copied from the statement as closely as possible.
- **Description** (line `Description`): a short who-and-what note using the client's description or memo pattern from the knowledge base.

Include the statement date and bank detail in the rationale.

## 5. Show the batch and wait

Show every line before writing:

| Date | Bank detail (memo) | Amount | Record as | General ledger | Vendor or payee | Description | Why |

Ask whether these postings are good. Wait for an answer. Apply edits. Do not propose until the user accepts the batch.

## 6. Propose, then ask again before recording

For each accepted line, call `propose_qbo_write` with `operation` `create` and the resource above. The payload uses the confirmed account, the knowledge-base general ledger, the vendor or payee, the amount, the statement date, the bank transaction detail in the memo, and the transaction description in the description field. Show the full payload.

If writes are disabled, stop and say an administrator must turn on Write enabled for that client. Do not look for another way to record the line.

Approval and recording are separate. After the user approves that exact batch, call `review_qbo_write_proposal` with `approve`. Ask again before calling `execute_qbo_write_proposal`. A changed line needs a new proposal.

After the records exist, offer to append the QuickBooks ids to `kb/decisions-log.md` with `save_client_files`. Save only if the user asks.
