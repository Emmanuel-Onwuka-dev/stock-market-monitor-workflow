# stock-market-monitor-workflow
# 📈 Stock Market Monitor

An automated daily market digest — scrapes live stock data, classifies it with AI, and delivers a clean summary straight to Telegram every morning. No manual checking required.

## Why

Checking market movers every morning takes time and pulls focus before the workday even starts. This workflow replaces that manual scan with an automated 7am digest, so the summary is waiting in Telegram before you need it.

## How it works

**Trigger:** Runs daily at 7:00 AM (schedule trigger)

1. **Scrape** — pulls live stock market data from Yahoo Finance using Firecrawl
2. **Classify** — Google Gemini analyzes the scraped content and categorizes performance into UP / DOWN / STABLE
3. **Validate** — checks that the AI response returned proper key:value structure
4. **Self-healing retry** — if validation fails, retries up to 3 times before giving up (handles occasional malformed AI output without failing the whole run)
5. **Standardize** — a Code node cleans the AI output into readable fields
6. **Reset state** — clears static data so the next day's run starts fresh
7. **Deliver** — sends the finished digest to Telegram
8. **Error handling** — if all 3 retries are exhausted, sends an alert email instead of failing silently

## Example output

```
📊 Daily Market Digest — July 20, 2026

🟢 UP
AAPL +2.1%, NVDA +3.4%, ...

🔴 DOWN
TSLA -1.8%, ...

⚪ STABLE
MSFT +0.1%, ...
```

## Stack

- **n8n** — orchestration
- **Firecrawl** — web scraping
- **Google Gemini** — classification / analysis
- **Telegram** — delivery
- **Gmail** — error alerting

## Setup

1. Import `workflow.json` into n8n
2. Connect the following credentials:
   - Firecrawl API
   - Google Gemini API
   - Telegram Bot (+ chat ID)
   - Gmail (for error alerts)
3. Adjust the schedule trigger to your preferred time/timezone
4. Activate the workflow

## Known limitations

- Depends on Yahoo Finance's page structure — if their layout changes significantly, the scrape step may need updating
- AI classification occasionally returns malformed output; the self-healing loop retries up to 3 times before falling back to an error alert, so failures are surfaced rather than silent

## Workflow diagram

<img width="1701" height="472" alt="stock market monitor" src="https://github.com/user-attachments/assets/9d3129e5-5221-462d-992e-fd8e920d9e45" />


## License

MIT — feel free to use or adapt this workflow.
