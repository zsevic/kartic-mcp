# Kartic

Kartic helps cafés, salons, and local shops run digital loyalty stamp cards in Google Wallet — no customer app to install. This repository packages the Claude plugin, ChatGPT Agent Plugins ZIP metadata, and MCP Registry listing for the remote server at `https://app.kartic.app/mcp`.

## Use it

Ask how Wallet stamp cards work, which businesses fit, or how to get started. After you connect your Kartic merchant account, ask Claude to list your stamp cards, show stats, search customers, or send a Wallet reminder.

## Data

Discovery tools return public product guidance only. Merchant tools send requests to your Kartic account on `app.kartic.app` after OAuth sign-in. The plugin stores nothing itself; account setup and billing stay on kartic.app.

## Package layout

| Path | Purpose |
| --- | --- |
| `.claude-plugin/plugin.json` | Claude directory plugin manifest |
| `.mcp.json` | Remote HTTP MCP for Claude |
| `skills/` | Onboarding skill (shared) |
| `assets/` | Icons for directory listings |
| `chatgpt-plugin/` | OpenAI Agent Plugins ZIP sources |
| `mcp-registry/` | Official MCP Registry `server.json` |
| `REVIEW.md` | OpenAI review checklist |

## Directories

- **Claude directory** — repo root is the plugin (`.claude-plugin/` + `.mcp.json` + `skills/`). Submit as a Plugin bundle; also submit `https://app.kartic.app/mcp` as an MCP connector.
- **ChatGPT App Directory** — build the ZIP from `chatgpt-plugin/` (below) and upload in the OpenAI plugins portal.
- **MCP Registry** — see [`mcp-registry/`](./mcp-registry) (`app.kartic/digital-loyalty-stamp-cards`).

## Build ChatGPT plugin ZIP

From this repo root (forward-slash entry names required on Windows):

```powershell
$dest = Join-Path (Get-Location) "kartic-chatgpt-plugin.zip"
if (Test-Path $dest) { Remove-Item $dest -Force }
Add-Type -AssemblyName System.IO.Compression
Add-Type -AssemblyName System.IO.Compression.FileSystem
$zip = [IO.Compression.ZipFile]::Open($dest, "Create")
function Add-Entry($name, $path) {
  [void][IO.Compression.ZipFileExtensions]::CreateEntryFromFile($zip, (Resolve-Path $path), ($name -replace '\\','/'))
}
Add-Entry "plugin.json" "chatgpt-plugin/plugin.json"
Add-Entry "mcp.json" "chatgpt-plugin/mcp.json"
Get-ChildItem assets -File | ForEach-Object { Add-Entry "assets/$($_.Name)" $_.FullName }
Add-Entry "skills/get-started/SKILL.md" "skills/get-started/SKILL.md"
$zip.Dispose()
```

Do not put secrets or demo passwords in the ZIP.
