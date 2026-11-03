# Changelog

All notable changes to this project are documented here.

Format: [Keep a Changelog](https://keepachangelog.com)

## [1.1.0] - 2026-11-03

### Added
- Subtasks: `create` and `batch-create` take `parent`; subtasks keep the given order.
- `move` tool: make a task a subtask, return it to top level, reorder it or move it to another list.
- `list` takes `taskListId` to list a single task list.

### Changed
- Package and server name are `gtasks-mcp`; the server reports the package version.
- README matches the code: 13 tools, `bun` as the run command.

### Removed
- Smithery files (`smithery.yaml`, `Dockerfile`) and the Smithery install path: the Smithery CLI is shut down, and the image ran the Bun build under Node, where it does not start.
- `package-lock.json`; `bun.lock` is the lockfile.

## [1.0.0] - 2026-04-12

### Changed
- Credentials live in a per-user config directory, with legacy migration; automatic token refresh; batch operations and task list CRUD.

## [0.0.1] - 2026-03-22

### Added
- Initial release of Google Tasks MCP server
- Full task CRUD operations (create, read, update, delete)
- Task search functionality with full-text query support
- List all tasks with cursor-based pagination
- List task lists to retrieve task list IDs
- Clear completed tasks from a specific list
- OAuth 2.0 authentication with Google Cloud
- Task resources accessible via `gtasks:///<task_id>` URIs
- Support for task properties: title, notes, status, due date
- Local credential storage for authenticated users
- MCP resource and tool interfaces
- Smithery package registry integration
- Docker support for containerized deployment

### Features
- **Tools**: search, list, list-tasklists, create, update, delete, clear
- **Resources**: Read-only task resources with full metadata
- **Auth**: Browser-based OAuth flow with local credential caching
- **Config**: Desktop app integration via MCP server configuration
