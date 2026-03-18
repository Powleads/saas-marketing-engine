# The SaaS Marketing Engine v8.3

**One command. 24 hours of autonomous outreach. Zero marketing team.**

Built to sell [SignalSprint](https://signalsprint.io). Fork it to sell yours.

## What It Does

Runs 24/7 as a Claude Code skill. Night shift discovers prospects, AI-qualifies them, drafts personalized emails. Day shift sends emails in waves, posts Reddit/LinkedIn/YouTube comments, checks for replies, and follows up automatically.

### Day 1 -> Day 7 -> Scale

| Metric | Day 1 | Day 7 | 5 VPS |
|--------|-------|-------|-------|
| Emails sent | 50 | 500 | 2,500/day |
| Prospects discovered | 30 | 150 | 750/day |
| Reddit comments | 15 | 15 | 75/day |
| Proof tool runs | 20 | 100 | 500/day |

## Quick Start

1. Clone into your project root:
   ```bash
   git clone https://github.com/Powleads/saas-marketing-engine .marketing-engine
   ```

2. Copy skill + config to your Claude Code setup:
   ```bash
   cp .marketing-engine/skills/marketing-engine.md .claude/skills/
   cp .marketing-engine/config/config-template.md .claude/config/marketing-engine-config.md
   ```

3. First run — the engine scans your codebase and auto-generates config:
   ```bash
   /marketing-engine setup
   ```

4. Review the generated config, connect Gmail + Google Sheets, then:
   ```bash
   /marketing-engine autopilot
   ```

## Requirements

- [Claude Code](https://claude.com/claude-code) subscription
- Google Workspace account (Gmail + Sheets)
- Playwright (`npx playwright install chromium`)
- Node.js 18+

## Cost

$0/month (excluding Claude Code subscription). No Zapier. No HubSpot. No Apollo. No Instantly.

## How It Works

See the [full breakdown](https://signalsprint.io/engine) or read [docs/how-it-works.md](docs/how-it-works.md).

## Architecture

```
skills/
  marketing-engine.md    # The Claude Code skill — drop into .claude/skills/
config/
  config-template.md     # Copy this, fill in your product details
  example-saas.md        # Filled example for a fictional SaaS
playbooks/
  channels.md            # Platform-specific posting flows
templates/
  voice-rules.md         # Anti-AI detection + brand mention rules
docs/
  how-it-works.md        # Full 24hr flow breakdown
  scaling.md             # VPS duplication guide
  setup-guide.md         # Step-by-step setup
```

## The 24-Hour Cycle

**Night Shift (10PM-6AM)** — Automated preparation
- 10PM: Discover 30 prospects via Google Maps + LinkedIn
- 11PM: Run proof tool (website scans, free tool invites)
- 1AM: Draft personalized emails for all due prospects
- 3AM: Crawl Reddit/YouTube for threads, pre-write comments
- 5AM: Write LinkedIn post, draft DMs, save to tmp/

**Day Shift (7AM-9PM)** — Send + engage
- 7AM: Send all night-prepped drafts + LinkedIn post
- 8AM-12PM: Pipeline discovery + AI qualification
- 9AM-3PM: Send drafts + LinkedIn connects + IG follows
- 1PM-8PM: Reddit + YouTube + LinkedIn + Quora comments
- 9PM: Evaluate, rotate niche, daily summary

## Case Study

This engine was built to sell [SignalSprint](https://signalsprint.io) — a speed-to-lead platform. In 2 days: 62 prospects in pipeline, 19 emails sent in one morning, 1 hot reply from an agency CEO. [Full case study ->](https://signalsprint.io/engine)

## Docs

- [Setup Guide](docs/setup-guide.md) — Step-by-step installation
- [How It Works](docs/how-it-works.md) — Full technical breakdown
- [Scaling Guide](docs/scaling.md) — VPS duplication for volume
- [Channel Playbooks](playbooks/channels.md) — Platform-specific flows
- [Voice Rules](templates/voice-rules.md) — Anti-AI detection rules

## License

MIT — do whatever you want with it.

Built by [James Taylor](https://linkedin.com/in/jamestaylor-signal-sprint) while building [SignalSprint](https://signalsprint.io).
