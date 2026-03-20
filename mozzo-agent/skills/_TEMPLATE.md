# Skill Template

> Copy this file to create a new skill. Rename it descriptively (e.g. `invoice-reminder.md`).
> Delete this header block before using.

---

## Skill: [SKILL NAME]

**Trigger:** [Scheduled at X time] OR [Slack mention: "@Mavi [command]"] OR [Webhook]
**Frequency:** [Daily / Weekly / On-demand]
**Output:** [Channel name or DM target]
**Knowledge files to load:** [List which files from /knowledge/ this skill needs]
**Data to fetch:** [What to pull from ClickUp / Gmail / etc. before calling Claude]

---

## Claude Prompt

```
TASK: [What Mavi should do]

INPUT DATA:
- [Variable 1]: {{make_variable_1}}
- [Variable 2]: {{make_variable_2}}

INSTRUCTIONS:
[Specific instructions for this skill — format, tone, what to include/exclude]

OUTPUT FORMAT:
[Exact Slack message format with emoji, sections, etc.]
```

---

## Make.com Flow

```
1. [Trigger type]
2. [Data fetch step 1]
3. [Data fetch step 2]
4. HTTP GET → GitHub raw URL for this skill file
5. HTTP POST → Claude API
6. Slack: Post Message / DM
```

---

## Notes

[Any edge cases, special logic, or reminders for this skill]
