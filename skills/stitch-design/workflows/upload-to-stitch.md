# File Upload Workflow

When the user wants to upload an image (screenshot, mockup) to a Stitch project,
use the upload script instead of the MCP tool directly (which has base64 size
limits).

## How to get the MCP URL and API Key

Locate your active MCP server configuration file (e.g., 
`.gemini/jetski/mcp_config.json` for Antigravity,
`~/.gemini/extensions/Stitch/gemini-extension.json` for Gemini Extension, 
`~/.gemini/settings.json` for Gemini CLI, `~/.claude.json` for Claude Code,
or the equivalent for your platform) and parse the configuration for the
Stitch MCP server:

- **MCP URL**: Extract the HTTP URL for the Stitch MCP server (e.g., defined
  via `httpUrl` or as an endpoint argument).
- **API Key**: Extract the API key used for authentication (e.g., defined
   in the `X-Goog-Api-Key` header or as an auth argument).

## Script Usage

You can use the absolute path to run the script from anywhere:

```bash
python3 <WORKSPACE_ROOT>/.agent/skills/stitch-design/scripts/upload_to_stitch.py \
  --project-id <PROJECT_ID> \
  --file-path <PATH_TO_FILE> \
  --api-key <API_KEY> \
  [--api-url <STITCH_API_URL>]
```

Alternatively, if you are in the local workspace root, you can use the relative
path:

```bash
python3 .agent/skills/stitch-design/scripts/upload_to_stitch.py \
  --project-id <PROJECT_ID> \
  --file-path <PATH_TO_FILE> \
  --api-key <API_KEY> \
  [--api-url <STITCH_API_URL>]
```

The script auto-detects mime type from the file extension (`.png` → `image/png`, etc.).
The default `--api-url` is `https://staging-stitch.sandbox.googleapis.com`.
The `--api-key` is required.
