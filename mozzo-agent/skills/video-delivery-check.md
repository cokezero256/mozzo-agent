# Skill: Video Delivery Check

**Trigger:** Scheduled — daily at 2:00 PM COT + @Mavi delivery check
**Frequency:** Daily
**Output:** #video-delivery channel · DM @mauricio if any critical alerts
**Knowledge files:** `knowledge/clients.md`, `knowledge/team.md`
**Data to fetch:** All ClickUp tasks tagged as video-production, with timestamps

---

## Claude Prompt

```
TASK: Generate the Video Delivery Status Report.

INPUT DATA:
- Current time: {{datetime}}
- All video production tasks: {{video_tasks}}
- Tasks flagged as delayed (>48hrs no status change): {{delayed_tasks}}
- Videos delivered today: {{delivered_today}}
- Videos delivered this week: {{delivered_this_week}}

INSTRUCTIONS:
- A task is DELAYED if it has been in the same status for >48 hours
- A task is CRITICAL if it is overdue by >24hrs AND no editor response
- Calculate overall delivery health: GOOD / AT RISK / CRITICAL
- For delayed tasks, always note hours delayed (not just "delayed")
- Recommend a specific action for each delayed task

OUTPUT FORMAT:
🎬 *MAVI DELIVERY CHECK — {{time}}*

✅ *DELIVERED TODAY:* [list or "None"]
[video title] · [client] · [editor]

🔄 *IN EDITING:* ([count] videos)
[video title] · [client] · [editor] · [Xhrs in editing]

⚠️ *DELAYED (>48hrs):*  ← only show if any exist
🔴 [video title] · [client] · [Xhrs delayed] · Action: [specific next step]

📋 *QUEUED:* [count] videos waiting

━━━━━━━━━━━━━━━━━━━━━
Delivery Health: [🟢 GOOD / 🟡 AT RISK / 🔴 CRITICAL]
Week total: [X] videos delivered

Keep under 250 words.
```

---

## Make.com Flow

```
1. Schedule trigger → 2:00 PM COT daily
   OR Slack Event trigger → app_mention containing "delivery"
2. ClickUp: Get Tasks → filter: tag = "video-production", status != "Delivered"
3. ClickUp: Get Tasks → filter: status = "Delivered", updated_date = today
4. Calculate: time_in_status for each task (Make.com date math)
5. Flag: tasks where time_in_status > 48hrs
6. HTTP POST → Claude API
7. Slack: Post Message → #video-delivery
8. Router: if any CRITICAL tasks → Slack DM → @mauricio
```

---

## Notes

- Critical alerts go to both #video-delivery AND a DM to Mauricio
- If no delays: keep the message short and positive — "All videos on track ✅"
- Track "delivered this week" to give Mauricio a sense of throughput
