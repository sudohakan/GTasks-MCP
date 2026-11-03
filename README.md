<div align="center">

<img src="logo.jpg" alt="GTasks MCP" width="480" />

# Google Tasks MCP Server

**Full-featured MCP server for Google Tasks with batch operations and task list management.**

13 tools · subtasks · batch operations · task list CRUD · automatic token refresh

[![CI](https://github.com/sudohakan/GTasks-MCP/actions/workflows/ci.yml/badge.svg)](https://github.com/sudohakan/GTasks-MCP/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/sudohakan/GTasks-MCP?style=flat-square)](https://github.com/sudohakan/GTasks-MCP/releases)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Bun](https://img.shields.io/badge/runtime-Bun-f9f1e1?style=flat-square&logo=bun)](https://bun.sh)

</div>

---

## What it does

Google Tasks over MCP, with the operations the base API makes tedious built in:

- **Batch operations**: create or update dozens of tasks in a single parallel call, no more one-by-one loops
- **Subtasks**: create a task under a parent, move it between parents, lists or positions
- **Full task list management**: create, rename, delete task lists, not just tasks
- **Externalized config**: credentials live outside the repo (`~/.config/gtasks-mcp/` or `%APPDATA%\gtasks-mcp\`), with automatic legacy migration
- **Auto token refresh**: OAuth tokens are refreshed and persisted transparently
- **Resource URIs**: access any task via `gtasks:///<task_id>` for MCP resource reads

Originally forked from [zcaceres/google-tasks-mcp](https://github.com/zcaceres/google-tasks-mcp), with significant additions: subtasks and `move`, batch operations, task list CRUD, config externalization, auto token refresh, and due date normalization.

## Install

```bash
# 1. Clone and build
git clone https://github.com/sudohakan/GTasks-MCP.git gtasks-mcp
cd gtasks-mcp && bun install && bun run build

# 2. Place your Google OAuth keys
#    Download from Google Cloud Console (Desktop App type)
#    Linux/macOS: ~/.config/gtasks-mcp/gcp-oauth.keys.json
#    Windows:     %APPDATA%\gtasks-mcp\gcp-oauth.keys.json

# 3. Authenticate (one-time)
bun run start auth

# 4. Add to your MCP client config
```

```json
{
  "mcpServers": {
    "gtasks": {
      "command": "bun",
      "args": ["/path/to/gtasks-mcp/dist/index.js"]
    }
  }
}
```

<details>
<summary>Google Cloud project setup</summary>

1. [Create a Google Cloud project](https://console.cloud.google.com/projectcreate)
2. [Enable the Google Tasks API](https://console.cloud.google.com/workspace-api/products)
3. [Configure OAuth consent screen](https://console.cloud.google.com/apis/credentials/consent) (internal is fine for testing)
4. Add scope: `https://www.googleapis.com/auth/tasks`
5. [Create OAuth Client ID](https://console.cloud.google.com/apis/credentials/oauthclient) (Desktop App type)
6. Download JSON, rename to `gcp-oauth.keys.json`, place in config directory

</details>

## Usage

The build targets the Bun runtime; `node dist/index.js` does not start it.

### Task Operations (9 tools)

| Tool | Description | Required Params |
|------|-------------|-----------------|
| `search` | Full-text search across title and notes | `query` |
| `list` | List tasks across all lists, or one list with `taskListId` | -- |
| `create` | Create a task with title, notes, due date; `parent` makes it a subtask | `title` |
| `update` | Update title, notes, status, or due date | `id`, `uri` |
| `delete` | Delete a task | `id`, `taskListId` |
| `clear` | Clear completed tasks from a list | `taskListId` |
| `batch-create` | Create multiple tasks in parallel, subtasks included | `items[]` |
| `batch-update` | Update multiple tasks in parallel | `items[]` |
| `move` | Make a task a subtask, return it to top level, reorder it (`previous`) or move it to another list | `id` |

### Task List Operations (4 tools)

| Tool | Description | Required Params |
|------|-------------|-----------------|
| `list-tasklists` | List all task lists with IDs | -- |
| `create-tasklist` | Create a new task list | `title` |
| `delete-tasklist` | Delete a task list | `taskListId` |
| `rename-tasklist` | Rename a task list | `taskListId`, `title` |

## Configuration

Credentials are stored outside the repository by default.

| Platform | Default Path |
|----------|-------------|
| Linux/macOS | `~/.config/gtasks-mcp/` |
| Windows | `%APPDATA%\gtasks-mcp\` |

### Environment Variables

| Variable | Purpose |
|----------|---------|
| `GTASKS_MCP_CONFIG_DIR` | Override config directory |
| `GTASKS_MCP_OAUTH_KEYS_PATH` | Override OAuth keys file path |
| `GTASKS_MCP_CREDENTIALS_PATH` | Override credentials file path |

Legacy files in the repo root are automatically migrated to the config directory on first run.

## Development

| Command | Purpose |
|---------|---------|
| `bun run build` | Build for production |
| `bun run dev` | Watch mode |
| `bun run start` | Run the server |
| `bun run start auth` | Run OAuth flow |
| `bun test` | Run tests |

Contributions welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

[MIT](LICENSE)
