# AGENTS.md — install «chrome-mine» (chrome-devtools-mcp, --autoConnect)

You are an AI agent asked by a non-technical user to install this MCP server. Follow these steps exactly. Reply to the user in their language (usually Ukrainian), in plain words.

## Server definition (use exactly this — pinned version)

- name: `chrome-mine`
- command: `npx`
- args: `-y`, `chrome-devtools-mcp@1.12.0`, `--autoConnect`, `--no-usage-statistics`
- optional: `startup_timeout_sec = 60`

Do **not** use `@latest`. Do **not** drop `--no-usage-statistics`.

## Rules

1. **Add only this one server.** Do not modify, reorder, reformat or remove any other settings or servers. Append; never rewrite the whole file.
2. **Back up first.** Copy the config file to `<file>.bak-chrome-mine` before editing.
3. If a server named `chrome-mine` already exists — stop and ask the user.
4. **Never** click «Allow» in Chrome's «Allow remote debugging?» dialog and never change Chrome settings yourself. Those are the user's actions.
5. Do not install anything else. If Node.js is missing, tell the user how to install it (see below) and stop.

## Steps

1. Check Node.js: run `node -v`. Need v20.19+ (or v22.12+). If missing:
   - Windows: `winget install --id OpenJS.NodeJS.LTS -e`, then the user restarts the app/PC.
   - macOS: installer `.pkg` from https://nodejs.org.
2. Detect the host and add the server:

   **ChatGPT desktop app / Codex** (config `~/.codex/config.toml`, Windows: `%USERPROFILE%\.codex\config.toml`). Preferred:

   ```
   codex mcp add chrome-mine -- npx -y chrome-devtools-mcp@1.12.0 --autoConnect --no-usage-statistics
   ```

   If the `codex` command is not available, append this block to the end of `config.toml` (do not touch anything else):

   ```toml
   [mcp_servers.chrome-mine]
   command = "npx"
   args = ["-y", "chrome-devtools-mcp@1.12.0", "--autoConnect", "--no-usage-statistics"]
   startup_timeout_sec = 60
   ```

   **Claude Desktop**: in `claude_desktop_config.json` (Settings → Developer → Edit Config) add inside `mcpServers` — keep all existing entries:

   ```json
   "chrome-mine": {
     "command": "npx",
     "args": ["-y", "chrome-devtools-mcp@1.12.0", "--autoConnect", "--no-usage-statistics"]
   }
   ```

   **Windows 10, if the server fails with `Connection closed`:** use `command = "cmd"` and args `["/c", "npx", "-y", "chrome-devtools-mcp@1.12.0", "--autoConnect", "--no-usage-statistics"]`.
3. Show the user the exact block you added and the backup path.

## Tell the user what to do next (in plain words)

1. Fully quit and reopen the app (ChatGPT: File → Quit; Claude: quit from tray).
2. In Chrome open `chrome://inspect/#remote-debugging` and turn on **Allow remote debugging for this browser instance**.
3. Close work profiles, banking and private tabs they don't want the agent to see.
4. Check: in ChatGPT type `/mcp` → `chrome-mine Enabled`.
5. First test: «Використай chrome-mine: відкрий bank.gov.ua у новій вкладці і скажи курс євро на сьогодні. Інших вкладок не чіпай.»
6. Two permissions: in the app — **Allow once**; in Chrome — **Allow** (the dialog may be hidden behind the app window).

## Uninstall

Remove the `chrome-mine` block (or restore `<file>.bak-chrome-mine`), restart the app, and turn off the checkbox on `chrome://inspect/#remote-debugging`.
