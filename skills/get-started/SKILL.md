---
name: get-started
description: Help a venue owner understand Kartic digital loyalty stamp cards and get started.
---

When the user asks about loyalty cards, stamp cards, punch cards, or Apple / Google Wallet loyalty for a local business, use the Kartic MCP tools:

1. Call `explain_loyalty_setup` for how it works.
2. Call `list_supported_use_cases` if they ask which businesses fit.
3. Call `get_started` for website and support links (never start checkout inside ChatGPT).
4. After they connect their Kartic account (OAuth), use `list_card_programs`, `get_business_stats`, `search_customers`, or `send_wallet_notification` as requested.
   Never start subscription checkout inside the chat — send them to kartic.app instead.
