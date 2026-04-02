# Reader Digests

Daily reading digest generated from [Readwise Reader](https://readwise.io/read) inbox, published to GitHub Pages and emailed via [Resend](https://resend.com).

**Live site:** https://rohitkaulcoder.github.io/reader-digests/

## How it works

A GitHub Actions workflow runs daily at **7:00 AM IST**, triggered externally by [cron-job.org](https://cron-job.org) via `workflow_dispatch`. The GitHub Actions built-in cron schedule has been removed to prevent duplicate runs.

1. **Fetch** — Pulls recent documents from Readwise Reader (new, later, feed) via `@readwise/cli`
2. **Analyze** — Claude (`claude -p`) reads each article and produces triage signals, sharp takes, and quotable excerpts
3. **Publish** — Commits the digest to `_digests/`, which Jekyll serves on GitHub Pages
4. **Email** — Sends the digest as a formatted HTML email via Resend API

## Digest format

- **Today's Top 3** — best articles at a glance
- Triage signals: ⚡ must-read (max 3), 📌 worth a look, 📎 skim
- **Why read this** — 1-2 sentence hook per article
- **Sharp takes** — 3 per article, leading with surprise
- **Quotable** — 1 best quote with context
- Briefs for short articles: 1 sentence + 1 data point

## Setup

### Secrets required (GitHub repo settings)

| Secret | Purpose |
|--------|---------|
| `ANTHROPIC_API_KEY` | Powers Claude for digest generation |
| `READWISE_TOKEN` | Authenticates with Readwise Reader API |
| `RESEND_API_KEY` | Sends digest email via Resend |

### Manual trigger

```bash
gh workflow run daily-digest.yml
```

## Stack

- **Site:** Jekyll on GitHub Pages
- **Automation:** GitHub Actions (triggered via cron-job.org `workflow_dispatch`)
- **AI:** Claude via `@anthropic-ai/claude-code`
- **Reading data:** `@readwise/cli`
- **Email:** Resend API
