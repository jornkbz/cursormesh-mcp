# CursorMesh — Browser Control Guidelines for Gemini

You have access to the `cursormesh` MCP server, which controls the user's real Google Chrome browser through the CursorMesh MV3 extension with visible AI cursors, colored tab groups, focus control, native file uploads, and desktop notifications.

## Recommended Workflow
1. **Discover or open tabs:** Start with `list_tabs` to inspect existing tabs or `open_tab` to create a new isolated tab in the agent's colored group. Always pass `tabId` explicitly on follow-up calls so parallel agents never collide.
2. **Read before acting:** Call `snapshot({ tabId })` to obtain structured `[uid]` references, visible text (including open Shadow DOM), and `href` attributes before calling `click`, `fill`, or `upload_file`.
3. **Upload local files cleanly:** Use `upload_file({ tabId, uid, filePath })` on either `<input type="file">` elements or Upload buttons — CursorMesh intercepts the native file picker via CDP without opening the OS dialog.
4. **Respect security gates:** If `click` or `fill` returns `SECURITY_APPROVAL_REQUIRED`, stop immediately and ask the user to confirm in Chrome (never attempt to bypass password, payment, or CAPTCHA checks).
5. **Finish cleanly:** Call `release_cursor({ tabId, summary })` or `notify_user({ title, message, kind, tabId })` when a task finishes or needs user input.

## Prerequisites
- Install the **CursorMesh** Chrome extension from https://cursormesh.app/
- Pair once by running `npx -y @cursormesh/mcp-bridge --print-token` and pasting the token in CursorMesh → Side Panel → **MCP & Pro**.
