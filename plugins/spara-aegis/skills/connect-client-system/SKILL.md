---
name: connect-client-system
description: Check whether a client's QuickBooks connection is active. Use when the user asks to connect or reconnect QuickBooks. This plugin does not collect credentials.
---

# QuickBooks connection

Select the client and call `get_connection_status`. Report whether the connection is active or needs a reconnect. If the client's `booksSystem` is not `quickbooks`, there is nothing to connect: say the client is not on QuickBooks and point to its repository files.

Do not ask for a QuickBooks password, client id, client secret, or token. Reconnect is an administrator action in Aegis, at Clients, using the firm QuickBooks app.
