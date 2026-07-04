# AI Newsletter Automation

A daily automated newsletter that pulls AI/tech news and upcoming AI events from multiple sources, summarizes everything into a clean digest with an LLM, and emails it out — a personal "Tech Brief" delivered every morning.

**Stack**: n8n, RSS feeds, SerpAPI, Google Gemini, Gmail

## What it does

Every morning at 8 AM, the workflow:
1. Pulls the latest articles from an AI news RSS feed and TechCrunch's RSS feed.
2. Searches for upcoming AI/tech events in India via SerpAPI (Google Events).
3. Merges all three sources into one dataset.
4. Sends everything to Gemini, which organizes it into a structured newsletter with three sections: AI News Highlights, Technology Updates, and Upcoming AI Events — each item formatted with a headline, 1-2 sentence summary, and link.
5. Emails the finished newsletter via Gmail.

## How it works

```
Schedule Trigger (daily, 8 AM)
         │
         ├──> AI News RSS Feed ───┐
         ├──> TechCrunch RSS Feed ─┤
         └──> SerpAPI (AI Events) ┘
                       │
                       ▼
                    Merge
                       │
                       ▼
                  Aggregate
                       │
                       ▼
            AI Summarizer (Gemini)
        — organizes into newsletter sections
                       │
                       ▼
              Send via Gmail
```

### Key design choices
- **Multiple sources merged before summarization** — rather than sending three separate emails, everything is aggregated into a single dataset so the LLM can produce one cohesive newsletter with clear sections.
- **Strict output formatting in the prompt** — the system message enforces an exact template (headline in caps, summary, link, section separators) so the final email is consistent and readable every single day, regardless of how the source content varies.
- **"Do not invent content" instruction** — the prompt explicitly restricts the LLM to only the provided items, reducing the risk of hallucinated news or fake links.

## Setup

To run this workflow yourself:

1. Import `workflow.json` into your n8n instance.
2. Set up credentials for:
   - **Gmail** (OAuth2, via n8n's Gmail node)
   - **Google Gemini API** (Google AI Studio)
3. Get a [SerpAPI](https://serpapi.com/) key and replace the placeholder in the `HTTP Request` node's `api_key` query parameter.
4. Update the `sendTo` field in the "Send a message" node with your own recipient email address.
5. Adjust the RSS feed URLs or SerpAPI search query if you want different news sources or event locations.
6. Activate the workflow.

> Note: The SerpAPI key and recipient email in `workflow.json` are placeholder values.

## Tech notes
- Schedule Trigger runs once daily at 8 AM; adjust the `triggerAtHour` value for a different delivery time.
- SerpAPI's `google_events` engine is used to search for location-specific events — the search query (`AI Tech Events in India`) can be edited for other regions or topics.
