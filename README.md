<div align="center">

<img src="logo.jpg" alt="GTasks MCP" width="480" />

# GTasks MCP

**MCP server for Google Tasks with batch operations, subtasks and task list management.**

[![CI](https://github.com/sudohakan/GTasks-MCP/actions/workflows/ci.yml/badge.svg)](https://github.com/sudohakan/GTasks-MCP/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/sudohakan/GTasks-MCP?style=flat-square)](https://github.com/sudohakan/GTasks-MCP/releases)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Bun](https://img.shields.io/badge/runtime-Bun-f9f1e1?style=flat-square&logo=bun)](https://bun.sh)

</div>

## Features

Google Tasks over MCP, with the operations the base API makes tedious built in. Originally forked from [zcaceres/google-tasks-mcp](https://github.com/zcaceres/google-tasks-mcp).

- **Batch operations**: create or update dozens of tasks in one parallel call.
- **Subtasks**: create a task under a parent, move it between parents, lists or positions.
- **Task list management**: create, rename and delete task lists, not just tasks.
- **Externalized config**: credentials live outside the repo (`~/.config/gtasks-mcp/` or `%APPDATA%\gtasks-mcp\`), with automatic legacy migration.
- **Auto token refresh**: OAuth tokens are refreshed and persisted transparently.
- **Resource URIs**: read any task via `gtasks:///<task_id>`.
- Due date normalization.

## Installation

The build targets the Bun runtime; `node dist/index.js` does not start it.

```bash
git clone https://github.com/sudohakan/GTasks-MCP.git gtasks-mcp
cd gtasks-mcp
bun install
bun run build
```

Place your Google OAuth keys (Desktop App type, downloaded from Google Cloud Console) in the config directory as `gcp-oauth.keys.json`, then authenticate once:

```bash
bun run start auth
```

Add it to your MCP client config:

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
3. [Configure the OAuth consent screen](https://console.cloud.google.com/apis/credentials/consent) (internal is fine for testing)
4. Add scope: `https://www.googleapis.com/auth/tasks`
5. [Create an OAuth Client ID](https://console.cloud.google.com/apis/credentials/oauthclient) (Desktop App type)
6. Download the JSON, rename it to `gcp-oauth.keys.json`, place it in the config directory

</details>

### Configuration

Credentials are stored outside the repository by default.

| Platform | Default path |
|---|---|
| Linux/macOS | `~/.config/gtasks-mcp/` |
| Windows | `%APPDATA%\gtasks-mcp\` |

| Variable | Purpose |
|---|---|
| `GTASKS_MCP_CONFIG_DIR` | Override config directory |
| `GTASKS_MCP_OAUTH_KEYS_PATH` | Override OAuth keys file path |
| `GTASKS_MCP_CREDENTIALS_PATH` | Override credentials file path |

Legacy files in the repo root are migrated to the config directory on first run.

## Usage

### Task operations

| Tool | Description | Required params |
|---|---|---|
| `search` | Full-text search across title and notes | `query` |
| `list` | List tasks across all lists, or one list with `taskListId` | none |
| `create` | Create a task with title, notes, due date; `parent` makes it a subtask | `title` |
| `update` | Update title, notes, status, or due date | `id`, `uri` |
| `delete` | Delete a task | `id`, `taskListId` |
| `clear` | Clear completed tasks from a list | `taskListId` |
| `batch-create` | Create multiple tasks in parallel, subtasks included | `items[]` |
| `batch-update` | Update multiple tasks in parallel | `items[]` |
| `move` | Make a task a subtask, return it to top level, reorder it (`previous`) or move it to another list | `id` |

### Task list operations

| Tool | Description | Required params |
|---|---|---|
| `list-tasklists` | List all task lists with IDs | none |
| `create-tasklist` | Create a new task list | `title` |
| `delete-tasklist` | Delete a task list | `taskListId` |
| `rename-tasklist` | Rename a task list | `taskListId`, `title` |

### Development

| Command | Purpose |
|---|---|
| `bun run build` | Build for production |
| `bun run dev` | Watch mode |
| `bun run start` | Run the server |
| `bun run start auth` | Run the OAuth flow |
| `bun test` | Run tests |

Contributions welcome, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Project structure

```
src/index.ts   server entry, tool registration, auth command
src/Tasks.ts   Google Tasks API calls (tasks, task lists, batch, move)
src/config.ts  config directory, OAuth key and credential paths, legacy migration
src/*.test.ts  unit tests next to the code
dist/          build output (what the MCP client runs)
logo.jpg       README logo
```

## License

MIT, see [LICENSE](LICENSE).
