# CursorMesh — Multi-Agent AI Cursors for Chrome (MCP · Gemini CLI · Claude Plugin)

[![Website](https://img.shields.io/badge/Website-cursormesh.app-00D4FF?style=flat-square)](https://cursormesh.app/)
[![Docs](https://img.shields.io/badge/Docs-cursormesh.app%2Fdocs-7C3AED?style=flat-square)](https://cursormesh.app/docs/)
[![npm](https://img.shields.io/npm/v/@cursormesh/mcp-bridge?style=flat-square&color=10B981)](https://www.npmjs.com/package/@cursormesh/mcp-bridge)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)

Control your real Google Chrome browser from **Gemini CLI**, **Claude Code / Claude Desktop**, **Cursor**, **Antigravity**, **VS Code**, or any MCP-compatible AI agent — with **visible colored AI cursors**, **isolated colored tab groups**, **Shadow DOM snapshots**, **native CDP file uploads**, **desktop notifications**, and an **instant STOP button**.

> This repository hosts the official public distribution manifests (`gemini-extension.json`, `.claude-plugin/plugin.json`, `skills/cursormesh/SKILL.md`, and `server.json`) for the [`@cursormesh/mcp-bridge`](https://www.npmjs.com/package/@cursormesh/mcp-bridge) npm package and the **CursorMesh** Chrome extension.

---

## Prerequisites

1. **Google Chrome** with the **[CursorMesh Chrome Extension](https://cursormesh.app/)** installed.
2. **Node.js 18+** (required to run `npx -y @cursormesh/mcp-bridge` locally; [nodejs.org](https://nodejs.org/)).
3. **One-time pairing token:** run:
   ```bash
   npx -y @cursormesh/mcp-bridge --print-token
   ```
   and paste the token in **CursorMesh Side Panel → MCP & Pro → Connect**.

Full step-by-step setup guide: **[cursormesh.app/docs](https://cursormesh.app/docs/)**

---

## 1-Command Installation

### Gemini CLI (Official Extension)

```bash
gemini extensions install https://github.com/jornkbz/cursormesh-mcp
```

### Claude Code / Claude Plugin

Install from the Claude Plugin Directory or add this repository as a plugin source (`.claude-plugin/plugin.json` + `skills/cursormesh/SKILL.md`).

### Cursor, Antigravity, Claude Desktop, VS Code, Windsurf (Generic MCP)

Add this entry to your MCP configuration (`mcp_config.json`, `claude_desktop_config.json`, `.cursor/mcp.json`, etc.):

```json
{
  "mcpServers": {
    "cursormesh": {
      "command": "npx",
      "args": [
        "-y",
        "@cursormesh/mcp-bridge",
        "--agent-name",
        "AI Agent",
        "--agent-color",
        "#00D4FF"
      ]
    }
  }
}
```

> **Multi-Agent tip:** Configure two or more agents with different `--agent-name` and `--agent-color` values. CursorMesh automatically coordinates them over a shared local WebSocket hub (`ws://127.0.0.1:47891`), giving each agent its own visible cursor and colored Chrome tab group.

---

## Included MCP Tools (14)

| Tool | Description |
| --- | --- |
| `list_tabs` | List open Chrome tabs with `tabId`, URL, title, and window/group metadata. |
| `open_tab` | Open a new tab inside the agent's colored tab group (respecting your Focus Mode). |
| `switch_tab` | Focus a specific tab (auto-restores minimized AI window). |
| `close_tab` | Close a tab by `tabId`. |
| `navigate` | Navigate a tab to a URL and wait for load completion. |
| `snapshot` | Structured DOM map with `[uid]` references, `href` attributes, and visible text (including open Shadow DOM). |
| `screenshot` | Fast PNG/JPEG capture (CDP-backed for unfocused windows). |
| `hover` | Smoothly move the visible AI cursor over a target element. |
| `click` | Smart-hybrid click: native `isTrusted` CDP on visible tabs, automatic Shadow-DOM click on occluded/background tabs. |
| `fill` | Type into `<input>`, `<textarea>`, `<select>`, and `contenteditable` fields (React/Vue compatible). |
| `upload_file` | Upload local files directly to `<input type="file">` or Upload buttons via CDP without opening the OS file picker. |
| `scroll` | Scroll by direction/delta or bring a `[uid]` into view. |
| `notify_user` | Send a clickable Chrome desktop notification when input is needed or a long task finishes. |
| `release_cursor` | Hide the agent's virtual cursor and optionally notify the user with a summary. |

---

## Safety & Privacy

- **100% local MCP bridge:** runs strictly on `127.0.0.1:47891` with a per-user local pairing token (`~/.cursormesh/token`).
- **Sensitive action approval:** password fields, payment flows, and destructive buttons trigger an explicit confirmation gate (`SECURITY_APPROVAL_REQUIRED`).
- **Instant STOP pill:** draggable floating button in Chrome immediately halts all agents.
- **Privacy Policy:** [cursormesh.app/privacy](https://cursormesh.app/privacy/)

---

## Links

- **Website:** https://cursormesh.app/
- **Documentation:** https://cursormesh.app/docs/
- **npm Package:** https://www.npmjs.com/package/@cursormesh/mcp-bridge
- **Support:** soporte@cursormesh.app
