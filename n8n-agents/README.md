# n8n Agents

A collection of AI agent workflows built with [n8n](https://n8n.io), deployed on Railway. Each folder is a self-contained, importable workflow with its own README.

## Projects

| Project | Description | Stack |
|---|---|---|
| [Jafer — Shopping & Styling Assistant](./jafer-shopping-assistant) | Telegram bot for product search (Amazon India) and outfit styling advice, with voice message support | n8n, Gemini 2.5 Flash, Groq Whisper, ScraperAPI |
| [Placement Email Alert](./placement-email-alert) | Watches Gmail for placement/internship emails, summarizes with AI, and posts alerts to Discord | n8n, Gmail API, Gemini, Discord Webhooks |

More projects coming soon.

## How to use these workflows

1. Clone this repo.
2. Import the `workflow.json` from the relevant project folder into your own n8n instance (Workflows → Import from File).
3. Follow the setup steps in that project's README to add your own API credentials.

## Note on credentials

All `workflow.json` files in this repo have had real API keys stripped and replaced with example/placeholder values. You'll need to supply your own credentials for each service used.
