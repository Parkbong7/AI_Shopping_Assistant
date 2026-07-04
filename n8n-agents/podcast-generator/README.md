# Podcast Generator

Turns any topic into a short, natural-sounding podcast — type a topic into a custom web UI, and get back an AI-written script converted to spoken audio in seconds.

**Live UI**: https://doodle-audio-maker.lovable.app/
**Stack**: n8n (webhook backend), Google Gemini, Murf.ai (text-to-speech), custom frontend (Lovable)

## What it does

1. User types a podcast topic into the **Podcast Studio** web UI.
2. The UI sends the topic to an n8n webhook.
3. Gemini writes a ~2-minute conversational podcast script on that topic — plain text, no headings or formatting, written to sound natural when spoken aloud.
4. The script is sent to Murf.ai's text-to-speech API, which generates a voiced audio file (voice: "Natalie").
5. The generated audio is fetched and returned to the UI, where it plays back to the user.

## How it works

```
Podcast Studio UI (topic input)
         │
         ▼
      Webhook
         │
         ▼
  Basic LLM Chain (Gemini)
  — writes 2-min podcast script
         │
         ▼
  Murf.ai Text-to-Speech API
  — generates voiced audio
         │
         ▼
  Fetch generated audio file
         │
         ▼
  Respond to Webhook
  — returns audio to the UI
```

### Key design choices
- **Webhook-based backend, decoupled frontend** — the n8n workflow acts purely as an API; the UI (built separately on Lovable) is what users interact with, keeping the automation logic and the interface cleanly separated.
- **Strict "plain text only" prompt constraint** — since the output goes straight to text-to-speech, the prompt explicitly avoids headings, labels, or markdown formatting that would otherwise be read aloud awkwardly.
- **Two-minute target length** — keeps generated audio short and listenable rather than requiring users to sit through a long AI ramble.

## Setup

To run this workflow yourself:

1. Import `workflow.json` into your n8n instance.
2. Set up credentials for:
   - **Google Gemini API** (Google AI Studio)
3. Get a [Murf.ai](https://murf.ai/) API key and replace the placeholder in the `HTTP Request` node's `api-key` header.
4. Activate the workflow and copy its webhook URL.
5. Point your frontend (or a tool like Postman) to that webhook URL, sending a POST request with `{ "text": "your topic here" }`.

> Note: The Murf.ai API key in `workflow.json` is a placeholder value.

## Status
Currently running as a personal project — not yet deployed for public/production use.

## Tech notes
- Voice used is Murf.ai's "Natalie" (en-US locale) — swap the `voiceId` parameter to change narrator voice.
- The webhook uses `responseMode: responseNode`, meaning the workflow itself sends the final HTTP response (the audio) back to the caller once processing completes.
