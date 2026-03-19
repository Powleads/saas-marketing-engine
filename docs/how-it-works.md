# How It Works

The SaaS Marketing Engine runs as a Claude Code skill — an autonomous AI agent that executes a full marketing operation on a 24-hour cycle. No marketing team. No SaaS subscriptions. Just Claude Code + Google Workspace + Playwright.

## The Stack

| Layer | Tool | Cost |
|-------|------|------|
| AI Agent | Claude Code | Subscription |
| Email | Gmail + gws CLI | $0 |
| CRM | Google Sheets + gws CLI | $0 |
| Browser automation | Playwright | $0 |
| Social platforms | Reddit, LinkedIn, YouTube, Quora, IG, FB | $0 |

Total additional cost: $0/month.

## The 24-Hour Cycle

The engine operates on a fixed schedule with two shifts:

### Night Shift (10PM - 6AM)

The night shift is preparation. No outbound contact — just research, writing, and staging.

**10PM — DISCOVER**
The engine opens Playwright, navigates to Google Maps, and searches for businesses matching your ICP. For each result, it extracts: business name, rating, review count, phone, website. Then it visits each website to scrape the owner's email, name, and social handles. New prospects get added to the Google Sheet CRM with status "new".

Simultaneously, it searches LinkedIn for B2B prospects matching your ICP keywords + geography filters.

**11PM — PROOF TOOL**
For each new prospect, the engine visits their website and runs an automated analysis:
- Screenshot homepage + mobile viewport
- AI-evaluates: CTA quality, form UX, trust signals, page speed, mobile experience
- Generates 3-5 specific findings with actionable quick wins
- Saves findings to the prospect's row in the Sheet

These findings get used in Day 6 "proof" emails — personalized, specific, and genuinely useful whether they buy or not.

**1AM — DRAFT EMAILS**
The engine checks the Pipeline sheet for all prospects where Next Action Date = today. For each one, it:
- Pulls the matching email template (keyed by sequence day + niche)
- Substitutes placeholders ({business_name}, {owner_name}, {city}, etc.)
- Creates a Gmail draft via gws CLI
- Logs the draft ID for morning send

**3AM — CONTENT PREP**
The engine crawls your target subreddits and YouTube channels for fresh threads/videos. For each relevant thread, it reads the full post + comments, then pre-writes a comment following the voice rules (no AI patterns, no product mentions, genuine value). Comments get saved to tmp/ for morning posting.

**5AM — FINAL PREP**
Writes the day's LinkedIn post (founder voice, personal insight). Drafts any personalized DMs for Instagram/Facebook prospects. Everything staged in tmp/ ready for the day shift.

### Day Shift (7AM - 9PM)

The day shift is execution. Sending, posting, engaging, discovering.

**7AM — SEND BLITZ**
All night-prepped email drafts get sent via gws CLI. The LinkedIn post goes live. Any scheduled DMs get sent. This is the highest-activity hour.

**8AM-12PM — PIPELINE**
Continue prospect discovery (Google Maps + LinkedIn). AI-qualify new prospects using Gemini. Enrich with CEO names, personal emails, social handles. Add to Sheet.

**9AM-3PM — SEND + WARM**
Send any remaining email drafts. Send LinkedIn connection requests to ICP profiles. Follow prospects on Instagram. Engage with their content.

**1PM-8PM — COMMENTS**
Post pre-written Reddit comments (3-5 per run, 60s spacing). YouTube comments on niche videos. LinkedIn comments on ICP posts. Quora answers to relevant questions.

**9PM — WRAP**
End of day. The engine:
- Counts all actions taken
- Checks reply rates and engagement
- Rotates to the next niche (if running multi-niche)
- Rotates city weekly
- Self-evaluates what worked and what to adjust
- Writes daily summary to log

## The 6 Phases (Per Run)

Every time the engine wakes up, it runs through these phases:

1. **READ STATE** — Load config, check Sheet for current position, read today's log
2. **CHECK REPLIES** — Scan Gmail for prospect replies, classify (positive/objection/not interested/OOO), auto-respond
3. **CHECK SOCIAL** — Check Reddit inbox, LinkedIn notifications, IG DMs for engagement
4. **SEND FOLLOW-UPS** — Send emails for prospects whose Next Action Date is today
5. **DISCOVER** — Find new prospects via Maps/LinkedIn (if in discovery slot)
6. **CONTENT** — Post comments on Reddit/LinkedIn/YouTube/Quora (if in content slot)
7. **WRAP** — Log everything, update Sheet state

## The Email Sequence

6 emails over 21 days, each with a different angle:

| Day | Type | Purpose |
|-----|------|---------|
| 1 | Cold intro | Lead with their problem, introduce your solution |
| 3 | Follow-up | New angle — stat, competitor insight, or quick tip |
| 6 | Proof | Reference website scan findings — specific and useful |
| 10 | Social proof | Case study or testimonial |
| 15 | Objection handler | Address the #1 objection head-on |
| 21 | Break-up | Graceful close, leave the door open |

All templates are fully customizable in the config file. Placeholders get replaced with real data from the prospect's Sheet row.

## Objection Auto-Responses

When the engine detects a reply, it classifies it:

- **Positive** ("interested", "tell me more", "let's chat") — marks as hot_reply, alerts you immediately
- **Price objection** ("too expensive", "what's the cost") — sends price reframe template
- **Competitor objection** ("we use X already") — sends differentiation template
- **Volume objection** ("we don't get enough leads") — sends ROI-at-small-scale template
- **Not interested** ("no thanks", "remove me") — polite close, marks as closed
- **OOO** (auto-reply detected) — pauses for 7 days

## Content Voice

The engine follows strict anti-AI detection rules to ensure all comments sound human:

- No round numbers, filler openers, or corporate buzzwords
- References personal experience, includes honest caveats
- Varies length wildly (1 sentence to 3 paragraphs)
- Reads the full post before commenting — references specific details
- 85% pure value / 10% soft "DM me" / 5% transparent founder
- Never mentions your product name in comments (your profile bio does the selling)

See [templates/voice-rules.md](../templates/voice-rules.md) for the full ruleset.

## Self-Configuration

On first run (`/marketing-engine setup`), the engine scans your codebase:
- Reads README.md, package.json, CLAUDE.md, landing page
- Understands what your product does, who it's for, pricing
- Auto-generates a complete config: ICP, niches, email templates, subreddit targets
- Asks you to review and adjust before going live

This means you don't need to write cold emails or figure out your ICP from scratch — the engine proposes everything based on your existing marketing copy and code.

## Real-World Results

In testing, a single instance of this engine produced the following results in 2 days:

- 62 prospects discovered and added to pipeline
- 19 personalized emails sent in one morning
- 32 Reddit/Quora/YouTube comments posted
- 1 hot reply from an agency CEO

Your results will vary based on your product, market, and how well your config is tuned.
