# Vylor MCP Server (`vylor-mcp`)

Fast, AST-aware code intelligence server built on the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/). Provides instant repository structure maps, multi-file retrieval, and symbol definition search for AI coding agents.

> **Zero Dependencies**: Standalone binary. No Python, Node.js, or external runtimes required.

---

## ⚡ Quick Setup (1 Minute)

### 1. Download the Binary
Download the standalone binary for your OS from **[Releases](../../releases)**:
- **Windows**: `vylor-mcp.exe`
- **macOS / Linux**: `vylor-mcp`
  ```bash
  chmod +x vylor-mcp
  # macOS: remove download quarantine flag if prompted by Gatekeeper
  xattr -d com.apple.quarantine vylor-mcp
  ```

*(Save the binary to any convenient path, e.g., `C:\tools\vylor-mcp.exe` or `/usr/local/bin/vylor-mcp`)*

---

### 2. Add to Your AI Tool

By default, `vylor-mcp` automatically indexes your **current workspace**. You only need to pass `--repo-path` if you want to target a specific project folder or when using Claude Desktop.

<details open>
<summary><b>Claude Code (CLI)</b></summary>

Claude Code configures MCP per project. `cd` to your project root and run:

```bash
# Windows
cd C:\path\to\project
claude mcp add vylor "C:\tools\vylor-mcp.exe"

# macOS / Linux
cd /path/to/project
claude mcp add vylor "/usr/local/bin/vylor-mcp"
```

> **Verify**: Launch `claude` in your project and type `/mcp` to confirm `vylor` is connected.
</details>

<details>
<summary><b>Cursor (<code>.cursor/mcp.json</code>)</b></summary>

```json
{
  "mcpServers": {
    "vylor": {
      "command": "C:\\tools\\vylor-mcp.exe"
    }
  }
}
```
*(On macOS / Linux, use `"command": "/usr/local/bin/vylor-mcp"`. To specify a target folder explicitly: `"args": ["--repo-path", "${workspaceFolder}"]`).*
</details>

<details>
<summary><b>Claude Desktop (<code>claude_desktop_config.json</code>)</b></summary>

```json
{
  "mcpServers": {
    "vylor": {
      "command": "C:\\tools\\vylor-mcp.exe",
      "args": ["--repo-path", "C:/path/to/your/project"]
    }
  }
}
```
*(On macOS / Linux, use `"command": "/usr/local/bin/vylor-mcp"` and specify your project path)*
</details>

<details>
<summary><b>VS Code / Antigravity / Cline / Roo Code (<code>mcp_config.json</code>)</b></summary>

```json
{
  "mcpServers": {
    "vylor": {
      "command": "C:\\tools\\vylor-mcp.exe"
    }
  }
}
```
*(On macOS / Linux, use `"command": "/usr/local/bin/vylor-mcp"`. To specify a target folder explicitly: `"args": ["--repo-path", "${workspaceFolder}"]`).*
</details>

---

## 💡 Prompting Your Agent to Use Vylor Tools

AI models may default to generic search tools (e.g. basic grep or shell commands) unless guided. Add this snippet to your project instructions (`.cursorrules`, `CLAUDE.md`, `AGENTS.md`, or your prompt) so the agent automatically uses Vylor's AST intelligence:

```markdown
When exploring the codebase or searching for definitions, always prioritize using Vylor MCP tools:
- Use `request_repo_map` first to understand the workspace structure and architecture.
- Use `find_code_definition` to locate function, class, and type definitions instantly via Tree-sitter AST.
- Use `find_files` for multi-file content searches across the project.
```

---

## 🛠 Available Tools

| Tool | Description |
|---|---|
| **`request_repo_map`** | Generates a token-budgeted, AST-aware structural map of the workspace highlighting architecture and symbol densities. |
| **`find_code_definition`** | Instant Tree-sitter AST symbol lookup for functions, classes, interfaces, and types across any language. |
| **`find_files`** | Multi-file finder that locates files across the repository and extracts full contents or head line previews. |

---

## ⚙️ Advanced: HTTP Server Mode (Optional)

If you prefer to run `vylor-mcp` as a persistent background HTTP service:

```bash
vylor-mcp --http --port 8000
```

Then connect Claude Code or your agent:
```bash
claude mcp add --transport http vylor http://127.0.0.1:8000/mcp
```
