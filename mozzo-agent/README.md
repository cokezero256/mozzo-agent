# mozzo-agent

> The single source of truth for Mavi and Mozzo Media operations.
> Every skill, SOP, knowledge file, and system prompt lives here.

---

## Repository Structure

```
mozzo-agent/
│
├── prompts/                    ← Mavi's brain
│   └── system-prompt.md        ← Master system prompt (load in every API call)
│
├── knowledge/                  ← What Mavi knows about the business
│   ├── clients.md              ← All active clients + health definitions
│   ├── team.md                 ← Team structure, escalation rules
│   └── contentops-sop.md       ← ContentOps methodology summary
│
├── skills/                     ← Mavi's agent capabilities (Make.com scenarios)
│   ├── morning-briefing.md
│   ├── video-delivery-check.md
│   ├── client-monitor.md
│   ├── dm-reminder.md
│   ├── customer-success.md
│   ├── on-demand.md
│   └── _TEMPLATE.md
│
├── cowork-skills/              ← All 13 Cowork skills (exported from Claude)
│   ├── README.md               ← Full skill index
│   ├── short-form-content-strategy/
│   ├── brand-colors/
│   ├── miro-mind-map/
│   ├── kpi-dashboard-design/
│   ├── market-research-report/
│   ├── creating-financial-models/
│   ├── grafana-dashboards/
│   ├── prompt-engineering-patterns/
│   ├── brainstorming/
│   ├── executing-plans/
│   ├── using-superpowers/
│   ├── instagram-research/
│   └── youtube-outliers/
│
├── contentops-sops/            ← Living SOP library for the whole team
│   ├── README.md
│   ├── video-editing-standards.md
│   ├── client-onboarding.md
│   ├── editor-handbook.md
│   └── quality-checklist.md
│
├── website-access/             ← How to give Mavi read access to the website repo
│   └── README.md
│
└── config/
    ├── settings.md             ← Channel IDs, user IDs, bot config
    └── make-integration-guide.md
```

---

## Quick Links

| What | File |
|------|------|
| Mavi's system prompt | `prompts/system-prompt.md` |
| Client list | `knowledge/clients.md` |
| Morning briefing skill | `skills/morning-briefing.md` |
| Short-form strategy | `cowork-skills/short-form-content-strategy/SKILL.md` |
| Editor quality checklist | `contentops-sops/quality-checklist.md` |
| Make.com setup guide | `config/make-integration-guide.md` |

---

## Who Has Access

| Person | Role | Access level |
|--------|------|-------------|
| Mauricio | Owner | Full read/write |
| Carolina | COO | Full read/write |
| Editors | Team | Read-only (SOPs + checklists) |
| Mavi | AI Agent | Read-only (all files via raw URLs) |

---

## GitHub Desktop Setup

1. Download GitHub Desktop: desktop.github.com
2. Clone this repo: File → Clone Repository → mozzo-media/mozzo-agent
3. Any file you edit locally → Commit → Push → team sees it instantly
4. Pull before editing to get latest changes

## How Mavi Reads Files

Any file in this repo can be fetched by Mavi via:
```
https://raw.githubusercontent.com/mozzo-media/mozzo-agent/main/[path/to/file]
```

No authentication needed if repo is public.
See `website-access/README.md` for private repo setup.
