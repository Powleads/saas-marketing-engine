# Marketing Engine Config — [YOUR PRODUCT NAME]

## 1. Product
- **name:** [Your SaaS product name]
- **one_liner:** [One sentence: what it does + who it's for]
- **website:** [https://yourproduct.com]
- **pricing:** [e.g., "$49/mo starter, $149/mo pro"]
- **free_trial:** [yes/no, length]
- **key_differentiator:** [What makes you different from competitors]
- **founder_name:** [Your name]
- **founder_linkedin:** [https://linkedin.com/in/yourprofile]
- **founder_email:** [you@yourproduct.com]

## 2. ICP (Ideal Customer Profile)
- **target_businesses:** [e.g., "marketing agencies running Facebook ads for local businesses"]
- **business_size:** [e.g., "5-50 employees"]
- **geography:** [e.g., "UK", "US", "global"]
- **cities:** ["Manchester", "London", "Birmingham", "Leeds", "Glasgow", "Bristol", "Edinburgh", "Sheffield", "Newcastle", "Liverpool"]
- **job_titles:** ["owner", "founder", "CEO", "director", "marketing manager"]
- **pain_points:** [List 3-5 pain points your product solves]

## 3. Niches (rotate daily)

### niche_1
- **name:** [e.g., "Marketing Agencies"]
- **maps_query:** [e.g., "marketing agency {city}"]
- **search_terms:** [e.g., "facebook ads agency", "lead gen agency"]
- **reddit_subreddits:** [e.g., "r/agency, r/PPC, r/FacebookAds, r/digital_marketing"]
- **linkedin_targets:** [URLs of 5-10 ICP LinkedIn profiles to follow/engage]
- **email_angle:** [e.g., "Your clients blame you for bad leads — here's how to prove them wrong"]
- **tier_criteria:**
  - Tier 1 = [e.g., "50+ reviews, runs Facebook ads, has case studies"]
  - Tier 2 = [e.g., "20-50 reviews, some social presence"]
  - Tier 3 = [e.g., "<20 reviews, basic website"]

### niche_2
- **name:** [second niche]
- **maps_query:** [search query for Google Maps]
- **search_terms:** [keywords for LinkedIn/Reddit search]
- **reddit_subreddits:** [relevant subreddits]
- **linkedin_targets:** [ICP profiles]
- **email_angle:** [unique angle for this niche]
- **tier_criteria:**
  - Tier 1 = [criteria]
  - Tier 2 = [criteria]
  - Tier 3 = [criteria]

### niche_3
- **name:** [third niche — optional]
- [same structure as above]

## 4. Proof Tool
- **type:** website_scan
  <!-- Options: none, website_scan, free_tool, case_study -->
- **free_tool_url:** [if type=free_tool, URL to your free tool]
- **case_study_url:** [if type=case_study, URL to your case study page]
- **case_study_metrics:** [e.g., "3x response rate in 14 days"]

## 5. Email Sequence

### Day 1 — INTRO
**Subject:** {business_name} — [your hook]
**Body:**
Hi {owner_name},

[Your personalized cold email template. Use these placeholders:
- {business_name} — prospect's company name
- {owner_name} — CEO/owner first name
- {city} — their city
- {rating} — Google Maps rating
- {review_count} — number of Google reviews
- {website} — their website URL
- {product_name} — your product name
- {product_url} — your website
- {proof_findings} — findings from proof tool (Day 6 only)]

[2-3 short paragraphs. Lead with their problem, not your product.]

— [Your name], [Your product] | [your website]

---
If you'd prefer not to hear from me, just reply "stop" and I'll remove you immediately.

### Day 3 — FOLLOW-UP
**Subject:** Re: {business_name} — [value angle]
**Body:**
Hi {owner_name},

[Follow up on Day 1. Add a new angle — maybe a stat, a competitor insight, or a quick tip they can use even without your product.]

[Keep shorter than Day 1. One clear CTA.]

— [Your name]

### Day 6 — PROOF
**Subject:** {business_name} — I looked at your [website/process/setup]
**Body:**
Hi {owner_name},

[Reference proof tool findings. Be specific: "I noticed your contact form has 7 fields — that's costing you about 40% of leads." or "Your site loads in 4.2s on mobile — here's a quick fix."]

[Offer genuine value whether they buy or not. Attach or link to the audit/scan if applicable.]

— [Your name]

### Day 10 — SOCIAL PROOF
**Subject:** How [similar business] got [result] with [product]
**Body:**
Hi {owner_name},

[Share a case study or testimonial. If you don't have one yet, share your own results or a relevant industry stat.]

[Make it about them: "You're in a similar position to [case study company] — they were also [pain point]."]

— [Your name]

### Day 15 — OBJECTION HANDLER
**Subject:** {owner_name} — quick question
**Body:**
Hi {owner_name},

[Address the most common objection for this niche head-on. "Most [niche] owners tell me they already have [competitor/process]. The difference is..."]

[Be direct. No fluff.]

— [Your name]

### Day 21 — BREAK-UP
**Subject:** Closing the loop — {business_name}
**Body:**
Hi {owner_name},

[Final email. Acknowledge they're busy. Leave the door open. No guilt-tripping.]

[Something like: "I'll stop filling your inbox. If [pain point] ever becomes a priority, I'm at [email]. No hard feelings either way."]

— [Your name]

## 6. Objection Replies

### Price objection
**Template:**
Totally fair — {product_name} isn't the cheapest option out there, and it shouldn't be.

[Reframe price as ROI. Use specific numbers if possible: "If it brings in even 2 extra leads/month at your average deal size of $X, that's $Y vs the $Z/mo cost."]

[Offer the trial if applicable: "Happy to set you up with a {free_trial_length} trial so you can see the numbers yourself before committing."]

### "We already have a tool" objection
**Template:**
Makes sense — switching tools is a pain, especially when something's working okay.

[Don't trash the competitor. Acknowledge it, then differentiate: "Most folks who come to us from [competitor] say it does X well but they needed Y. That's specifically what we built for."]

[Offer a side-by-side: "Run us alongside [current tool] for {free_trial_length} — zero risk, and you'll see the difference in [metric] within a week."]

### "Not enough volume" objection
**Template:**
That's actually when [product_name] makes the most difference — when every lead counts.

[Flip the objection: smaller volume = higher cost per lost lead. "If you're getting 30 leads/month and losing 5 to slow follow-up, that's [percentage]% of your pipeline."]

### "Not interested" / generic brush-off
**Template:**
No worries at all, {owner_name}. Appreciate you taking the time to reply.

I'll take you off the list. If [pain point] ever becomes a priority, I'm at [your email].

All the best with {business_name}.

## 7. Google Sheet CRM
- **sheet_id:** [your Google Sheet ID — create from template or manually]
- **Pipeline columns:** Business Name | Niche | City | Tier | Email | Phone | Website | Rating | Reviews | FB Ads | IG | FB | Status | Discovery Date | Last Action | Next Action Date | Next Action | Sequence Day | Thread ID | Notes

### Status values:
- `new` — just discovered, not yet contacted
- `email_1` through `email_6` — in email sequence
- `hot_reply` — positive reply received
- `objection_[type]` — objection received, auto-reply sent
- `closed_won` — converted
- `closed_not_interested` — opted out
- `closed_no_response` — completed sequence, no reply
- `paused` — temporarily on hold (OOO, timing, etc.)

### Config tab columns:
- Current Niche Index | Current City Index | Total Runs | Last Run Date | Daily Send Count | Daily Comment Count

## 8. Banned Subreddits
<!-- Add any subs you get banned from here so the engine skips them -->

## 9. Restricted Subreddits (need higher karma)
<!-- Subs that auto-remove low-karma comments — the engine will skip these until your karma is high enough -->

## 10. UTM Parameters
- **source:** gmail
- **medium:** cold_email
- **campaign:** {niche}
- **content:** {owner_name}

Example URL: `https://yourproduct.com?utm_source=gmail&utm_medium=cold_email&utm_campaign=agencies&utm_content=john_smith`

## 11. Schedule Overrides
<!-- Optional: override default schedule for specific days -->
<!-- Example: -->
<!-- - Monday: skip Reddit (low engagement) -->
<!-- - Friday: double email sends -->
<!-- - Weekend: content prep only, no sends -->
