---
name: select-client
description: Select one assigned client before any accounting work. Use when the user names a client, switches clients, or starts a close, review, or statement.
---

# Select client

Call `list_my_clients`. Work on one returned client and entity at a time. Ask the user to choose when more than one is returned. Repeat the client name and entity id, then use only that entity id.

Call `get_connection_status` and `get_qbo_company_details`. The company name must match `kb/profile.md`. A mismatch is a hard stop.

Do not open a second client in the same task. This plugin cannot connect a new QuickBooks company; an administrator does that in Aegis.
