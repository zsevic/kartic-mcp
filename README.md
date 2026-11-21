# Kartic MCP

Agent Plugins package and directory listings for **Kartic** digital loyalty stamp cards.

The MCP server itself runs remotely at:

`https://app.kartic.app/mcp`

This repo holds packaging and directory metadata only (Agent Plugins ZIP, MCP Registry listing).

## Package layout (Agent Plugins)

| Path | Purpose |
| --- | --- |
| `plugin.json` | Manifest + OpenAI listing / review metadata |
| `mcp.json` | Remote Streamable HTTP MCP |
| `skills/` | Onboarding skill |
| `assets/` | Icons for directory listings |
| `REVIEW.md` | OpenAI review checklist |

## Directories

- **ChatGPT App Directory** — build the ZIP (below) and upload in the OpenAI plugins portal.
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
Add-Entry "plugin.json" "plugin.json"
Add-Entry "mcp.json" "mcp.json"
Get-ChildItem assets -File | ForEach-Object { Add-Entry "assets/$($_.Name)" $_.FullName }
Add-Entry "skills/get-started/SKILL.md" "skills/get-started/SKILL.md"
$zip.Dispose()
```

Do not put secrets or demo passwords in the ZIP.
