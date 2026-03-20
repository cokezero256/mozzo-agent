# Skill: On-Demand Commands (@Mavi mentions)

**Trigger:** Slack app_mention event — any channel
**Frequency:** Real-time, on-demand
**Output:** Reply in thread where @Mavi was mentioned
**Knowledge files:** All (loaded dynamically based on command)
**Data to fetch:** Depends on command detected

---

## Command Router

When @Mavi is mentioned, detect the command and route accordingly:

| Command contains | Action |
|-----------------|--------|
| "briefing" / "brief me" | Run morning-briefing skill NOW |
| "delivery" / "delivery check" | Run video-delivery-check skill NOW |
| "client [name]" | Pull ClickUp tasks + last email for that client |
| "overdue" / "what's late" | List all overdue tasks across all clients |
| "weekly" / "week summary" | Run client-monitor skill NOW |
| "dm [person] [message]" | Run dm-reminder skill |
| "help" / "what can you do" | Post capability list |
| anything else | Open-ended Q&A (see below) |

---

## Claude Prompt — Command Router

```
TASK: Respond to this Slack mention of @Mavi.

MESSAGE: "{{slack_message_text}}"
FROM: {{sender_name}} ({{sender_role}})
CHANNEL: {{channel_name}}
TIME: {{timestamp}}

DETECTED COMMAND: {{detected_command}} ← parsed by Make.com router
FETCHED DATA: {{relevant_data}} ← fetched based on command

INSTRUCTIONS:
- If this is a known command: execute it and respond with the skill output
- If this is a question: answer it directly using your knowledge of Mozzo
- If you need data you don't have: say exactly what data you need
- Always respond in the same channel/thread — never redirect to another channel
- Keep responses tight — Slack threads should be scannable
- Start your response with a relevant emoji + action taken (e.g. "📊 Pulled the latest delivery data:")

TONE: Direct, confident, helpful. You're a team member, not a help desk.
```

---

## Claude Prompt — Open Q&A Mode

```
TASK: Answer this question from {{sender_name}} about Mozzo operations.

QUESTION: "{{question}}"

KNOWLEDGE AVAILABLE:
- Client list and status [loaded from knowledge/clients.md]
- Team structure [loaded from knowledge/team.md]
- ContentOps methodology [loaded from knowledge/contentops-sop.md]
- ClickUp data (if fetched): {{clickup_data}}

INSTRUCTIONS:
- Answer directly from your knowledge — don't hedge unless genuinely uncertain
- If the answer requires live data you don't have, say: "I don't have that data right now. 
  Want me to pull it? (@mauricio confirm)"
- If the question is outside your scope, say so briefly and redirect
- Max 150 words for Q&A responses

```

---

## Capability List (for "help" command)

```
👋 Hey {{name}}! Here's what I can do:

📊 *Scheduled reports (automatic):*
• Morning briefing — 8am weekdays in #ops-briefing
• Delivery check — 2pm daily in #video-delivery  
• Client intel — Mondays 9am in #client-intel

⚡ *On-demand commands:*
@Mavi briefing           → Run morning briefing now
@Mavi delivery check     → Run delivery status now
@Mavi client [name]      → Status for a specific client
@Mavi what's overdue     → All overdue tasks
@Mavi weekly             → Full week summary
@Mavi dm @[person] [msg] → Send a DM on your behalf
@Mavi [any question]     → Ask me anything about Mozzo ops
```

---

## Make.com Flow

```
1. Slack Event trigger → app_mention (all channels)
2. Extract: message text, sender ID, channel ID
3. Text parser → detect command keyword
4. Router → branch based on detected command:
   Branch A: "briefing" → run briefing flow → respond
   Branch B: "delivery" → run delivery flow → respond
   Branch C: "client [X]" → ClickUp filter by client name → respond
   Branch D: "overdue" → ClickUp overdue tasks → respond
   Branch E: "dm [X] [msg]" → run dm-reminder flow
   Branch F: "help" → post capability list
   Branch Z: anything else → Claude open Q&A mode
5. HTTP POST → Claude API (with appropriate prompt + data)
6. Slack: Post Message → same channel, reply_to: original message timestamp
```
