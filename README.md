# Airlock — Claude Code Plugins

Official [Claude Code](https://code.claude.com/) plugins for **Airlock** — the cryptographically enforced approval gateway for AI agents.

These plugins integrate directly into Claude Code's plugin and hooks architecture to gate AI tool use (shell commands, file edits, MCP calls, etc.) through a mobile approval flow, ensuring no sensitive or high-impact action runs without an explicitly signed human decision.

---

## Available Plugins

| Plugin | Description |
|--------|-------------|
| **[Airlock Enforcer](plugins/claude-code-enforcer/)** | Gates all Claude Code tool use through the Airlock gateway for human-in-the-loop mobile approval. Includes sign-in, pairing, auto-approve patterns, fail-mode, and Do Not Disturb support. |

---

## Quick Start

### Install from the marketplace

This repository is a **Claude Code plugin marketplace**. Install the Airlock enforcer directly from Claude Code:

1. Add the marketplace (one-time):
   ```bash
   /plugin marketplace add airlockapp/claude-plugins
   ```

2. Install the plugin:
   ```bash
   /plugin install airlock@airlock-claude-plugins
   ```

3. Sign in and pair:
   ```
   /airlock:sign-in
   /airlock:pair
   ```

That's it! The daemon starts automatically and all tool use is now gated through Airlock.

### Install from a local directory

For development or one-off use, load the plugin directly:

```bash
claude --plugin-dir /path/to/plugins/claude-code-enforcer
```

See [plugins/claude-code-enforcer/INSTALL.md](plugins/claude-code-enforcer/INSTALL.md) for the full installation guide (prerequisites, secure storage, dev mode, troubleshooting).

---

## Uninstall

To fully remove the Airlock enforcer:

1. **Unpair and sign out** (from Claude Code or terminal):
   ```
   /airlock:unpair
   /airlock:sign-out
   ```

2. **Remove the plugin** (if installed from the marketplace):
   ```bash
   claude plugin uninstall airlock@airlock-claude-plugins
   ```

3. **Remove config directory**:
   ```bash
   rm -rf ~/.config/airlock-enforcer
   ```
   On Windows: `Remove-Item -Recurse -Force "$env:USERPROFILE\.config\airlock-enforcer"`

4. **Remove OS keychain entries** (if you used secure storage): delete entries named **"Airlock Enforcer"** from Credential Manager (Windows), Keychain Access (macOS), or your keyring manager (Linux).

5. **Remove `.airlock` dotfiles** from any paired workspace directories (optional).

See [plugins/claude-code-enforcer/INSTALL.md](plugins/claude-code-enforcer/INSTALL.md) for detailed uninstall steps.

---

## HARP (Human Authorization & Review Protocol)

Airlock implements **[HARP](https://harp-protocol.github.io/)** — the Human Authorization & Review Protocol — a standards-grade, cryptographically verifiable authorization layer for AI agents.

- **Binding** — Approvals are cryptographically bound to the exact artifact reviewed.
- **Verifiable decisions** — Ed25519-signed decisions; replay and substitution are rejected.
- **E2E encryption** — Artifact payloads are encrypted (AES-256-GCM) so gateways stay zero-knowledge.
- **Interoperability** — Open schemas, test vectors, and conformance criteria for cross-vendor use.

---

## Repository Structure

```
airlock-claude-plugins/
├── .claude-plugin/
│   └── marketplace.json       # Claude Code marketplace definition
├── plugins/
│   └── claude-code-enforcer/  # Airlock enforcer plugin for Claude Code
│       ├── .claude-plugin/
│       │   └── plugin.json    # Plugin manifest
│       ├── commands/          # Plugin slash commands (/airlock:*)
│       ├── hooks/             # SessionStart, SessionEnd, PreToolUse hooks
│       ├── scripts/           # Bootstrap and session management
│       ├── daemon/            # Standalone pipe server + auth + gateway
│       ├── skills/            # Plugin skills
│       ├── INSTALL.md         # Detailed installation guide
│       └── README.md          # Plugin documentation
├── LICENSE                    # MIT License
└── README.md                 # This file
```

---

## Related

- **[Airlock Extensions](https://github.com/airlockapp/extensions)** — IDE enforcer extensions for Cursor, Windsurf, VS Code (Copilot & Antigravity), and CLI
- **[HARP Protocol](https://harp-protocol.github.io/)** — Human Authorization & Review Protocol specification

---

## License

This project is licensed under the [MIT License](LICENSE).
