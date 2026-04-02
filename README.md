# Reader Digests

Daily reading digest generated from [Readwise Reader](https://readwise.io/read) inbox, published to GitHub Pages and emailed via [Resend](https://resend.com).

**Live site:** https://rohitkaulcoder.github.io/reader-digests/

## How it works

A GitHub Actions workflow runs daily at **7:00 AM IST**, triggered externally by [cron-job.org](https://cron-job.org) via `workflow_dispatch`. The GitHub Actions built-in cron schedule has been removed to prevent duplicate runs.

1. **Fetch** — Pulls recent documents from Readwise Reader feed via `@readwise/cli`, strips to slim metadata
2. **Stage 1 (Haiku)** — Triages ~30-50 articles, picks top 10 by novelty/depth
3. **Fetch full content** — Downloads full markdown for the top 10 articles only
4. **Stage 2 (Sonnet)** — Analyzes each article in isolated `claude -p` calls (context resets between articles to control cost)
5. **Stage 3 (Haiku)** — Assembles analyses into the final digest with Top 3, themed sections, and signals
6. **Publish** — Commits the digest to `_digests/`, which Jekyll serves on GitHub Pages
7. **Email** — Sends the digest as a formatted HTML email via Resend API

**Cost:** ~$1/run (~$30/month) using Sonnet for analysis quality, Haiku for mechanical tasks.

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
