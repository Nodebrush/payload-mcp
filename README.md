# payload-mcp

Payload CMS MCP server for Claude Code. Used as a **git submodule** inside svelteload project monorepos.

Gives Claude Code read/write access to a running Payload CMS instance via its REST API. Not a standalone tool — requires the local `payload-admin` dev server on port 3000 and an API key in `.env`.

The MCP **only targets `http://localhost:3000`**. Contact-sheet generation (`browse_media`) returns local file paths that Claude reads off disk, so the admin must run on the same machine as Claude Code. CRUD tools follow the same rule for consistency. Start the admin with `pnpm --filter payload-admin dev` before using any tool other than `get_schema`.

## Tools provided

| Tool | Description |
|---|---|
| `get_schema` | Returns all collections, globals, blocks, and fields from the project's Payload config |
| `find` | Query a collection with filters, pagination, locale, depth |
| `get_by_id` | Fetch a single document by ID |
| `create` | Create a new document (always saved as draft) |
| `update` | Update an existing document (always saved as draft) |
| `get_global` | Fetch a global document |
| `update_global` | Update a global document (always saved as draft) |
| `update_section` | Update a single block/section within a page |
| `browse_media` | Generate visual contact sheets of all uploaded media |

## Adding to a new project

```bash
git submodule add git@github.com:nodebrush/payload-mcp.git apps/payload-mcp
```

Then follow the setup instructions in `claude-config/README.md`:

```bash
cp apps/payload-mcp/claude-config/settings.json .claude/settings.json
cp apps/payload-mcp/claude-config/launch.json .claude/launch.json
```

Create `apps/payload-mcp/.env`:

```
PAYLOAD_API_KEY=<api-key-from-payload-admin-users>
```

## How credentials work

The MCP connects to the local admin at `http://localhost:3000` using `PAYLOAD_API_KEY` from `apps/payload-mcp/.env` (gitignored). The API key is created in the Payload admin under Users — the Users collection has `useAPIKey: true` enabled.

## `get_schema` and the local config

The `get_schema` tool imports `payloadConfigBase` from `packages/payload-config` directly (TypeScript, not HTTP). This is why the MCP must be inside the monorepo — it reads the project's actual config files to build the schema description.

## Updating this submodule

```bash
cd apps/payload-mcp
git pull origin main
cd ../..
git add apps/payload-mcp
git commit -m "update payload-mcp submodule"
git push
```
