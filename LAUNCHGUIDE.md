# Kartic

## Tagline
Digital loyalty stamp cards in Google Wallet for cafés, salons, and local shops.

## Description
Kartic is a remote MCP server for venue owners who run digital loyalty stamp cards in Google Wallet. Public tools explain how Wallet punch cards work, which businesses fit, and how to get started on kartic.app. After OAuth sign-in, merchants can list stamp cards, review stats, search customers, and send Wallet notifications from any MCP-compatible assistant (Claude, ChatGPT, Cursor, and others). Checkout and account setup stay on the website — this server does not start payments inside the chat.

Remote endpoint: `https://app.kartic.app/mcp` (Streamable HTTP). Official registry name: `app.kartic/digital-loyalty-stamp-cards`.

## Setup Requirements
- Kartic merchant account (required for owner/manager tools): Create or sign in at https://app.kartic.app — OAuth connect from your MCP client; no local API keys or env vars.
- Discovery tools (optional): `explain_loyalty_setup`, `list_supported_use_cases`, and `get_started` work without signing in.

## Category
Business Tools

## Use Cases
Loyalty programs, Local business, Customer retention, Wallet marketing, Café operations, Salon CRM

## Features
- Explain how digital loyalty stamp cards work with Google Wallet
- List venue types that fit Kartic (café, salon, bakery, gym, and more)
- Get started links to website, dashboard, privacy, and support (no in-chat checkout)
- List stamp card programs for a connected merchant account
- Pull business analytics for a chosen lookback window
- Search customers by name or phone
- Send Google Wallet push notifications to card holders (daily send limit)

## Getting Started
- "How do digital loyalty stamp cards work with Kartic?"
- "Which businesses is Kartic for?"
- "How do I get started with Kartic?"
- "List my stamp cards"
- "Show my Kartic stats for the last 30 days"
- "Find customer Ana"
- Tool: explain_loyalty_setup — Overview of Wallet stamp cards and setup steps
- Tool: list_supported_use_cases — Industry examples for digital stamp cards
- Tool: get_started — Website and support links; never starts checkout
- Tool: list_card_programs — Card names, slugs, stamps required, and rewards (signed in)
- Tool: get_business_stats — Analytics snapshot for the last N days (owner/manager)
- Tool: search_customers — Find customers by name or phone (owner/manager)
- Tool: send_wallet_notification — Push a Wallet message to holders of one card (owner/manager)

## Tags
loyalty, stamp-card, punch-card, google-wallet, local-business, cafe, salon, retail, customer-retention, marketing, wallet-notifications, remote-mcp

## Documentation URL
https://github.com/zsevic/kartic-mcp

## Health Check URL
https://app.kartic.app/mcp
