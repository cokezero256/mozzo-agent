# Skill: Client Pipeline Monitor

**Trigger:** Scheduled — every Monday at 9:00 AM COT + @Orion pipeline
**Frequency:** Weekly internal report
**Output:** DM to @mauricio — internal only, never sent to clients
**Knowledge files:** knowledge/clients.md
**Data to fetch:** All ClickUp tasks per client, Slack channel last message per client

---

## What this skill does

This is Orion's internal view of the entire client pipeline. It scans every active
client, looks at where their videos are, flags what needs Mauricio's attention, and
identifies which clients have gone quiet. This report is for Mauricio only — it is
never sent to clients.

---

## Claude Prompt

```
TASK: Generate the internal weekly pipeline report for Mauricio.

DATA:
- All active client tasks from ClickUp: {{all_client_tasks}}
- Last message timestamp per client Slack channel: {{last_message_per_client}}
- Current date: {{current_date}}

INSTRUCTIONS:

For each active client, summarize:
- How many videos are in flight and what status each is in
- Whether anything is stuck (same status for more than 48hrs)
- Whether the client has been communicated with in the last 5 days
- Whether there is anything waiting on the client (needs their feedback or approval)
- Whether there is anything waiting on us (overdue edit or revision)

Then give Mauricio a short list of the actions that need to happen today or this week.
Not a health score. Not a dashboard. A list of real things to do.

OUTPUT FORMAT:

Pipeline — [date]

[Client name]
[2-3 sentences describing where their videos are and what is open]
Action needed: [one specific thing, or "none"]

[Next client]
[same format]

---

This week:
- [Specific action 1]
- [Specific action 2]
- [Specific action 3]

Keep the whole thing under 400 words. No emojis. No health scores. No status labels.
Just the facts and what to do about them.
```

---

## Make.com Flow

```
1. Schedule trigger → Monday 9:00 AM COT
2. ClickUp: Get Tasks → all tasks across all lists → status not complete
3. For each client channel: Slack Get Channel Messages → get timestamp of last message
4. HTTP GET → prompts/system-prompt.md from GitHub
5. HTTP GET → skills/client-monitor.md from GitHub
6. HTTP GET → knowledge/clients.md from GitHub
7. HTTP POST → Claude API
8. Slack: Send DM → @mauricio
```
