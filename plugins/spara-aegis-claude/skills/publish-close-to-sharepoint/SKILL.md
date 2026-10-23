---
name: publish-close-to-sharepoint
description: Prepare a closed month for delivery. Use when the user asks to publish or archive a close. Aegis does not upload to SharePoint from this plugin.
---

# Publish a close

Select the client and the closed month. Read `kb/sharepoint.md` and the files under `closes/YYYY-MM/` with `get_client_context`. List the close notes that are ready.

Aegis cannot upload to SharePoint from this plugin. Do not ask for Microsoft credentials and do not claim the files were published. Tell the user the SharePoint delivery still runs from Spara OS after they approve it there.

If the user's own SharePoint or OneDrive connector is connected, you may save the close files through it under **When Aegis can't do the job** in `aegis-workflow`. Show the destination path and file names first, and save only after they confirm in the chat. Then log the save in the close notes.
