# OpenAI review checklist (Kartic)

## Where you are

1. MCP live: `https://app.kartic.app/mcp`
2. OAuth tested (you signed in and tried tools)
3. Plugin draft status: **Development**
4. Next: finish Review details → Submit for review

## Review details (paste into portal)

### Login / demo account

| Field | Value |
| --- | --- |
| Login URL | `https://app.kartic.app/login` |
| Role needed | owner or manager |
| Notes | Dedicated demo merchant with sample stamp cards + customers. No MFA, magic link, or email OTP. Password login only. |

Enter email/password only in the portal’s private Reviewer credentials fields (never in the ZIP).

### Release notes

```text
Initial Kartic ChatGPT app (v0.1.0): remote MCP with public discovery tools and OAuth-connected merchant tools for listing stamp cards, stats, customer search, and Wallet notifications.
```

### 5 positive test cases

**1. Explain loyalty setup**  
- Prompt: `How do digital loyalty stamp cards work with Kartic?`  
- Expected tools: `explain_loyalty_setup`  
- Expected result: English overview of Wallet stamp cards and setup steps; no checkout.

**2. Use cases**  
- Prompt: `Which businesses is Kartic for?`  
- Expected tools: `list_supported_use_cases`  
- Expected result: List of venue types (café, salon, bakery, etc.).

**3. Get started (no checkout)**  
- Prompt: `How do I get started with Kartic?`  
- Expected tools: `get_started`  
- Expected result: Informational links to kartic.app / privacy / support; must not open Polar checkout.

**4. List cards (signed in)**  
- Prompt: `List my stamp cards`  
- Expected tools: `list_card_programs`  
- Expected result: Card names/slugs for the connected demo business.

**5. Stats (signed in)**  
- Prompt: `Show my Kartic stats for the last 30 days`  
- Expected tools: `get_business_stats`  
- Expected result: Analytics snapshot for the demo business.

### 3 negative test cases

**1. Merchant tool before connect**  
- Prompt (signed out / disconnected): `List my stamp cards`  
- Why not complete: No OAuth session.  
- Expected: Ask user to connect / sign in to Kartic; do not invent card data.

**2. Staff cannot search customers**  
- Prompt (staff demo account, if provided): `Find customer Ana`  
- Why not complete: `search_customers` requires owner/manager.  
- Expected: Forbidden / insufficient role message.

**3. Daily notification limit**  
- Prompt (after 3 Wallet sends in 24h): `Send a Wallet notification saying Welcome back`  
- Why not complete: Google Wallet daily cap.  
- Expected: Clear daily limit error; no silent send.

### Video walkthrough

Record ~2–4 min (Loom / YouTube, public or unlisted):

1. Ask discovery prompts 1–3 (no login).
2. Connect OAuth → Allow on `app.kartic.app/oauth/consent`.
3. Run prompts 4–5.
4. Optionally show one negative case (disconnected list cards).

Paste the public URL into Review details.

## Domain challenge

1. Copy token from OpenAI portal (Verify Domain).
2. Set Vercel env `OPENAI_APPS_CHALLENGE_TOKEN` on the app host (`app.kartic.app`) → redeploy.
3. Check: `https://app.kartic.app/.well-known/openai-apps-challenge` returns the token as plain text only.

## Submit

1. Resolve blocking Issues (Metadata & Skills / MCP scan).
2. Save Review details (credentials + tests + video + release notes).
3. **Submit for review** → attestations → status becomes In review.

## ZIP (if re-upload needed)

From `chatgpt-plugin/` (files at ZIP root). Use forward-slash entry names — `Compress-Archive` writes `assets\…` and OpenAI rejects that.

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
