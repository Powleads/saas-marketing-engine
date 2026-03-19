# Setup Guide

Step-by-step instructions to get the marketing engine running for your SaaS product.

## Prerequisites

Before starting, make sure you have:

- [ ] **Node.js 18+** -- [nodejs.org](https://nodejs.org)
- [ ] **Claude Code** -- [claude.com/claude-code](https://claude.com/claude-code) (active subscription)
- [ ] **Google Workspace account** (or regular Gmail) -- for sending emails and managing your CRM
- [ ] **A SaaS product** -- with a landing page, pricing, and a clear idea of who you are selling to

---

## Step 1: Install Playwright + Chromium

Playwright drives all browser automation -- prospect discovery, social posting, proof tool scans, and form fills.

```bash
npx playwright install chromium
```

On Linux servers (VPS), you also need system dependencies:
```bash
npx playwright install-deps chromium
```

Verify it works:
```bash
npx playwright screenshot https://example.com /tmp/test.png --full-page
```

---

## Step 2: Install gws CLI + Authenticate

The Google Workspace CLI handles Gmail (sending/reading emails) and Google Sheets (your CRM).

```bash
npm install -g @googleworkspace/cli
```

Authenticate with your Google account:
```bash
gws auth login --scopes gmail,sheets,drive
```

This opens a browser window for OAuth. Log in with the Gmail account you want to send outreach from.

Verify it worked:
```bash
gws gmail users getProfile --params '{"userId":"me"}'
```

You should see your email address in the response.

---

## Step 3: Set Up Your Knowledge Base (Obsidian Vault)

The engine reads and writes to markdown docs for configuration, logging, and evaluation. We recommend using [Obsidian](https://obsidian.md/) to manage these docs, but any markdown editor works.

### Create the docs structure

```bash
mkdir -p docs
touch docs/marketing-engine-log.md
touch docs/evaluations.md
```

The engine will populate these automatically on each run.

### Optional: Obsidian setup

If you want the full interlinked knowledge base experience:

1. Download [Obsidian](https://obsidian.md/) (free)
2. Open your project's `docs/` folder as an Obsidian vault
3. This gives you a visual graph of all your marketing docs, backlinks between files, and a nice editor for reviewing/editing config

Your docs structure will look like:
```
docs/
  marketing-engine-log.md    # Daily activity log (auto-populated)
  evaluations.md             # Performance evaluation history
  mystery-tests.md           # Proof tool reference (from this repo)
  cli-reference.md           # CLI commands reference (from this repo)
  night-agents.md            # Night shift agent prompts (from this repo)
```

---

## Step 4: Browser Profile Setup

The engine uses a persistent Playwright browser profile so you only need to log in to social platforms once.

The profile is stored at `~/.marketing-engine-browser`. When the engine launches Playwright MCP, it uses:
```
--user-data-dir=~/.marketing-engine-browser --browser=chromium
```

This means your login sessions persist between runs.

---

## Step 5: Log In to All Platforms

Run the setup mode to log in to each platform:

```
/marketing-engine setup
```

The engine will open a browser and navigate to each platform. Log in manually:

1. **Reddit** -- use old.reddit.com (more reliable for automation)
2. **LinkedIn** -- linkedin.com
3. **YouTube** -- youtube.com (same Google account as Gmail)
4. **Instagram** -- instagram.com (optional but recommended)
5. **Facebook** -- facebook.com (optional)
6. **Gmail** -- accounts.google.com (same account as Step 2)

The browser profile saves your sessions so you will not need to log in again.

### Set up your profiles for selling

Before the engine starts posting, make sure your profiles work for you:

**Reddit:**
- Display name: Your real name
- Bio: "Building {your_product} -- {one-liner}. {website_url}"

**LinkedIn:**
- Headline: "Founder of {your_product} | {what you do for customers}"
- About: Your story + what the product does
- Featured: Pin a post about your product

**YouTube:**
- Channel name: Your name
- Description: 1-2 sentences about what you are building + link

---

## Step 6: Create Google Sheet CRM

Create a new Google Sheet with two tabs.

### Tab 1: Pipeline

Add these column headers in row 1:

```
A: Business Name    K: IG
B: Niche            L: FB
C: City             M: Status
D: Tier             N: Discovery Date
E: Email            O: Last Action
F: Phone            P: Next Action Date
G: Website          Q: Next Action
H: Rating           R: Sequence Day
I: Reviews          S: Thread ID
J: FB Ads           T: Notes
```

### Tab 2: Config

```
A1: Current Niche Index    B1: 0
A2: Current City Index     B2: 0
A3: Total Runs             B3: 0
A4: Last Run Date          B4: [leave empty]
A5: Daily Send Count       B5: 0
A6: Daily Comment Count    B6: 0
```

### Get your Sheet ID

Copy the Sheet ID from the URL -- it is the long string between `/d/` and `/edit`:
```
https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID_HERE/edit
```

You will paste this into your config file in the next step.

---

## Step 7: Run `/marketing-engine setup`

This is where the engine configures itself for your product.

Open Claude Code in your project directory and run:
```
/marketing-engine setup
```

The engine will:
1. **Scan your codebase** -- reads README.md, package.json, CLAUDE.md, landing page code, pricing page
2. **Understand your product** -- what it does, who it is for, pricing, differentiators
3. **Auto-generate a complete config** at `.claude/config/marketing-engine-config.md`:
   - Product details and pricing
   - ICP (Ideal Customer Profile) definition
   - 2-3 niche targets with Google Maps search queries and subreddit lists
   - 6-email outreach sequence tailored to your product
   - Objection reply templates
   - City list for geographic targeting
4. **Ask you to review** -- edit anything that does not match reality

### Review the generated config

Open `.claude/config/marketing-engine-config.md` and verify:

- [ ] Product name and one-liner are accurate
- [ ] Pricing is correct
- [ ] ICP description matches your actual customers
- [ ] Cities list is relevant to your market
- [ ] Niche definitions make sense for your business
- [ ] Email templates sound like YOU (not generic AI)
- [ ] Objection replies address your real objections
- [ ] Sheet ID is set correctly

The engine only knows what it can read from your codebase -- you know your market better. Edit anything that is off.

---

## Step 8: Set Up Your Proof Tool

A "proof tool" is anything that generates personalized, specific data about a prospect's business. This powers your Day 6 "proof" email -- the highest-converting email in the sequence because it contains real findings, not generic pitches.

### Do you already have a proof tool?

**Yes -- I have a mystery shopper / website scanner / free calculator / audit tool:**
Set `proof_tool.type` in your config to match:
- `website_scan` -- you screenshot + AI-audit their website
- `free_tool` -- you have a free tool page (e.g., `/tools/calculator`) prospects can use
- `mystery_test` -- you submit their contact form and measure response time
- `case_study` -- you link to relevant case studies instead

**No -- I do not have anything yet:**
The setup wizard will help you build a simple one. Options:

### Option A: Screenshot + AI Audit (easiest, no backend needed)
Uses Playwright CLI to screenshot the prospect's website + Gemini Vision to grade it. Generates a report with CTA quality, mobile experience, trust signals, and quick wins. Takes 2 minutes to set up -- see [mystery-tests.md](mystery-tests.md) for the full process.

### Option B: Free Calculator Page
Build a simple page on your site (e.g., `/tools/calculator`) that:
1. Asks for 3-5 inputs relevant to your product
2. Calculates potential ROI / savings / improvement
3. Shows results immediately
4. Optionally gates the detailed report behind an email capture

The engine will reference this in outreach: "I ran your numbers through our calculator -- here is what I found."

### Option C: Full Mystery Test
The most powerful option. Submit the prospect's contact form as a test customer, measure their response time, then use that data in your outreach. Requires a test email inbox you monitor. See [mystery-tests.md](mystery-tests.md) for the complete process.

---

## Step 9: Proposal Pages (Optional, for B2B)

If you want to create personalized proposal pages for prospects (especially powerful for agency/B2B sales):

You will need a backend to dynamically create and serve pages. Options:
- **Convex** -- real-time backend, great for dynamic pages
- **Supabase** -- PostgreSQL + API, good for CRUD
- **Static pages** -- generate HTML files and host them (simplest, least dynamic)
- **Notion + Super.so** -- create Notion pages and serve them as web pages

The engine can scaffold a basic proposal page system during setup if you tell it your backend stack.

Each proposal page includes:
- Prospect's company name and personalized data
- Your product's value proposition framed for their situation
- Proof tool findings (if available)
- Clear CTA (book a call, start trial, etc.)

---

## Step 10: Start the Engine

Everything is connected. Start the engine:

```
/marketing-engine autopilot
```

The engine will:
1. Create cron jobs for all 18 daily slots (13 day + 5 night)
2. Determine which slot to execute right now
3. Start running through the phases

---

## What to Expect

### Day 1
- 20-30 prospects discovered via Google Maps / LinkedIn
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

---

## Troubleshooting

### "gws auth" fails
- Make sure you are using a Google Workspace or regular Gmail account
- Try `gws auth logout` then `gws auth login` again
- Check that you granted all requested scopes (gmail, sheets, drive)

### Playwright cannot find Chromium
```bash
npx playwright install chromium
npx playwright install-deps chromium  # Linux only
```

### Reddit comments getting removed
- Your account karma may be too low -- build up with 10+ pure value comments first
- Check the subreddit's minimum karma/age requirements
- Add the subreddit to "Restricted Subreddits" in config

### Gmail sends bouncing
- Check the email addresses in your pipeline -- scraping is not always accurate
- If bounce rate exceeds 5%, pause sends and clean the list
- Make sure your Gmail account is warmed up (see [scaling.md](scaling.md))

### Engine not running on schedule
- Check that Claude Code crons are set up
- Alternatively, use system cron as a fallback
- Make sure Claude Code has permissions to run in the background

### Config scan missed something
- The auto-setup reads your codebase but may miss context
- Manually edit `.claude/config/marketing-engine-config.md` with corrections
- Re-run `/marketing-engine setup` to regenerate if needed
