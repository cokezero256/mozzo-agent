# Skill: Client Communication & Video Updates

**Trigger:** @Orion update [client] OR @Orion send update [client] OR scheduled proactive checks
**Frequency:** On-demand anytime + automatic scan when video status changes
**Output:** Message sent directly to the client's Slack channel
**Knowledge files:** knowledge/clients.md
**Data to fetch:** All ClickUp tasks for this client, channel history from client Slack channel

---

## What this skill does

This skill manages all outbound communication with clients. It reads the current state
of their videos in ClickUp, reads the recent history in their Slack channel, understands
what has already been communicated, and sends a natural update that moves the conversation
forward. The update is never the same twice. It reads like a human wrote it.

---

## Claude Prompt — Video Status Update to Client

```
TASK: Write a Slack message to send to {{client_name}} with an update on their videos.

CHANNEL HISTORY (last 20 messages from their Slack channel):
{{channel_history}}

CURRENT VIDEO TASKS IN CLICKUP:
{{client_tasks}}

Each task includes: video title, current status, editor assigned, last updated timestamp,
hours in current status, any comments.

TASK STATUSES YOU WILL SEE:
- Brief received — we have the brief, not started yet
- In editing — editor is actively working on it
- Pending review — editing complete, waiting for Mauricio or client to review
- Revision requested — client or team asked for changes
- Revision in progress — editor working on changes
- Delivered — sent to client, waiting for their feedback
- Approved — client confirmed the video is good
- Published — video is live

INSTRUCTIONS:

Read the channel history first. Understand:
- What was the last thing communicated to the client
- Were any promises made (delivery dates, revision timelines)
- Has the client asked anything that has not been answered
- What is the current mood of the conversation

Then look at the ClickUp tasks and understand:
- Which videos are moving forward
- Which videos are waiting on the client (need their feedback or approval)
- Which videos are waiting on us (in editing or revision)
- If any video has been sitting in the same status for too long

Write one Slack message that covers the current state of their work naturally.
The message should feel like an update from a real person who knows their project
intimately. It should acknowledge context from the channel history where relevant.

RULES FOR THE MESSAGE:
- No emojis
- No bullet points — write in sentences and short paragraphs
- No bold text, no headers, no markdown
- Two to four sentences per paragraph, two paragraphs maximum for routine updates
- Vary the opening — never start with the same phrase twice
- If a video was just delivered, mention it by title and say we are waiting for their feedback
- If a video was approved, acknowledge it and mention what is next
- If a video is in revision, give a realistic timeline
- If the client asked a question in the channel history that was not answered, answer it
- If everything is on track with no news, still send a brief natural check-in — never go silent
- Never say "as per our records" or "according to our system" or anything that sounds automated
- End with one clear action item — what you need from them, or what they can expect next

OUTPUT: The Slack message only. Nothing else.
```

---

## Claude Prompt — Follow-Up After No Client Response

```
TASK: Write a follow-up message to {{client_name}} because they have not responded
to the last update sent {{days_since_last_response}} days ago.

CHANNEL HISTORY:
{{channel_history}}

PENDING ITEMS WAITING ON CLIENT:
{{pending_client_action_tasks}}

INSTRUCTIONS:
Write a short, natural follow-up. Not pushy. Not robotic. Just a real person
checking in because there are open items that need their attention.

Reference the specific video titles that are waiting. Be specific about what you
need from them. Keep it to two or three sentences. Make it feel like a genuine
nudge, not an automated reminder.

OUTPUT: The Slack message only.
```

---

## Claude Prompt — Video Delivered Notification

```
TASK: Write a message to {{client_name}} notifying them that a video has been delivered.

VIDEO DETAILS:
- Title: {{video_title}}
- Drive link: {{drive_link}}
- Editor: {{editor_name}}
- Any notes from the editor: {{editor_notes}}

CHANNEL HISTORY (last 10 messages):
{{channel_history}}

INSTRUCTIONS:
Write a short delivery notification. Mention the video by title. Include the link.
Tell them what you need from them — feedback, approval, or just to let you know
if anything needs adjusting. Keep it to three or four sentences maximum.

Make the phrasing feel fresh. If the channel history shows this is the third video
delivered this week, acknowledge the momentum. If this is their first video, make
it feel like a meaningful moment. Read the context and match the energy.

No emojis. No bullet points. No markdown.

OUTPUT: The Slack message only.
```

---

## Make.com Flow — On-Demand Update

```
1. Slack app_mention trigger → "@Orion update [client name]" OR "@Orion send update [client]"
2. Parse: extract client name from mention text
3. ClickUp: Get Tasks → filter by client tag → all statuses → all open tasks
4. Slack: Get Channel Messages → client's Slack channel → last 20 messages
5. HTTP GET → prompts/system-prompt.md from GitHub
6. HTTP GET → skills/customer-success.md from GitHub
7. HTTP GET → knowledge/clients.md from GitHub
8. HTTP POST → Claude API with all data combined
9. Parse response → extract message text
10. Slack: Post Message → client's Slack channel (as Orion)
11. Slack: Reply in thread where @Orion was mentioned → "Sent to [client name]."
```

---

## Make.com Flow — Automatic Delivery Notification

```
1. ClickUp Watch Tasks trigger → status changes to "Pending Review" or "Delivered"
2. Identify which client the task belongs to (via task tag or list)
3. ClickUp: Get Task → pull full task details including title, drive link, editor notes
4. Slack: Get Channel Messages → that client's Slack channel → last 10 messages
5. HTTP GET → prompts/system-prompt.md from GitHub
6. HTTP GET → skills/customer-success.md from GitHub (use Video Delivered prompt)
7. HTTP POST → Claude API
8. Slack: Post Message → client's channel
```

---

## Make.com Flow — Follow-Up Scanner

```
1. Schedule trigger → every day at 10am
2. For each active client:
   a. Slack: Get Channel Messages → last 20 messages from their channel
   b. Check: when was the last message sent by Orion
   c. Check: are there any tasks in status "Delivered" or "Pending Review" older than 48hrs
   d. If yes AND client has not responded → trigger follow-up flow
3. HTTP GET → system-prompt.md + skill file
4. HTTP POST → Claude API with follow-up prompt
5. Slack: Post Message → client channel
```

---

## Notes

- The channel history is the memory. Always fetch it. Never skip it.
- A video title in ClickUp must match what the client knows the video as.
  If the ClickUp task name is a code or an internal reference, use the actual
  video topic from the task description instead.
- If a client has 5 videos in flight, do not list all 5 in one message.
  Focus on the 1 or 2 that need action right now.
- The goal of every message is to move one thing forward. Not to report everything.
