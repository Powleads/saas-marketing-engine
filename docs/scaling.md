# Scaling Guide

How to multiply the marketing engine across VPS instances for serious volume.

## Why Scale?

A single instance handles ~50 emails/day and ~15 Reddit comments. That's plenty for early traction. But if you want to blanket a market:

| Instances | Emails/day | Prospects/day | Comments/day |
|-----------|-----------|---------------|--------------|
| 1 | 50 | 30 | 15 |
| 3 | 150 | 90 | 45 |
| 5 | 250 | 150 | 75 |
| 10 | 500 | 300 | 150 |

## Architecture

Each VPS instance runs independently. No shared state, no coordination needed. Each instance has:

- Its own Gmail account
- Its own Google Sheet CRM
- Its own Playwright browser profile (separate cookies/sessions)
- Its own Claude Code session
- Its own config (different niche or city assignment)

## Setup Per Instance

### 1. Provision a VPS
Any Linux VPS works. Recommended: Ubuntu 22.04, 2GB RAM, 1 vCPU. Cost: ~$5-10/month.

Providers: Hetzner, DigitalOcean, Vultr, Linode.

### 2. Install Dependencies
```bash
# Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Playwright + Chromium
npx playwright install chromium
npx playwright install-deps chromium

# gws CLI
npm install -g @googleworkspace/cli

# Claude Code
npm install -g @anthropic-ai/claude-code
```

### 3. Create a Fresh Gmail Account
Each instance needs its own Gmail. Do NOT share Gmail accounts across instances — this kills deliverability.

- Create a new Google Workspace or Gmail account
- Set up a professional signature
- Add a profile photo (your face, not a logo)

### 4. Warm Up the Gmail Account
New Gmail accounts that blast 50 emails on day 1 get flagged. Follow this warm-up schedule:

| Day | Max Sends | Notes |
|-----|-----------|-------|
| 1-3 | 5/day | Send to real people you know |
| 4-7 | 10/day | Mix of known + cold |
| 8-14 | 20/day | Gradually increase cold |
| 15-21 | 35/day | Monitor bounce rate |
| 22+ | 50/day | Full capacity |

If bounce rate exceeds 5% at any point, pause for 24 hours and clean your list.

### 5. Create a Google Sheet CRM
Each instance gets its own Sheet. Use the same column structure as the template:

```
Business Name | Niche | City | Tier | Email | Phone | Website | Rating | Reviews | FB Ads | IG | FB | Status | Discovery Date | Last Action | Next Action Date | Next Action | Sequence Day | Thread ID | Notes
```

Add a Config tab with: Current Niche Index, Current City Index, Total Runs, Last Run Date, Daily Send Count, Daily Comment Count.

### 6. Authenticate gws CLI
```bash
gws auth login --scopes gmail,sheets,drive
```

### 7. Create Platform Accounts
Each instance needs separate accounts for Reddit, LinkedIn, YouTube, etc. — OR you can have some instances do email-only and others do social-only.

Recommended split:
- **Email instances:** Gmail + Sheets only. No social. Max 50 sends/day each.
- **Social instances:** Reddit + LinkedIn + YouTube. No email. Shared across 2-3 instances via separate accounts.

### 8. Configure the Engine
Copy the config template and customize for this instance:
- Assign specific niches (instance 1 = niche_1, instance 2 = niche_2, etc.)
- Assign specific cities (no overlap between instances)
- Set the Sheet ID for this instance's CRM
- Set Gmail address for this instance

### 9. Set Up Cron
Use the Claude Code cron system or a simple system cron:

```bash
# Run the engine every hour
0 * * * * cd /path/to/project && claude-code "/marketing-engine" >> /var/log/marketing-engine.log 2>&1
```

## Deduplication

The biggest risk with multiple instances is contacting the same prospect twice from different Gmail accounts. Solutions:

### Option A: Geographic Split (Recommended)
Assign each instance different cities. Instance 1 gets Manchester + London. Instance 2 gets Birmingham + Leeds. No overlap possible.

### Option B: Niche Split
Instance 1 handles agencies. Instance 2 handles dentists. Instance 3 handles med spas. No overlap unless a business spans niches.

### Option C: Central Dedup Sheet
Create a shared "Master Dedup" Google Sheet. Before adding a prospect, each instance checks the master sheet for the business name + city. If exists, skip. This adds complexity but allows flexible assignment.

## Platform Rate Limits

### Gmail
- Google Workspace: 2,000 sends/day per account
- Free Gmail: 500 sends/day per account
- In practice, keep under 50/day for cold outreach to avoid spam flags

### Reddit
- ~10 comments/hour before triggering rate limits
- New accounts: much lower limits
- Different per subreddit

### LinkedIn
- ~100 connection requests/week
- ~20-30 profile views/day before warnings
- Content posting: no hard limit but 1/day is optimal

### YouTube
- Comments are rarely rate-limited
- 10-20/day is safe

## Monitoring

Each instance logs to its own `docs/marketing-engine-log.md`. To get a combined view:

1. Create a "Master Dashboard" Google Sheet
2. Each instance writes daily summary stats to a shared tab
3. Check the master sheet for: total pipeline, total sends, reply rate, hot replies

Key metrics to watch:
- **Reply rate:** Should be 5-15% for cold email. Below 3% = fix your copy.
- **Bounce rate:** Keep under 5%. Above 5% = clean your list, check email sources.
- **Positive reply rate:** 1-3% is good for cold. Above 5% = you're onto something.
- **Reddit karma:** Track per account. If dropping, adjust voice/targeting.

## Cost at Scale

| Instances | VPS | Gmail | Total |
|-----------|-----|-------|-------|
| 1 | $0 (local) | $0 | $0 |
| 3 | $15-30/mo | $0-18/mo | $15-48/mo |
| 5 | $25-50/mo | $0-30/mo | $25-80/mo |
| 10 | $50-100/mo | $0-60/mo | $50-160/mo |

Compare to Apollo ($99/mo), Instantly ($37/mo per account), or hiring a marketing person ($4,000+/mo).
