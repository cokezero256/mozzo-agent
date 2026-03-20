# Cowork Skills

> All of Mauricio's Cowork skills, exported and version-controlled here.
> These are the same skills that run in Claude/Cowork — now also readable by Mavi.

---

## What These Are

Cowork skills are instruction sets that tell Claude how to behave for specific tasks.
When Mavi needs to do something complex (write a script, run a content analysis,
design a dashboard), she loads the relevant skill file from this folder.

This also means: if you update a skill here, Mavi uses the new version automatically.
No rebuilds, no redeployments.

---

## Skill Index

### Content & Strategy

| Skill | What it does | When Mavi uses it |
|-------|-------------|------------------|
| `short-form-content-strategy/` | Full Viraler methodology — ideation, validation, scripting, 50 hook types, review rubric | Script writing, content ideation, hook selection, script reviews |
| `instagram-research/` | Apify-powered IG content research — outlier detection, hook analysis | When analyzing competitor Instagram content |
| `youtube-outliers/` | Find viral YouTube videos in any niche | Competitive intelligence, content inspiration |

### Design & Brand

| Skill | What it does | When Mavi uses it |
|-------|-------------|------------------|
| `brand-colors/` | Full Mozzo dark/light mode design system — tokens, typography, components | Any UI, dashboard, or design work |
| `miro-mind-map/` | Generate structured Miro boards from any content | Visualizing scripts, strategies, frameworks |
| `kpi-dashboard-design/` | KPI selection, visualization patterns, dashboard layouts | Building business dashboards |

### Business Intelligence

| Skill | What it does | When Mavi uses it |
|-------|-------------|------------------|
| `market-research-report/` | 50+ page consulting-grade market research reports | Deep industry analysis requests |
| `creating-financial-models/` | DCF, Monte Carlo, scenario planning, sensitivity analysis | Financial modeling requests |
| `grafana-dashboards/` | Production Grafana dashboards for system monitoring | Infrastructure/ops monitoring |

### AI & Prompting

| Skill | What it does | When Mavi uses it |
|-------|-------------|------------------|
| `prompt-engineering-patterns/` | Advanced prompt design patterns, structured outputs, few-shot learning | Improving prompts, building new skills |

### Process & Workflow

| Skill | What it does | When Mavi uses it |
|-------|-------------|------------------|
| `brainstorming/` | Turn ideas into fully-formed specs through dialogue | Before any build or design task |
| `executing-plans/` | Batch execution with checkpoints | Implementing multi-step plans |
| `using-superpowers/` | Meta-skill — how to find and use all other skills | Loaded at conversation start |

---

## How Mavi Loads a Skill

In Make.com, before any complex task:
```
HTTP GET → https://raw.githubusercontent.com/mozzo-media/mozzo-agent/main/cowork-skills/[skill-name]/SKILL.md
→ inject content into Claude API system prompt alongside master system-prompt.md
```

For example, when Mavi runs a script review:
1. Loads `prompts/system-prompt.md` (who she is)
2. Loads `cowork-skills/short-form-content-strategy/SKILL.md` (how to review)
3. Loads `knowledge/clients.md` (client context)
4. Runs the review

---

## Adding a New Skill

1. Create folder: `cowork-skills/[skill-name]/`
2. Add `SKILL.md` — follow the format of existing skills
3. Add entry to this README's index
4. Push to main
5. Reference in relevant Make.com scenarios or Mavi on-demand commands
