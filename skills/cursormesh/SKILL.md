---
name: cursormesh
description: Control Google Chrome tabs using CursorMesh MCP with visible multi-agent cursors, colored tab groups, Shadow DOM snapshots, native CDP file uploads, and desktop notifications. Use when automating web apps, forms, dashboards, or visual browser checks in the user's Chrome session.
---

# CursorMesh Browser Automation Skill

Use the `cursormesh` MCP server to interact with the user's real Chrome browser safely and transparently.

## First-Time Local MCP Setup (if `cursormesh` tools are not connected yet)
1. Install the **CursorMesh** Chrome extension from the Chrome Web Store: https://chromewebstore.google.com/detail/cursormesh/hcbdhdajjpabnjhabpakmfogdlmlnghn (requires Node.js 18+).
2. Register the local MCP server in Claude Code:
   ```bash
   claude mcp add cursormesh -- npx -y @cursormesh/mcp-bridge --agent-name Claude --agent-color "#F59E0B"
   ```
3. Print your one-time local pairing token and paste it in **CursorMesh Side Panel → MCP & Pro**:
   ```bash
   npx -y @cursormesh/mcp-bridge --print-token
   ```

## Core Loop
1. **Locate or open a tab:** Call `list_tabs` to see open tabs, or `open_tab({ url })` to open a dedicated tab in the agent's colored Chrome tab group. Always pass `tabId` on subsequent calls.
2. **Inspect the DOM:** Call `snapshot({ tabId })` before clicking or typing. Use the returned `[uid]` identifiers (e.g. `e12`), `href` attributes, and visible text (including open Shadow DOM).
3. **Interact:**
   - `click({ tabId, uid })` — smart hybrid click (native `isTrusted` CDP on visible tabs, automatic Shadow-DOM click on occluded/background tabs).
   - `fill({ tabId, uid, value, pressEnter? })` — works on `<input>`, `<textarea>`, `<select>`, and `contenteditable` (React/Vue compatible).
   - `upload_file({ tabId, uid, filePath })` — uploads local files directly to `<input type="file">` or Upload buttons via CDP without opening the OS file chooser.
   - `screenshot({ tabId, format })` — fast PNG/JPEG visual capture (CDP-backed for unfocused windows).
4. **Handle security gates & handoffs:**
   - If `click` or `fill` returns `SECURITY_APPROVAL_REQUIRED`, pause and ask the user to approve the sensitive action (password, payment, deletion) in Chrome.
   - Use `notify_user({ title, message, kind: "action_required" | "done", tabId })` to send a clickable desktop notification when user intervention is needed or when a long task completes.
   - Call `release_cursor({ tabId, summary, closeGroup: true })` when finished and no user reply/review is needed (closes only this agent's temporary tab group cleanly). Keep `closeGroup: false` (default) when leaving a draft, login, or form open for the user.

## Reference
- Chrome Extension & full docs: https://cursormesh.app/docs/
- npm package: https://www.npmjs.com/package/@cursormesh/mcp-bridge
