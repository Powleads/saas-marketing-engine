# The SaaS Marketing Engine v9.0

**One command. 24 hours of autonomous outreach. Zero marketing team.**

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

3. First run -- the engine scans your codebase and auto-generates config:
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

## Architecture

The engine uses a **split architecture**: a lean orchestration skill (~265 lines) that references detailed docs for specific capabilities. This keeps the skill file fast to load while giving the engine deep knowledge when it needs it.

```
skills/
  marketing-engine.md        # Orchestration skill (~265 lines) -- drop into .claude/skills/

config/
  config-template.md         # Config template -- copy + fill in your product details
  example-saas.md            # Filled example for a fictional SaaS

docs/
  setup-guide.md             # Step-by-step installation + proof tool setup
  how-it-works.md            # Full 24hr flow breakdown
  mystery-tests.md           # Proof tool: CLI recon + form fill + response tracking
  cli-reference.md           # Gmail, Sheets, Playwright, Gemini CLI commands
  night-agents.md            # 4-6 specialist agents for overnight content prep
  scaling.md                 # VPS duplication guide for volume

playbooks/
  channels.md                # Platform-specific posting flows (Reddit, LinkedIn, YouTube, etc.)

templates/
  voice-rules.md             # Anti-AI detection + brand mention rules
```

### How the split works

The skill file (`skills/marketing-engine.md`) contains:
- **Modes:** Setup, Autopilot, Run
- **Cron schedule:** 18 daily slots across 4 blocks
- **Phase definitions:** What to do in each phase (1-7)
- **Caps and targets:** Per-run and daily limits
- **Voice rules summary:** Quick reference for content quality

When the engine needs details, it reads the relevant doc:
- Posting on Reddit? Read `playbooks/channels.md`
- Running a mystery test? Read `docs/mystery-tests.md`
- Sending emails? Read `docs/cli-reference.md`
- Preparing night content? Read `docs/night-agents.md`

This means you can customize any aspect by editing the relevant doc without touching the skill file.

## The 24-Hour Cycle

**Night Shift (10PM-6AM)** -- Automated preparation
- 10PM: Discover 30 prospects via Google Maps + LinkedIn
- 12AM: Draft personalized emails for all due prospects
- 2AM: Launch specialist agents to prep content in parallel
- 3:30AM: Review agent outputs, pre-write comments
- 5AM: Final prep -- LinkedIn post, DMs, everything staged

**Day Shift (7AM-9PM)** -- Send + engage
- 7AM: Send all night-prepped drafts + LinkedIn post
- 9AM-4PM: Pipeline discovery + proof tool tests + comments
- 5PM-9PM: Evening comments + final sends + daily evaluation

## Proof Tool (Mystery Tests)

The engine can automatically test your prospects' businesses -- submit their contact form as a test customer, measure response time, and use the data in personalized outreach. See [docs/mystery-tests.md](docs/mystery-tests.md) for the full process.

Don't have a proof tool yet? The setup wizard helps you build one:
- **Screenshot + AI Audit** -- screenshot their site, grade it with Gemini Vision
- **Free Calculator** -- build a simple ROI calculator page
- **Full Mystery Test** -- submit their forms and measure response time

## Knowledge Base (Obsidian-Compatible)

The engine's docs are plain markdown files that work as an [Obsidian](https://obsidian.md/) vault. Open your `docs/` folder in Obsidian for:
- Visual graph of all marketing docs
- Backlinks between files
- Nice editor for reviewing and editing config
- Daily log entries as a knowledge base

Not required -- any markdown editor works. But Obsidian makes it easier to see the big picture.

## Docs

- [Setup Guide](docs/setup-guide.md) -- Step-by-step installation + proof tool setup
- [How It Works](docs/how-it-works.md) -- Full technical breakdown
- [Mystery Tests](docs/mystery-tests.md) -- Proof tool reference
- [CLI Reference](docs/cli-reference.md) -- Gmail, Sheets, Playwright, Gemini commands
- [Night Agents](docs/night-agents.md) -- Specialist agent prompts for overnight prep
- [Scaling Guide](docs/scaling.md) -- VPS duplication guide for volume
- [Channel Playbooks](playbooks/channels.md) -- Platform-specific posting flows
- [Voice Rules](templates/voice-rules.md) -- Anti-AI detection rules

## License

MIT -- do whatever you want with it.
