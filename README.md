# The SaaS Marketing Engine v10.0

**One command. 24 hours of autonomous outreach. Zero marketing team.**

Now with **Fill-the-Slot** execution + **adaptive priority queue**. 3-5x more actions per slot.

## What's New in v10

- **Fill-the-Slot execution**: Each cron gets a 25-min budget. Priority queue continuously picks the highest-need channel until time runs out. No idle gaps.
- **Adaptive multipliers**: Phase 7d runs nightly, scores each channel on 10 performance signals, auto-adjusts priorities. Today's performance shapes tomorrow's focus.
- **Dynamic priority queue**: `score = (remaining / target) * multiplier`. Undertouched channels get served first. Capped channels get skipped.
- **YouTube commenting fixed**: `document.execCommand('insertText')` bypasses shadow DOM.

## What It Does

Runs 24/7 as a Claude Code skill. Night shift discovers prospects and drafts emails. Day shift sends in waves, posts comments across 9 channels, checks for replies, follows up automatically.

### Day 1 -> Day 7 -> Scale

| Metric | Day 1 | Day 7 | 5 VPS |
|--------|-------|-------|-------|
| Emails sent | 50 | 500 | 2,500/day |
| Prospects discovered | 30 | 150 | 750/day |
| Reddit comments | 30 | 30 (cap) | 150/day |
| LinkedIn comments | 20 | 20 (cap) | 100/day |
| Proof tool runs | 20 | 100 | 500/day |

## Quick Start

### 1. Clone

```bash
git clone https://github.com/Powleads/saas-marketing-engine .marketing-engine
```

### 2. Copy skill + config

```bash
cp .marketing-engine/skills/marketing-engine.md .claude/skills/
mkdir -p .claude/config
cp .marketing-engine/config/config-template.md .claude/config/marketing-engine-config.md
```

### 3. Setup

```bash
/marketing-engine setup
```

Auto-scans your codebase, generates config, opens browser for logins, creates Sheets CRM.

### 4. Run

```bash
/marketing-engine autopilot
```

18 crons activate. Check back in 24 hours.

## How Fill-the-Slot Works

**Before (v8):** Fixed actions per slot, 5 min work, 55 min idle.

**After (v10):** Priority loop runs 25 minutes per cron:

```
START 18:15 -- Budget: 25 min

[Once] Reply check + social notifs (5 min)
[Once] Due emails (1 min)

[Fill Loop]
  Scores: LinkedIn 1.50 > IG 0.90 > Reddit 0.44
  -> LinkedIn x5 (3 min) -- 16 min left
  -> IG DMs x3 (2 min) -- 14 min left
  -> FB msgs x3 (2 min) -- 12 min left
  -> Reddit x3 (2.5 min) -- 9 min left
  -> LinkedIn x5 (3 min) -- 6 min left
  -> Quora x2 (2 min) -- 4 min left
  -> Reddit x3 (2.5 min) -- EXIT

[Wrap] 7 batches, 24 actions in 25 min
```

## Adaptive Multipliers

The engine learns which channels work:

| Signal | Effect |
|--------|--------|
| Channel drove signups | +0.3x |
| Channel drove site traffic | +0.2x |
| Reply rate > 5% | +0.2x |
| Below 50% of target | +0.1x |
| Hit ceiling 3 days | -0.1x |
| Zero engagement all week | -0.2x |
| Rate limited / CAPTCHA | -0.3x |
| Channel banned / broken | -0.5x |

Stored in Google Sheets `Multipliers` tab. Recalculated nightly. Clamped 0.1-2.5.

## Requirements

- [Claude Code](https://claude.com/claude-code) subscription
- Google Workspace (Gmail + Sheets)
- Playwright (`npx playwright install chromium`)
- Node.js 18+

## Cost

**$0/month** (excluding Claude Code subscription).

## Repo Structure

```
skills/marketing-engine.md    # Core skill (~165 lines)
config/config-template.md     # Blank config
config/example-saas.md        # Example SaaS config
docs/                         # Setup guide, CLI reference, scaling
playbooks/channels.md         # Per-channel playbooks
templates/voice-rules.md      # Anti-AI voice rules
```

## License

MIT

---

Built by [James Taylor](https://linkedin.com/in/jamestaylor-signal-sprint) while building [SignalSprint](https://signalsprint.io).
