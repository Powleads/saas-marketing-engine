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
- `docs/marketing-engine-log.md` -- today's activity log (create if missing)

---

## Modes

### Setup Mode (`/marketing-engine setup`)
1. Install Chromium: `npx playwright install chromium 2>/dev/null`
2. Scan the codebase (README, package.json, landing page, pricing) to understand the product
3. Auto-generate `.claude/config/marketing-engine-config.md` with product details, ICP, niches, email templates
4. Ask if they have a proof tool (mystery shopper, website scanner, calculator, proposal generator)
   - If yes: configure `proof_tool.type` in config
   - If no: help build a simple one (Playwright CLI screenshot + Gemini Vision audit, or a free calculator page)
5. Use Playwright MCP with `--user-data-dir=~/.marketing-engine-browser` and `--browser=chromium`
6. Navigate to each platform for login: reddit.com, linkedin.com, instagram.com, facebook.com, accounts.google.com
7. Report login status per platform
8. Tell user: "Setup complete. Run `/marketing-engine autopilot` for 24hr autonomous mode."

### Autopilot Mode (`/marketing-engine autopilot`)
**24-hour continuous execution.** Create ALL crons, then execute the current block immediately.

**DESIGN PRINCIPLE:** No idle gaps. Each cron runs ALL phases back-to-back. If a run finishes in <10 min, double per-run caps next time. If <5 min, triple them.

**MORNING BLOCK (7-9AM, 2 crons):**
```
07:03 -- MORNING BLITZ: Send ALL night-prepped drafts + LinkedIn post (AM peak) + Reddit x5 + LinkedIn comments x5
08:30 -- FOLLOW-UP: Reply check + remaining drafts + IG follows x10 + LinkedIn connects x20 + Reddit x3
```

**BUSINESS HOURS (9AM-4PM, 7 crons every 60 min):**
```
09:15 -- Discovery x10 + proof tool tests x5 + Reddit x3
10:15 -- Discovery x10 + IG DMs x5 + FB msgs x5 + LinkedIn comments x5
11:15 -- Discovery x10 + proof tool tests x5 + Reddit x3 + Quora x2
12:15 -- LinkedIn post (PM peak) + LinkedIn comments x5 + Reddit x3 + send due follow-ups
13:15 -- Discovery x10 + proof tool tests x5 + Reddit x3 + proposals
14:15 -- IG DMs x5 + FB msgs x5 + proof tool tests x5 + LinkedIn comments x5
15:15 -- Final proof tool check + Reddit x3 + YouTube x3 + discovery x5
```

**EVENING BLOCK (5-9PM, 4 crons every 60 min):**
No proof tool tests. Comments + replies + outreach.
```
17:15 -- Reddit x5 + YouTube x3 + reply check
18:15 -- Reddit x5 + Quora x3 + IG DMs x3 + FB msgs x3
19:15 -- LinkedIn comments x5 + final reply check + send any unsent follow-ups
20:15 -- Reddit x5 + LinkedIn comments x5 + Phase 7 evaluate + daily email
```

**NIGHT BLOCK (10PM-6AM, 5 crons -- PREP + AGENTS):**
NO audience-facing actions. NO proof tool tests.
```
22:07 -- DISCOVER: 20 prospects (Maps) + 20 (LinkedIn) + Reddit research
00:07 -- DRAFT EMAILS: Day 1/3/6/10 for all due prospects + proposals
02:07 -- AGENT WAVE: Launch 4 specialist agents in parallel (read docs/night-agents.md)
03:37 -- CONTENT PREP: Review agent outputs + pre-write YouTube comments + draft IG/FB messages
05:07 -- FINAL PREP: Review all content + send daily email if not sent + print summary
```

**KEY RULES:**
- Proof tool tests ONLY 9AM-4PM (businesses must be open for accurate data)
- LinkedIn posts ONLY 7-9AM or 12-1PM (algorithm peak)
- Reddit comments spread across ALL slots
- Night shift: ZERO audience-facing actions

Tell user: "24-hour engine active. 18 crons set (13 day + 5 night). Auto-expires in 3 days."

### Run Mode (default)
Check current time, determine which slot to execute, run all phases.

---

## Browser Configuration

**Playwright MCP** (logged-in sessions): Reddit commenting, LinkedIn, IG/FB DMs, Google Maps extraction, form fills
**Playwright CLI** (stateless, saves tokens): Site screenshots, recon, visual audits -- see `docs/cli-reference.md`

**Rule:** If it does not need login or DOM interaction, use CLI. Read the PNG visually instead of parsing snapshots.

---

## PHASE 1 -- READ STATE (~30s)

1. Read config: `.claude/config/marketing-engine-config.md`
2. Read Sheet Config tab (niche index, city index, caps) -- see `docs/cli-reference.md` for commands
3. Read `docs/marketing-engine-log.md` -- if new day, archive + reset
4. Note START TIME for run duration tracking
5. Print: `"Run starting at {HH:MM}. Niche: {niche} | City: {city} | Pipeline: {count}"`

---

## PHASE 2 -- CHECK REPLIES (~2 min)

1. Gmail: search for unread replies from prospects (see `docs/cli-reference.md`)
2. Classify: POSITIVE -> alert founder. OBJECTION -> auto-reply from config templates. NOT INTERESTED -> close. OOO -> pause.
3. Check proof tool results in Pipeline Sheet columns U/V (see `docs/mystery-tests.md` for status parsing)
4. Log all replies

---

## PHASE 2.5 -- SOCIAL NOTIFICATIONS (~3 min)

