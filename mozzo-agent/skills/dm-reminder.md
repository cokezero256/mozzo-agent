# Skill: DM Reminder

**Trigger:** @Mavi dm [person] [message] OR Scheduled reminders from a reminders config
**Frequency:** On-demand + scheduled
**Output:** Direct message to target user in Slack
**Knowledge files:** `knowledge/team.md`
**Data to fetch:** Slack user ID for target person

---

## Claude Prompt

```
TASK: Draft a Slack DM from Mavi to {{target_person}} about: {{reminder_topic}}

CONTEXT:
- Sender: Mavi (acting on behalf of Mauricio / Mozzo Ops)
- Recipient: {{target_person}} (role: {{target_role}})
- Topic: {{reminder_topic}}
- Tone: {{tone}} ← "friendly", "firm", or "urgent"
- Additional context: {{extra_context}}

INSTRUCTIONS:
- Write as Mavi — professional ops voice, not robotic
- For editors: warm but clear — they're team members, not employees
- For clients: professional, helpful, never pushy
- Keep it short — DMs should be 2-4 lines max
- End with a clear ask or action (never leave it open-ended)
- Do NOT start with "Hi" followed by a comma — use "Hey [name]" for internal, "[Name]," for clients

OUTPUT: The DM message only. No preamble. No labels.
```

---

## Example Commands

```
@Mavi dm @editor-jose remind him the Clover Trading video is due tomorrow
@Mavi dm @alex-temiz check if he got the latest edit
@Mavi dm @carolina let her know the Coinvo task needs reassignment
@Mavi dm @mauricio daily summary is ready in #ops-briefing
```

---

## Make.com Flow

```
1. Slack Event trigger → app_mention containing "dm"
2. Parse: extract target person name + message/topic from the mention text
3. Slack: Find User → search by name → get user_id
4. HTTP POST → Claude API (generate the DM text)
5. Slack: Send Direct Message → target user_id
6. Slack: Reply in thread (where @Mavi was mentioned) → "✅ DM sent to [name]"
```

---

## Scheduled DM Examples (configure in Make.com)

| Schedule | Recipient | Message |
|----------|-----------|---------|
| Every Friday 4pm | @mauricio | "Week wrap-up: here's what was delivered this week + what's queued for Monday" |
| Every Sunday 6pm | @mauricio | "Week ahead: here's what's due this week across all clients" |
| When task = overdue | Assigned editor | "Hey [name], [task] is now overdue — any blockers? Let me know ASAP." |
| When video delivered | @mauricio | "✅ [Video title] for [client] just marked delivered by [editor]" |

---

## Notes

- Never DM a client without Mauricio's explicit trigger — use @Mavi dm [client] only
- For editor DMs: always mention the specific task name, never be vague
- Tone defaults to "friendly" unless "urgent" or "firm" is specified
