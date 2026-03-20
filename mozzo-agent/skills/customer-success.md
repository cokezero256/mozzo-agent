# Skill: Customer Success Manager

**Trigger:** @Mavi cs [client] OR Scheduled weekly check-ins OR Webhook from onboarding
**Frequency:** On-demand + Weekly per client
**Output:** DM @mauricio with recommended action + optional DM to client
**Knowledge files:** `knowledge/clients.md`, `knowledge/contentops-sop.md`
**Data to fetch:** ClickUp tasks for client, last email threads, delivery history

---

## Claude Prompt — Client Check-In

```
TASK: Run a Customer Success check-in for {{client_name}}.

CLIENT DATA:
- Recent tasks (last 14 days): {{client_tasks}}
- Emails from client (last 14 days): {{client_emails}}
- Videos delivered this month: {{delivered_count}}
- Open issues or flags: {{open_issues}}
- Client since: {{client_since}}
- Current plan/service: {{client_service}}

INSTRUCTIONS:
Assess this client across 4 dimensions:
1. DELIVERY — Are we delivering on time and to standard?
2. COMMUNICATION — Is the client engaged and responsive?
3. SATISFACTION — Any signals of dissatisfaction (complaints, silence, reduced volume)?
4. GROWTH — Any opportunity to upsell, expand, or deepen the relationship?

OUTPUT FORMAT:
🤝 *CS CHECK-IN: {{client_name}}*

Delivery: [🟢/🟡/🔴] [one sentence]
Communication: [🟢/🟡/🔴] [one sentence]
Satisfaction: [🟢/🟡/🔴] [one sentence]
Growth: [🟢/🟡/🔴] [one sentence]

*OVERALL STATUS:* [HEALTHY / NEEDS ATTENTION / AT RISK]

*RECOMMENDED ACTION:*
[Specific, concrete next step for Mauricio — who to contact, what to say, when]

*SUGGESTED MESSAGE TO CLIENT:* ← only if proactive outreach recommended
[Draft a short, warm check-in message Mauricio can send or approve]
```

---

## Claude Prompt — Onboarding Welcome DM

```
TASK: Draft a Slack welcome DM for new client {{client_name}} who just signed.

CLIENT INFO:
- Name: {{client_name}}
- Service: {{service_type}}
- Niche: {{client_niche}}
- Onboarding call scheduled: {{onboarding_date}}

INSTRUCTIONS:
- Tone: warm, professional, confident — they just made a great decision
- Communicate that work is already in motion
- Set expectations for next steps
- Keep it under 120 words
- Do NOT be generic — reference their niche and what we'll build together

OUTPUT: The DM message only.
```

---

## Make.com Flow — Weekly CS Check-Ins

```
1. Schedule trigger → Friday 3:00 PM COT
2. For each active client (iterate):
   a. ClickUp: Get Tasks → filter by client tag, last 14 days
   b. Gmail: Search → from client email, last 14 days
3. HTTP POST → Claude API (check-in prompt per client)
4. Slack DM → @mauricio with all client check-ins consolidated
```

---

## Make.com Flow — On-Demand

```
1. Slack app_mention → "@Mavi cs [client name]"
2. Parse: extract client name
3. ClickUp + Gmail fetch for that client
4. HTTP POST → Claude API
5. Slack: Reply in thread with CS report
```

---

## Notes

- This skill is Mavi's highest-value output — it replaces a full-time CS hire
- Always frame the recommended action as something Mauricio can do in <10 minutes
- "Suggested message to client" should be ready to send — not a template, an actual draft
- Growth signals to watch: client mentions wanting more content, asks about other services, refers someone