1. **Reddit inbox:** MCP -> old.reddit.com/message/inbox/ -> extract unread via `browser_evaluate`
2. **LinkedIn notifs:** MCP -> linkedin.com/notifications/ -> reply to post comments, accept ICP connections
3. **Instagram DMs:** MCP -> instagram.com/direct/inbox/ -> reply to interested prospects
4. **Hand raiser scan:** Check own posts for trigger words (AUDIT, TEST, HOW, interested)
5. Reply voice: no filler, reference their words, 2-3 sentences, anti-AI rules

---

## PHASE 3 -- SEND FOLLOW-UPS (~10s/email)

1. Read Pipeline Sheet -> filter where Next Action Date <= today
2. For each due prospect: read matching template from config, substitute placeholders, create draft, send
3. Day 6 emails include proof tool data -- see `docs/mystery-tests.md`
4. Update Sheet: Status, Last Action, Next Action Date, Sequence Day++, Thread ID

---

## PHASE 4 -- DISCOVER NEW PROSPECTS (~3 min/prospect)

1. Google Maps via MCP -> search niche query + city -> extract via `browser_evaluate`
2. **CLI screenshots for enrichment** (NOT MCP):
   ```bash
   npx playwright screenshot "{website}" /tmp/prospect-homepage.png --full-page
   npx playwright screenshot "{website}/contact" /tmp/prospect-contact.png --full-page 2>/dev/null
   ```
3. Read PNGs visually -> extract email, phone, IG, FB
4. LinkedIn CEO search via MCP -> connect + follow on IG (warm-up before cold email)
5. Score Tier using niche `tier_criteria` from config
6. Add to Sheet with status `new`, Next Action Date = tomorrow

---

## PHASE 4.5 -- PROPOSALS (optional, for B2B niches)

If your config defines a proposal page system:
1. Gather prospect data from Pipeline Sheet
2. Create personalized proposal page via your backend API
3. Include proposal URL in Day 1 email as main CTA
4. Track views if supported

---

## PHASE 5 -- CONTENT CHANNELS

Read `playbooks/channels.md` for detailed per-platform playbooks.

**Content Topics -- ROTATE (max 30% your core topic):**
1. Your core problem space (but not every comment)
2. Proof tool concept -- "test your own business"
3. Marketing automation / systems
4. AI tools for business -- practical, not hype
5. SaaS building -- first customers, validation
6. Industry-specific tactical advice
7. Cold email / outbound
8. Lead quality vs quantity

**Per platform:** use MCP for posting (needs login). Score Reddit posts on trending signals before commenting. Read FULL post + comments before writing. 30s between Reddit, 15s between LinkedIn, 15s between YouTube.

---

## PHASE 6 -- WRAP UP (~1 min)

1. Update engine log with all actions from this run
2. Record END TIME -> calculate run duration
3. **Capacity check:**
   - Run < 10 min -> double per-run caps next cron
   - Run < 5 min -> triple per-run caps next cron
   - Run > 25 min -> caps are about right
4. Print: `"Run complete. {X} actions in {Y} min. Daily: {emails}/75, {reddit}/30, {linkedin}/20, {discovery}/80"`

---

## PHASE 7 -- EVALUATE + DAILY REPORT (21:00 slot only)

1. Reply rate per niche, discovery quality, bounces, issues
2. Append findings to `docs/evaluations.md`
3. Send daily summary email to founder
4. If Sunday -> compile weekly digest

---

## Per-Run Caps

| Channel | Max/run | Spacing |
|---------|---------|---------|
| Reddit comments | 3-5 | 30s |
| LinkedIn comments | 5 | 15s |
| LinkedIn posts | 1 | AM or PM window |
| LinkedIn connects | 20 | 10s |
| Instagram DMs | 5 | 30s |
| Facebook messages | 5 | 30s |
| YouTube comments | 3 | 15s |
| Gmail sends | 20 | 3s |
| Quora answers | 2-3 | 30s |
| Discovery | 10 | -- |
| Proof tool tests | 5 | 15s |
| Social replies | 15 | 15s |

**Self-adjusting:** If run finishes early, caps auto-increase for next cron.

---

## 24-Hour Daily Targets

| Channel | Target | Ceiling |
|---------|--------|---------|
| Gmail sends | 75 | 100 |
| Reddit comments | 30 | 40 |
| LinkedIn comments | 20 | 25 |
| LinkedIn connects | 40 | 50 |
| Discovery | 80 | 100 |
| Proof tool tests | 20 | 30 |
| IG DMs | 25 | 30 |
| FB messages | 25 | 30 |

---

## Content Voice Rules

**Anti-AI (MANDATORY):** No round numbers, no filler openers ("Great question"), no "The [noun] is..." pattern, vary length wildly, read full post before commenting, reference specific details, include honest caveats.

**Brand:** NEVER mention your product in comments. 85% pure value / 10% soft DM / 5% transparent founder (tool-request posts only). LinkedIn POSTS can mention your product.

**Platform voice:** Reddit = pub chat. LinkedIn = founder voice. YouTube = brief casual. Email = professional + warm.

Full rules in `playbooks/channels.md` and `templates/voice-rules.md`.

---

## Approval Mode

ALL actions PRE-APPROVED. Only pause for: rate limits, CAPTCHA, expired login, unexpected errors, hot reply needing founder input.

---

## Logging

After EVERY action: `| [HH:MM] | [Channel] | [Action] | [Target] | [Preview] | [Status] | Duration: {X}min |`

Log to `docs/marketing-engine-log.md`.
