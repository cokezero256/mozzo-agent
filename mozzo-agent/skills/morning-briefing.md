# Skill: Morning Briefing

**Trigger:** Scheduled — every weekday at 8:00 AM COT
**Frequency:** Daily, Monday–Friday
**Output:** #ops-briefing channel + DM to @mauricio
**Knowledge files:** `knowledge/clients.md`, `knowledge/team.md`
**Data to fetch:** ClickUp tasks — overdue, due today, in-progress, pending review

---

## Claude Prompt

```
TASK: Generate today's Morning Briefing for Mauricio and the Mozzo team.

INPUT DATA:
- Today's date: {{date}}
- Overdue tasks: {{overdue_tasks}}
- Due today: {{due_today_tasks}}
- In progress: {{in_progress_tasks}}
- Pending client review: {{review_tasks}}
- New tasks created yesterday: {{new_tasks}}

INSTRUCTIONS:
- Lead with the most critical issue (if any)
- Be direct — no filler, no preamble
- Flag any client with 2+ overdue tasks as AT RISK
- If nothing is overdue, keep it tight and positive
- End with exactly 3 priorities for today

OUTPUT FORMAT:
📊 *MAVI MORNING BRIEFING — {{date}}*

🔴 *OVERDUE* ← only if there are overdue tasks
[task name] · [client] · [X days overdue] · Assigned: [editor]

⚠️ *DUE TODAY*
[task name] · [client] · Assigned: [editor]

🔄 *IN PROGRESS* ([count] videos)
[task name] · [client] · [X hrs in current status]

👁️ *NEEDS REVIEW* ([count])
[task name] · [client]

📌 *TODAY'S PRIORITIES*
1. [Most critical action]
2. [Second priority]
3. [Third priority]

Keep under 280 words total.
```

---

## Make.com Flow

```
1. Schedule trigger → 8:00 AM COT, Mon-Fri
2. ClickUp: Search Tasks → filter: due_date <= today, status != complete
3. ClickUp: Search Tasks → filter: status = "In Progress"
4. ClickUp: Search Tasks → filter: status = "Pending Review"
5. Format task data into readable variables
6. HTTP GET → raw GitHub URL for this skill file
7. HTTP POST → Claude API (system: system-prompt.md, user: this prompt + data)
8. Slack: Post Message → #ops-briefing
9. Slack: Send DM → @mauricio (same message)
```

---

## Notes

- If ALL tasks are on time: shorten the briefing, make it a green status
- Always include the editor name so Mauricio knows who to follow up with
- Do not list more than 5 tasks per section — summarize if more
