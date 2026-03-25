# The SaaS Marketing Engine

> Autonomous 7-channel outbound marketing that runs 24/7. Discover prospects, mystery-test their websites, send personalised outreach across email, LinkedIn, Instagram, Reddit, YouTube, and more. Zero marketing team required.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Built with Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code-blueviolet)](https://claude.com/claude-code)
[![Next.js](https://img.shields.io/badge/Next.js-16-black)](https://nextjs.org)
[![Convex](https://img.shields.io/badge/Convex-realtime-orange)](https://convex.dev)

## Why This Exists

Most businesses lose leads because they respond too slowly. Most marketing tools solve the wrong problem -- they help you send more emails, not find the right prospects or prove the problem exists first.

This engine does what a 3-person marketing team would do, autonomously:

1. **Discovers** prospects via Google Maps, Facebook Ad Library, LinkedIn, and agency directories
2. **Mystery-tests** their websites by submitting real enquiries and timing the response
3. **Sends personalised outreach** with their actual mystery test results, not generic templates
4. **Follows up** across 7 channels on a scheduled cadence until they reply or the sequence ends
5. **Learns** which channels perform and auto-adjusts priority every night

Nobody else does autonomous discovery + mystery testing + 7-channel outreach in one system. The closest alternatives are $900-10,000/month AI SDR tools that only do email.

<!-- Screenshots: replace placeholders before launch -->
| | |
|---|---|
| ![Engine Landing Page](docs/screenshots/landing-page.png) | ![Real-Time Dashboard](docs/screenshots/dashboard.png) |
| **Landing Page** | **Real-Time Dashboard** |
| ![Setup Wizard](docs/screenshots/setup-wizard.png) | ![Terminal Output](docs/screenshots/terminal.png) |
| **5-Step Setup Wizard** | **Engine Running in Autopilot** |

## Quick Start

### Prerequisites

- [Claude Code](https://claude.com/claude-code) subscription
- [Node.js](https://nodejs.org) 18+
- Google Workspace account (Gmail + Sheets)
- Playwright: `npx playwright install chromium`

### 1. Clone and configure

```bash
git clone https://github.com/Powleads/saas-marketing-engine.git
cd saas-marketing-engine
cp .env.example .env.local
# Fill in your API keys
```

### 2. Run setup

```bash
/marketing-engine setup
```

Opens a browser for social logins, creates your Google Sheets CRM, and validates configuration.

### 3. Start the engine

```bash
/marketing-engine autopilot
```

18 cron slots activate. Night shift discovers and drafts. Day shift sends and engages.

## What It Does

### 6 Outbound Channels

| Channel | What it does |
|---------|-------------|
| **Email** | Personalised cold email sequences with mystery test results |
| **LinkedIn** | ICP commenting, connection requests, DMs |
| **Instagram** | DMs to enriched prospects referencing their content |
| **Reddit** | Value-first comments on trending threads in target subreddits |
| **YouTube** | Authority comments on niche-relevant videos |
| **Mystery Testing** | Submits real enquiries, tracks response time, grades A-F |

### Key Features

- **Niche-Specific Templates** -- Pre-built for dental, medspa, plumber, solicitor, agency + generic
- **Mystery Testing** -- Real enquiries, real response times, real audit data in outreach
- **ESP Matching** -- Gmail recipients get emails from GWS, Outlook from M365
- **Plan Limit Enforcement** -- Lite/Pro/Agency tiers with hard action limits
- **Real-Time Dashboard** -- Convex WebSocket updates, no polling
- **Docker Container Isolation** -- Each client engine runs in its own container
- **noVNC Social Login** -- Secure browser session for social auth, credentials never leave container
- **AgentMail Integration** -- Auto-provision branded sending domains + mailboxes
- **Self-Healing Watchdog** -- Token refresh, service restart, health alerts

### How a 24-Hour Cycle Works

**Night (10PM-7AM):** Discover 30+ prospects, enrich contacts, draft emails, pre-write social comments

**Morning (7-9AM):** Send email waves, LinkedIn posts, mystery test submissions

**Business Hours (9AM-5PM):** Social outreach, mystery test checks, reply handling

**Evening (5-9PM):** Reddit/LinkedIn engagement peaks, final reply checks, nightly evaluation

## Architecture

```
+------------------+     +------------------+     +------------------+
|   Next.js 16     |     |     Convex       |     |   Hetzner VPS    |
|   (App Router)   |<--->|   (Real-time)    |<--->|  (Engine Runtime) |
|                  |     |                  |     |                  |
| - Dashboard UI   |     | - engineInstances|     | - Claude Code    |
| - Engine setup   |     | - engineStats    |     | - Playwright     |
| - Mystery test   |     | - engineActions  |     | - Docker         |
| - Pricing page   |     | - mysteryTests   |     | - noVNC          |
+--------+---------+     +--------+---------+     +--------+---------+
         |                         |                        |
         v                         v                        v
+------------------+     +------------------+     +------------------+
|     Clerk        |     |     Stripe       |     |    AgentMail     |
|   (Auth + SSO)   |     |  (Subscriptions) |     | (Email Infra)    |
+------------------+     +------------------+     +------------------+
```

## Pricing

### Self-Hosted (Free)

Clone this repo, bring your own infrastructure:
- Claude Code subscription (~$20-100/mo)
- VPS (~$6/mo on Hetzner)
- Google Workspace (~$12/mo)
- Residential proxy (~$25/mo)

**Total: ~$63-143/month** for unlimited outreach.

### Hosted on SignalSprint

| Plan | Price | Channels | Actions/day | Niches |
|------|-------|----------|-------------|--------|
| **Lite** | $49/mo | Email + Mystery Test | 30 | 1 |
| **Pro** | $149/mo | All 6 | 150 | 3 |
| **Agency** | $299/mo | All 6 + Multi-client | 500 | Unlimited |

[Start free on SignalSprint](https://signalsprint.io/engine)

## FAQ

**Is this legal?** The engine performs the same actions a human marketing assistant would. Mystery tests use publicly available contact forms. All emails include unsubscribe.

**Will my social accounts get banned?** Residential proxies, human-speed delays, and anti-detection voice rules. Use dedicated accounts, not personal profiles.

**How is this different from Instantly/Smartlead?** Those are email-only. This discovers prospects, mystery-tests websites, and engages across 7 channels with real audit data.

**How is this different from AI SDR tools ($900-10K/mo)?** Same or better results at 5-20x lower cost, plus mystery testing and open source transparency.

## Contributing

Most useful areas: new niche templates, channel playbooks, voice rules, Playwright selector fixes.

1. Fork the repo
2. Create a feature branch
3. Submit a PR with a clear description

## License

MIT -- use it however you want.

---

Built by [James Taylor](https://signalsprint.io) while building [SignalSprint](https://signalsprint.io) -- speed-to-lead software that turns intent into first-call wins.

---

## Self-Hosted Installation (VPS)

Want to run everything yourself? Here's the full VPS setup:

### 1. Provision a VPS

```bash
# Hetzner cx22 (~€4/mo) or any Ubuntu 24.04 server with 4GB+ RAM
# After server is created:
ssh root@YOUR_VPS_IP
```

### 2. Install dependencies

```bash
# Docker
curl -fsSL https://get.docker.com | sh

# Node.js 20
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt-get install -y nodejs

# Clone the engine
git clone https://github.com/Powleads/saas-marketing-engine.git /opt/engine
cd /opt/engine
```

### 3. Build the Docker image

```bash
docker build -t signalsprint-engine:latest -f docker/engine/Dockerfile .
```

### 4. Create your client config

```bash
node scripts/engine/generate-config.js \
  --niche dental \
  --brand "Your Business" \
  --cities "London,Manchester" \
  --channels "email,mystery_test,reddit,linkedin,instagram,youtube,twitter" \
  --plan pro \
  --instance-id "my-engine-001" \
  > /opt/engine/config/client-config.json
```

### 5. Run your engine container

```bash
mkdir -p /opt/engine/logs
docker run -d \
  --name my-engine \
  --restart unless-stopped \
  --memory 1g \
  -v /opt/engine/config:/app/config:ro \
  -v /opt/engine/logs:/app/logs \
  -p 6080:6080 \
  -e CLIENT_CONFIG=/app/config/client-config.json \
  -e LOOP_INTERVAL=1200 \
  signalsprint-engine:latest
```

### 6. Check it's running

```bash
docker logs my-engine --tail 20
# Should show: Xvfb running, Chrome running, noVNC running, engine starting
```

### Or skip all of this and use the hosted version

[signalsprint.io/engine](https://signalsprint.io/engine) — 5-minute setup, no VPS, no Docker, no maintenance. From $49/mo.
