# Make.com Integration Guide

> How to wire every skill to Make.com. Step-by-step.

---

## One-Time Setup

### 1. Add your Anthropic API Key to Make.com
- Make.com → Connections → Add → HTTP
- Name: "Claude API"
- Base URL: `https://api.anthropic.com`
- Headers: `x-api-key: YOUR_ANTHROPIC_KEY`, `anthropic-version: 2023-06-01`

### 2. Add your Slack Bot Token
- Make.com → Connections → Slack → Add
- Paste your Bot User OAuth Token from api.slack.com

### 3. Add your GitHub Token (for private repo)
- Make.com → Connections → HTTP → Add
- Header: `Authorization: Bearer YOUR_GITHUB_TOKEN`
- Only needed if repo is private

---

## The Claude API HTTP Module (Master Template)

Every scenario uses this same HTTP call:

```
Module: HTTP → Make a Request
URL: https://api.anthropic.com/v1/messages
Method: POST
Content-Type: application/json

Body:
{
  "model": "claude-sonnet-4-20250514",
  "max_tokens": 1000,
  "system": [
    {
      "type": "text",
      "text": "{{system_prompt_content}}",
      "cache_control": {"type": "ephemeral"}
    }
  ],
  "messages": [
    {
      "role": "user",
      "content": "{{skill_prompt_with_injected_data}}"
    }
  ]
}
```

**Parsing the response in Make.com:**
- Map: `data.content[0].text` → this is Mavi's message
- Use this as the input for your Slack Post Message module

---

## Scenario 1: Morning Briefing

```
Modules:
[Schedule] → 8:00 AM COT, Mon-Fri
[ClickUp: Search Tasks] → overdue tasks
[ClickUp: Search Tasks] → due today
[ClickUp: Search Tasks] → in progress
[ClickUp: Search Tasks] → pending review
[HTTP GET] → GitHub: skills/morning-briefing.md
[HTTP GET] → GitHub: prompts/system-prompt.md
[HTTP POST] → Claude API
[Slack: Post Message] → #ops-briefing
[Slack: Send DM] → @mauricio
```

---

## Scenario 2: Video Delivery Check

```
Modules:
[Schedule] → 2:00 PM COT daily
[ClickUp: Search Tasks] → tag: video-production
[Set Variable] → calculate hours_in_status for each task
[Filter] → flag tasks where hours_in_status > 48
[HTTP GET] → GitHub: skills/video-delivery-check.md
[HTTP GET] → GitHub: prompts/system-prompt.md
[HTTP POST] → Claude API
[Slack: Post Message] → #video-delivery
[Router] → if critical: [Slack: Send DM] → @mauricio
```

---

## Scenario 3: Client Monitor

```
Modules:
[Schedule] → Monday 9:00 AM COT
[ClickUp: Search Tasks] → all tasks, last 7 days
[Gmail: Search Messages] → from client emails, last 7 days
[HTTP GET] → GitHub: skills/client-monitor.md
[HTTP GET] → GitHub: knowledge/clients.md
[HTTP GET] → GitHub: prompts/system-prompt.md
[HTTP POST] → Claude API
[Slack: Post Message] → #client-intel
[Router] → if AT RISK: [Slack: Send DM] → @mauricio
```

---

## Scenario 4: @Mavi On-Demand

```
Modules:
[Slack: Watch Events] → event: app_mention
[Text Parser] → extract command keyword from message text
[Router] → branch by command:
  ├── "briefing" → run Scenario 1 modules
  ├── "delivery" → run Scenario 2 modules
  ├── "client [X]" → [ClickUp filter by client] → Claude
  ├── "overdue" → [ClickUp overdue tasks] → Claude
  ├── "dm [X] [msg]" → run DM Reminder flow
  └── default → Claude open Q&A
[HTTP GET] → GitHub: skills/on-demand.md
[HTTP GET] → GitHub: prompts/system-prompt.md
[HTTP POST] → Claude API
[Slack: Post Message] → same channel, reply_to: {{message.ts}}
```

---

## Scenario 5: DM Reminder

```
Modules:
[Slack: Watch Events] → event: app_mention containing "dm"
[Text Parser] → extract: target person name + message content
[Slack: Get User] → search by name → get user_id
[HTTP GET] → GitHub: skills/dm-reminder.md
[HTTP POST] → Claude API (generate DM text)
[Slack: Send Direct Message] → target user_id
[Slack: Post Message] → original channel, reply: "✅ DM sent to [name]"
```

---

## Cost Estimate

| Scenario | Runs/month | Tokens/run | Monthly cost |
|----------|-----------|------------|-------------|
| Morning briefing | 22 | ~800 | ~$0.50 |
| Delivery check | 30 | ~600 | ~$0.40 |
| Client monitor | 4 | ~1000 | ~$0.10 |
| On-demand @Mavi | ~60 | ~500 | ~$0.65 |
| **TOTAL** | | | **~$1.65/month** |

With prompt caching (90% reduction on system prompt tokens): **under $5/month total**.
