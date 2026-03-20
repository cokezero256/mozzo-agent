# Skill: Client Activity Monitor

**Trigger:** Scheduled — every Monday at 9:00 AM COT
**Frequency:** Weekly
**Output:** #client-intel channel + DM @mauricio if any AT RISK clients
**Knowledge files:** `knowledge/clients.md`, `knowledge/team.md`
**Data to fetch:** ClickUp tasks per client (last 7 days), Gmail client threads

---

## Claude Prompt

```
TASK: Generate the Weekly Client Activity Report.

INPUT DATA:
- Week of: {{week_start}} to {{week_end}}
- Tasks completed per client (last 7 days): {{client_task_summary}}
- Open tasks per client: {{client_open_tasks}}
- Emails from clients (last 7 days): {{client_email_summary}}
- Clients with ZERO activity this week: {{inactive_clients}}
- Client list for reference: [loaded from knowledge/clients.md]

INSTRUCTIONS:
- Assess each active client as HEALTHY / MONITOR / AT RISK
- AT RISK triggers: zero activity 7+ days, delivery complaint, or no response to email
- For each client: 1 sentence status + any action needed
- Flag concentration risk if Reece Phillips has low activity
- Recommended outreach list should be specific (who to contact + why)

OUTPUT FORMAT:
👥 *MAVI CLIENT INTEL — Week of {{week_start}}*

For each client:
━━━━━━━━━━━
📌 *[CLIENT NAME]* · [✅ HEALTHY / ⚠️ MONITOR / 🔴 AT RISK]
Videos delivered: X · Open tasks: X · Last contact: [date]
[One sentence status or flag]

━━━━━━━━━━━
🚨 *NEEDS ATTENTION:* ← only if any AT RISK
[Client name] — [specific reason] — [recommended action]

📞 *RECOMMENDED OUTREACH THIS WEEK:*
1. [Client] — [reason to reach out]
2. [Client] — [reason]

Keep under 400 words.
```

---

## Make.com Flow

```
1. Schedule trigger → Monday 9:00 AM COT
2. ClickUp: Get Tasks → group by client tag, filter: last 7 days
3. Gmail: Search Messages → from known client emails, last 7 days
4. Identify: clients with zero ClickUp activity in 7 days
5. HTTP GET → knowledge/clients.md (from GitHub)
6. HTTP POST → Claude API
7. Slack: Post Message → #client-intel
8. Router: if any AT RISK clients → Slack DM → @mauricio
```

---

## Notes

- Reece Phillips is a concentration risk — always flag if his activity is lower than normal
- "Zero activity" means no tasks moved AND no emails — both must be zero to flag
- If a client just delivered a batch and is between projects, note that (not a risk signal)
