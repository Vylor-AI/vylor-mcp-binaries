# Vylor Code Intelligence MCP Server

Fast, AST-aware code intelligence server built on the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/).

Provides repository structure mapping, multi-file retrieval, and symbol definition search for AI coding agents (**Cursor**, **Claude Desktop**, **Claude Code**, **VS Code**, **Cline**, **Roo Code**, **Antigravity**).

---

## ⚡ Features & Available Tools

| Tool | Description |
|---|---|
| **`request_repo_map`** | Generates a token-budgeted, AST-aware structural map of your workspace, highlighting directories, symbol densities, and import relationships. |
| **`find_files`** | Multi-file finder that locates files across the repository and extracts full contents or head line previews. |
| **`find_code_definition`** | Instant Tree-sitter AST symbol lookup for functions, classes, interfaces, and types across 100+ programming languages. |

---

## 📥 Download

Download the standalone binary for your operating system from the **[Releases](../../releases)** page:

- **Windows**: `vylor-mcp.exe`
- **macOS**: `vylor-mcp` (Apple Silicon & Intel)
- **Linux**: `vylor-mcp-linux`

> **Note**: These are self-contained native binaries. **No Python, Git, or dependencies are required.**

---

## 🚀 Setup & Integration

Place the downloaded binary in a convenient folder on your system (e.g. `C:\tools\vylor-mcp.exe` on Windows or `~/bin/vylor-mcp` on Mac/Linux).

---

### 1. Cursor

Create or edit `.cursor/mcp.json` in your project root:

**Windows:**
```json
{
  "mcpServers": {
    "vylor": {
      "command": "C:\\tools\\vylor-mcp.exe",
      "args": ["--repo-path", "${workspaceFolder}"]
    }
  }
}
```

**macOS / Linux:**
```json
{
  "mcpServers": {
    "vylor": {
      "command": "/Users/YOUR_USER/bin/vylor-mcp",
      "args": ["--repo-path", "${workspaceFolder}"]
    }
  }
}
```

---

### 2. Claude Desktop

Add to your Claude Desktop configuration file:
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "vylor": {
      "command": "C:\\tools\\vylor-mcp.exe",
      "args": [
        "--repo-path", "C:\\path\\to\\your\\project"
      ]
    }
  }
}
```

---

### 3. Claude Code (CLI)

Run in your terminal:

**Windows:**
```powershell
claude mcp add vylor "C:\tools\vylor-mcp.exe" "--" "--repo-path" "C:\path\to\your\project"
```

**macOS / Linux:**
```bash
claude mcp add vylor "/usr/local/bin/vylor-mcp" "--" "--repo-path" "/path/to/your/project"
```

---

### 4. VS Code / Antigravity / Cline / Roo Code

Add to your MCP configuration file (`mcp_config.json`):

```json
{
  "mcpServers": {
    "vylor": {
      "command": "C:\\tools\\vylor-mcp.exe",
      "args": [
        "--repo-path", "${workspaceFolder}"
      ]
    }
  }
}
```

---

## 🌐 Running as a Persistent HTTP Daemon (Optional)

If you prefer to run the server on a dedicated network port:

```bash
# Start the server on port 8000
./vylor-mcp.exe --http --host 127.0.0.1 --port 8000 --repo-path "/path/to/project"
```

Connect AI clients to `http://127.0.0.1:8000/mcp`:
```bash
claude mcp add --transport http vylor http://127.0.0.1:8000/mcp
```

---

## ⚡ Background Pre-indexing

- On startup, `vylor-mcp` automatically indexes the target workspace in a background worker thread.
- AST symbol trees and file metadata are cached locally in `.vylor/` inside the target project.
- Queries against the warm cache resolve in **< 2 milliseconds**.
