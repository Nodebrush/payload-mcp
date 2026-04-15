# Claude Code config

These files need to be copied to the project root's `.claude/` folder for Claude Code to pick them up. The `.claude/` directory is gitignored in the main project — this folder is the source of truth.

## Setup (new project or fresh clone)

```bash
cp apps/payload-mcp/claude-config/settings.json .claude/settings.json
cp apps/payload-mcp/claude-config/launch.json .claude/launch.json
```

Then create `apps/payload-mcp/.env`:

```
PAYLOAD_URL=http://localhost:3000
PAYLOAD_API_KEY=<your-api-key-from-payload-admin>
```

## After updating the submodule

If `claude-config/` changed after a `git pull` inside the submodule, re-run the copy commands above and restart Claude Code.

## Files

- `settings.json` — MCP server config. Tells Claude Code to run `apps/payload-mcp` as the `payload-cms` MCP server.
- `launch.json` — Dev server config. Used by the Claude Preview tool to start the frontend (port 5174) and Payload admin (port 3000).
