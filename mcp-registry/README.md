# MCP Registry listing (Kartic)

Publishes the remote MCP at `https://app.kartic.app/mcp` as **`app.kartic/digital-loyalty-stamp-cards`** on [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io).

## Auth (HTTP, domain `kartic.app`)

Proof file is served from the landing site:

`https://kartic.app/.well-known/mcp-registry-auth`

Private key stays **local only** (never commit). Typical layout:

- `mcp-registry/keys/key.pem` (gitignored)
- `mcp-registry/keys/key.hex` (gitignored)

Copy keys from your machine (not this repo) before republishing.

## Republish

```powershell
$pub = "$env:LOCALAPPDATA\mcp-publisher\mcp-publisher.exe"
Set-Location mcp-registry
& $pub login http --domain kartic.app --private-key-file .\keys\key.hex
& $pub validate
& $pub publish
```

## Rotate key

1. Generate a new Ed25519 key + proof line.
2. Update landing `/.well-known/mcp-registry-auth` and deploy.
3. Log in again with the new private key, then publish.
