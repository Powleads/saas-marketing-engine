---
name: marketing-engine
description: Autonomous marketing engine for Reddit, LinkedIn, AEO, mystery tests, prospecting, Gmail, Instagram, Facebook outreach. Use for autopilot marketing or setup mode.
---

# Marketing Engine -- Generic SaaS

Autonomous marketing engine. This file is orchestration only -- all reference data lives in docs.

**Read these as needed (not all at once):**
- `.claude/config/marketing-engine-config.md` -- niche data, email templates, cities, objection replies
- `docs/mystery-tests.md` -- proof tool: CLI recon + MCP form fill process
- `docs/cli-reference.md` -- Gmail/Sheets/Gemini/Playwright CLI commands
- `docs/night-agents.md` -- night shift specialist agent prompts
- `playbooks/channels.md` -- channel playbooks, voice rules, hand raiser templates

---

## Modes

### Setup Mode
1. Install Chromium: `npx playwright install chromium`
2. Scan codebase to understand the product
3. Auto-generate config with product details, ICP, niches, email templates
4. Open browser for platform logins
5. Create Google Sheets CRM with Pipeline + Config + Multipliers tabs

### Autopilot Mode
**24-hour continuous execution.** 18 crons. Fill-the-Slot model.

**DESIGN PRINCIPLE: FILL THE SLOT.** Every cron gets a 25-minute budget. Priority queue continuously picks the highest-need channel until budget exhausted.

**CRON SCHEDULE (18 slots):**
- Morning (7-9AM): 2 crons -- send drafts, fill loop
- Business hours (9AM-4PM): 7 crons -- discovery, proof tests, comments
- Evening (5-9PM): 4 crons -- engagement focus, no proof tests
- Night (10PM-6AM): 5 crons -- PREP ONLY, zero audience-facing actions

---

## EXECUTION MODEL -- FILL THE SLOT

### Step 1: Detect Time
```bash
TZ="Your/Timezone" date +"%H:%M"
```

### Step 2: Run Fixed Phases ONCE (~7 min)

**PHASE 1 -- READ STATE:** Read config, sheet, log, dynamic multipliers
**PHASE 2 -- CHECK REPLIES:** Gmail replies, classify, auto-respond to objections
**PHASE 2.5 -- SOCIAL NOTIFICATIONS:** Reddit inbox, LinkedIn notifs, IG DMs, hand raisers
**PHASE 3 -- SEND FOLLOW-UPS:** Pipeline sheet, due emails, template substitution

### Step 3: FILL LOOP (until 25 min budget exhausted)

```
WHILE time_elapsed < 23 min:
  1. Read daily totals from log
  2. Read DYNAMIC multipliers from Multipliers sheet tab
  3. For each channel: score = (remaining / target) * multiplier
     If remaining = 0 -> score = 0
     If TIME-BLOCKED -> score = 0
  4. Pick HIGHEST score channel
  5. Execute ONE BATCH
  6. Log, update totals
  7. If < 2 min remaining -> exit
  8. Recalculate -> repeat
```

### Priority Multipliers (BASE -- overridden by sheet)

| Channel | Base | Why |
|---------|------|-----|
| LinkedIn comments | 1.5x | Profile authority ROI |
| Reddit comments | 1.2x | Authority builder, karma compounds |
| Discovery | 1.1x | Feeds email pipeline |
| Proof tests | 1.1x | High data value |
| IG DMs | 0.9x | Needs consistent touching |
| FB messages | 0.9x | Needs consistent touching |
| LinkedIn connects | 0.8x | Night/morning only |
| Quora answers | 0.6x | Low volume |
| YouTube comments | 0.5x | Lower priority |

### Batch Sizes

| Channel | Batch | Time | Spacing |
|---------|-------|------|---------|
| LinkedIn comments | 5 | ~3 min | 15s |
| Reddit comments | 3 | ~2.5 min | 30s |
| Discovery | 5 | ~5 min | -- |
| Proof tests | 3 | ~5 min | 15s |
| IG DMs | 3 | ~2 min | 30s |
| FB messages | 3 | ~2 min | 30s |
| LinkedIn connects | 10 | ~2 min | 10s |
| Quora answers | 2 | ~2 min | 30s |
| YouTube comments | 2 | ~1.5 min | 15s |
| Gmail sends | 10 | ~1 min | 3s |

### Time-of-Day Constraints

- LinkedIn posts: ONLY 7-9AM or 12-1PM
- Proof tests: BLOCKED outside 9AM-4PM
- LinkedIn connects: BLOCKED outside 22:00-09:00
- Discovery: BLOCKED 5-9PM
- Night (10PM-6AM): ZERO audience-facing actions
- Comments (Reddit, LinkedIn, YouTube, Quora): ANY time

### Step 4: PHASE 6 -- WRAP UP
Log loop stats, print daily progress.

### Step 5: PHASE 7 -- EVALUATE + ADAPT (20:15 slot ONLY)

**7a.** Pipeline evaluation (reply rates, bounces)
**7b.** Analytics (pageviews, referrers)
**7c.** Daily email report
**7d.** ADAPTIVE MULTIPLIER RECALCULATION:

```
For each channel:
  Start with BASE multiplier
  BOOST: +0.3 signup, +0.2 traffic, +0.2 reply rate >5%, +0.1 underfed
  DECAY: -0.1 ceiling 3 days, -0.2 zero engagement, -0.3 rate limited, -0.5 banned
  Clamp: MIN 0.1, MAX 2.5
  Write to Multipliers!A:B in Sheet
```

**7e.** Trigger daily-recap skill if not run today

---

## Daily Targets

| Channel | Target | Ceiling |
|---------|--------|---------|
| Gmail sends | 75 | 100 |
| Reddit comments | 30 | 40 |
| LinkedIn comments | 20 | 25 |
| LinkedIn connects | 40 | 50 |
| Discovery | 80 | 100 |
| Proof tests | 20 | 30 |
| IG DMs | 25 | 30 |
| FB messages | 25 | 30 |

---

## Content Voice Rules

**Anti-AI:** No round numbers, no filler openers, vary length, read full post, reference specifics, include caveats.
**Brand:** NEVER mention product in comments. 85% pure value / 10% soft DM / 5% transparent founder.
**Platform:** Reddit = pub chat. LinkedIn = founder voice. YouTube = brief casual. Email = professional + warm.

---

## Approval Mode

ALL actions PRE-APPROVED. Only pause for: rate limits, CAPTCHA, expired login, hot reply.

---

## Logging

After EVERY action: `| [HH:MM] | [Channel] | [Action] | [Target] | [Preview] | [Status] |`
After EVERY batch: `| -- | Loop | Batch {N} | {channel} x{count} | {time_left} min left |`
After EVERY run: `| -- | System | Run complete | {batches} batches, {actions} actions | {duration} min |`
