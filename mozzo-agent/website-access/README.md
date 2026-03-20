# Website Repository Access for Mavi

> How to give Mavi read access to the Mozzo website repo
> so she can help make changes, suggest copy edits, and
> understand what's live on the site.

---

## Why This Matters

When Mavi has read access to the website repo, she can:
- Read landing page copy and suggest improvements
- Check if the onboarding page (`onboarding.html`) is up to date
- Help draft copy changes that Mauricio then reviews and pushes
- Reference live site content when helping with client-facing materials

Mavi will NEVER push directly to the site. She reads, suggests, and drafts.
Mauricio or a developer reviews and deploys all changes.

---

## Option 1: Public Repo (Simplest)

If the website repo is or can be made public:
- Mavi can read any file via raw GitHub URL with no authentication
- No token needed
- In Make.com: HTTP GET → `https://raw.githubusercontent.com/[org]/[repo]/main/[file]`

---

## Option 2: Private Repo with Read Token (Recommended)

### Step 1: Create a GitHub Fine-Grained Token
1. GitHub → Settings → Developer Settings → Personal Access Tokens → Fine-grained tokens
2. Name: `mavi-read-access`
3. Repository access: select specific repos → add website repo
4. Permissions: **Contents: Read-only** (nothing else)
5. Generate token → copy it

### Step 2: Add to Make.com
1. Make.com → Connections → HTTP → Create new connection
2. Name: `GitHub Read (Mavi)`
3. Add header: `Authorization: Bearer [YOUR_TOKEN]`
4. Use this connection in any HTTP GET module that reads from GitHub

### Step 3: Test
```
GET https://api.github.com/repos/[org]/[repo]/contents/[path]
Header: Authorization: Bearer [token]
```
Returns file content base64 encoded. Decode it to read.

---

## What Mavi Can Access (once set up)

| File | What Mavi uses it for |
|------|----------------------|
| `index.html` / `landing pages` | Copy review, headline suggestions |
| `onboarding.html` | Verify onboarding flow is current |
| `README.md` | Understand site structure |
| Any page Mauricio specifies | On-demand content review |

---

## Mavi's Website-Related Skills

Once connected, Mavi can respond to commands like:

```
@Mavi check the onboarding page copy
@Mavi is the pricing page up to date with our current offers?
@Mavi draft a copy update for the hero headline
```

She will read the file, analyze it, and draft suggestions — never push changes directly.

---

## Security Rules (Non-Negotiable)

- Token is **read-only** on the website repo only
- Token is stored only in Make.com (never in this repo or Slack)
- Mavi cannot execute code, run deployments, or push commits
- If you rotate the token, update it in Make.com connections

---

## GitHub Org Structure (Recommended)

```
mozzo-media/ (GitHub Organization)
├── mozzo-agent          ← This repo (Mavi's brain + all skills)
├── website              ← Your live site (Mavi has read access)
└── [future repos]       ← Add as needed
```

Creating a GitHub Org (free):
1. github.com → Your profile → Organizations → New organization
2. Name: `mozzo-media`
3. Move relevant repos into the org
4. Invite Carolina as a member with appropriate permissions
