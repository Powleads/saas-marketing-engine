# Setup Guide

Step-by-step instructions to get the marketing engine running for your SaaS product.

## Prerequisites

Before starting, make sure you have:

- [ ] **Node.js 18+** — [nodejs.org](https://nodejs.org)
- [ ] **Claude Code** — [claude.com/claude-code](https://claude.com/claude-code) (active subscription)
- [ ] **Google account** — Gmail + access to Google Sheets
- [ ] **A SaaS product** — with a landing page, pricing, and a clear ICP

## Step 1: Clone Into Your Project

Clone the marketing engine into your existing project directory:

```bash
cd /path/to/your/project
git clone https://github.com/Powleads/saas-marketing-engine .marketing-engine
```

This creates a `.marketing-engine/` folder inside your project. It won't interfere with your codebase.

## Step 2: Install Playwright

Playwright drives the browser automation for prospect discovery, social posting, and proof tool scans.

```bash
npx playwright install chromium
```

On Linux servers, you may also need system dependencies:
```bash
npx playwright install-deps chromium
```

## Step 3: Install gws CLI

The Google Workspace CLI handles Gmail and Sheets operations.

```bash
npm install -g @googleworkspace/cli
```

## Step 4: Authenticate Google Workspace

```bash
gws auth login --scopes gmail,sheets,drive
```

This opens a browser window for OAuth. Log in with the Gmail account you want to send emails from.

Verify it worked:
```bash
gws gmail users getProfile --params '{"userId":"me"}'
```

You should see your email address in the response.

## Step 5: Copy Skill + Config

Copy the Claude Code skill and config template to your project's Claude Code setup:

```bash
# Create directories if they don't exist
mkdir -p .claude/skills
mkdir -p .claude/config

# Copy skill
cp .marketing-engine/skills/marketing-engine.md .claude/skills/

# Copy config template
cp .marketing-engine/config/config-template.md .claude/config/marketing-engine-config.md
```

## Step 6: Run Auto-Setup

Open Claude Code in your project and run:

```
/marketing-engine setup
```

The engine will:
1. Scan your codebase (README, package.json, landing page, pricing page)
2. Understand what your product does and who it's for
3. Auto-generate a complete config with:
   - Product details and pricing
   - ICP definition
   - 2-3 niche targets with search queries and subreddits
   - 6-email sequence tailored to your product
   - Objection reply templates
4. Save the config to `.claude/config/marketing-engine-config.md`
5. Ask you to review

## Step 7: Review Generated Config

Open `.claude/config/marketing-engine-config.md` and check:

- [ ] Product name and one-liner are accurate
- [ ] Pricing is correct
- [ ] ICP description matches your actual customers
- [ ] Cities list is relevant to your market
- [ ] Niche definitions make sense
- [ ] Email templates sound like YOU (not generic AI)
- [ ] Objection replies address your real objections

Edit anything that's off. The engine only knows what it can read from your codebase — you know your market better.

## Step 8: Create Google Sheet CRM

Create a new Google Sheet with two tabs:

### Tab 1: Pipeline
Add these column headers in row 1:

```
A: Business Name
B: Niche
C: City
D: Tier
E: Email
F: Phone
G: Website
H: Rating
I: Reviews
J: FB Ads
K: IG
L: FB
M: Status
N: Discovery Date
O: Last Action
P: Next Action Date
Q: Next Action
R: Sequence Day
S: Thread ID
T: Notes
```

### Tab 2: Config
Add these in column A:

```
A1: Current Niche Index    B1: 0
A2: Current City Index     B2: 0
A3: Total Runs             B3: 0
A4: Last Run Date          B4: [leave empty]
A5: Daily Send Count       B5: 0
A6: Daily Comment Count    B6: 0
```

Copy the Sheet ID from the URL (the long string between `/d/` and `/edit`) and paste it into your config file under `sheet_id`.

## Step 9: Log Into Social Platforms

The engine uses Playwright with a persistent browser profile, so you only need to log in once.

Run this in Claude Code:
```
/marketing-engine setup
```

When it reaches the "Connect services" step, it will open a browser and navigate to each platform. Log in manually:

1. **Reddit** — old.reddit.com (use old Reddit for reliability)
2. **LinkedIn** — linkedin.com
3. **YouTube** — youtube.com (same Google account as Gmail)
4. **Instagram** — instagram.com (optional)
5. **Facebook** — facebook.com (optional)

The browser profile saves your sessions so you won't need to log in again.

## Step 10: Set Up Platform Profiles

Before the engine starts posting, make sure your profiles are set up to sell:

### Reddit
- Display name: Your real name
- Bio: "Building [product name] — [one-liner]. [website URL]"

### LinkedIn
- Headline: "Founder of [product] | [what you do for customers]"
- About section: Your story + what the product does
- Featured: Pin a post about your product

### YouTube
- Channel name: Your name
- Channel description: 1-2 sentences about what you're building + link

### Quora
- Bio: "Founder of [product] | [one-liner]"
- Credentials: Add relevant topic expertise

## Step 11: Create the Marketing Log

Create a log file for the engine to track daily activity:

```bash
mkdir -p docs
touch docs/marketing-engine-log.md
```

The engine will populate this automatically on each run.

## Step 12: Run Autopilot

Everything is connected. Start the engine:

```
/marketing-engine autopilot
```

The engine will:
1. Create cron jobs for all 20 daily slots (15 day + 5 night)
2. Determine which slot to execute right now
3. Start running through the phases

## What to Expect

### Day 1
- 20-30 prospects discovered via Google Maps/LinkedIn
- 5-10 cold emails sent (if Gmail is warmed up)
- 3-5 Reddit comments posted
- 1 LinkedIn post published
- Engine log shows all actions taken

### Day 3
- 60-90 prospects in pipeline
- Follow-up emails going out automatically
- First replies starting to come in
- Engine auto-classifying replies and responding to objections

### Day 7
- 150+ prospects in pipeline
- Full email sequence running for early prospects
- Daily content posting across all platforms
- Hot replies flagged for your personal attention

### Day 14+
- Engine is fully autonomous
- You only need to handle hot replies and close deals
- Consider scaling with additional VPS instances (see [scaling.md](scaling.md))

## Troubleshooting

### "gws auth" fails
- Make sure you're using a Google Workspace or regular Gmail account
- Try `gws auth logout` then `gws auth login` again
- Check that you granted all requested scopes

### Playwright can't find Chromium
```bash
npx playwright install chromium
npx playwright install-deps chromium  # Linux only
```

### Reddit comments getting removed
- Your account karma may be too low — build up with 10+ value comments first
- Check the subreddit's minimum karma/age requirements
- Add the subreddit to "Restricted Subreddits" in config

### Gmail sends bouncing
- Check the email addresses in your pipeline — scraping isn't always accurate
- If bounce rate exceeds 5%, pause sends and clean the list
- Make sure your Gmail account is warmed up (see [scaling.md](scaling.md))

### Engine not running on schedule
- Check that Claude Code crons are set up: look for cron entries in Claude Code
- Alternatively, use system cron as a fallback
- Make sure Claude Code has permissions to run in the background

### Config scan missed something
- The auto-setup reads your codebase but may miss context
- Manually edit `.claude/config/marketing-engine-config.md` with corrections
- Re-run `/marketing-engine setup` to regenerate if needed
