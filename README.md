# Vylor MCP Server (`vylor-mcp`)

Fast, AST-aware code intelligence server built on the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/). 

Provides repository structure maps, multi-file retrieval, and symbol definition search for AI coding agents.

---

## Features & Available Tools

| Tool | Description |
|---|---|
| **`request_repo_map`** | Generates a token-budgeted, AST-aware structural map of the workspace, highlighting directories, symbol densities, and import relationships. |
| **`find_files`** | Multi-file finder that locates files across the repository and extracts full contents or head line previews. |
| **`find_code_definition`** | Instant Tree-sitter AST symbol lookup for functions, classes, interfaces, and types across any language. |

---

## Installation

Clone the repository and install in editable mode:

```bash
git clone https://github.com/vylor-ai/vylor-mcp.git
cd vylor-mcp
pip install -e .
```

To verify the installation:
```bash
vylor-mcp --help
```

---

## Running the Server Locally

> **Note**: The `--repo-path` parameter is **optional**. If omitted, `vylor-mcp` automatically targets the **current working directory**.

### Option 1: Direct stdio Mode (Default)
This is the standard mode used by local AI agents and IDE extensions:

```bash
# Automatically targets the current directory:
vylor-mcp

# Or specify a custom folder:
vylor-mcp --repo-path "/path/to/your/project"
```

### Option 2: HTTP Transport Mode (Persistent Daemon)
Run the server as an always-on local HTTP daemon on a dedicated port:

```bash
# Automatically targets the current directory:
vylor-mcp --http --port 8000

# Or specify a custom folder and host:
vylor-mcp --http --host 127.0.0.1 --port 8000 --repo-path "/path/to/your/project"
```

---

## Connecting to AI Agents & Clients

### 1. Claude Code (CLI)

#### stdio Mode (Recommended)
Run inside your terminal:
```bash
# Targets the current directory automatically:
claude mcp add vylor vylor-mcp

# Or target a specific folder:
claude mcp add vylor vylor-mcp "--" "--repo-path" "/path/to/your/project"
```

#### HTTP Mode (Connecting to running daemon)
```bash
claude mcp add --transport http vylor http://127.0.0.1:8000/mcp
```

---

### 2. Cursor

Create or edit `.cursor/mcp.json` in your workspace root:

```json
{
  "mcpServers": {
    "vylor": {
      "command": "vylor-mcp"
    }
  }
}
```
*(Since Cursor runs the server in the workspace directory, `--repo-path` is optional).*

---

### 3. Claude Desktop

Edit your Claude Desktop configuration file:
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "vylor": {
      "command": "vylor-mcp",
      "args": [
        "--repo-path", "C:/path/to/your/project"
      ]
    }
  }
}
```

---

### 4. VS Code / Antigravity / Cline / Roo Code

Add to your MCP configuration file (`mcp_config.json` or settings):

```json
{
  "mcpServers": {
    "vylor": {
      "command": "vylor-mcp"
    }
  }
}
```

---

## Background Pre-indexing & Caching

- When started, `vylor-mcp` automatically spawns a background thread (`vylor-preindex`) to parse and index the workspace.
- Indexes are cached locally in `.vylor/` inside the target workspace.
- Subsequent startups and queries against the warm cache resolve in **under 3 ms**.
python tests/test_smoke.py
```
