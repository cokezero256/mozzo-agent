# Config: Slack Channels & Users

> Fill in your actual IDs after creating the Slack App.
> Make.com reads this file to know where to post.
> Get IDs by right-clicking a channel/user in Slack → Copy Link → the ID is at the end.

---

## Channels

```json
{
  "ops_briefing":    "C_XXXXXXXXX",
  "video_delivery":  "C_XXXXXXXXX",
  "client_intel":    "C_XXXXXXXXX",
  "alerts":          "C_XXXXXXXXX",
  "general":         "C_XXXXXXXXX"
}
```

## Users (for DMs)

```json
{
  "mauricio":  "U_XXXXXXXXX",
  "carolina":  "U_XXXXXXXXX"
}
```

## Bot Config

```json
{
  "bot_name":        "Mavi",
  "bot_username":    "mavi",
  "timezone":        "America/Bogota",
  "morning_briefing_time": "08:00",
  "delivery_check_time":   "14:00",
  "weekly_report_day":     "Monday",
  "weekly_report_time":    "09:00"
}
```

## GitHub Raw URL Base

```
https://raw.githubusercontent.com/YOUR_USERNAME/mozzo-agent/main/
```

Skill URLs (use these in Make.com HTTP GET modules):
```
skills/morning-briefing.md    → daily 8am
skills/video-delivery-check.md → daily 2pm
skills/client-monitor.md      → monday 9am
skills/dm-reminder.md         → on-demand
skills/on-demand.md            → @Mavi mentions
skills/customer-success.md    → friday 3pm + on-demand
```
