# Proof Tool -- Mystery Tests (CLI Recon + MCP Form Fill)

A "mystery test" is when you secretly test a prospect's website as if you were a customer -- submit their contact form and measure how long it takes them to respond. This creates powerful, personalized proof data for your outreach emails.

## When to Run

- **ONLY during business hours (9AM-4PM)** -- businesses must be open for accurate response time data
- Run during Phase 4 (Discovery) or dedicated proof tool slots in the schedule
- Max 5 per run, 20 per day

## Process: CLI Recon -> MCP Form Fill

### STEP 1 -- RECON via CLI (saves tokens)

Take screenshots with Playwright CLI (fast, stateless, no login needed):

```bash
npx playwright screenshot "{website_url}" /tmp/mt-homepage.png --full-page
npx playwright screenshot "{website_url}/contact" /tmp/mt-contact.png --full-page 2>/dev/null
npx playwright screenshot "{website_url}/contact-us" /tmp/mt-contact2.png --full-page 2>/dev/null
npx playwright screenshot "{website_url}" /tmp/mt-mobile.png --full-page --viewport-size="375,812"
```

Read the PNG screenshots visually -- analyse for:
- Contact form presence (input fields, submit buttons)
- CTAs (buttons with action words)
- Email addresses visible on page
- Phone numbers
- Live chat widget icons
- Mobile layout issues

Determine: `has_form` (yes/no), `form_url` (which page), `email_found`, `phone_found`

### STEP 2 -- FORM FILL via MCP (only if form found)

If form found, switch to Playwright MCP:

1. `browser_navigate` -> `{form_url}`
2. Dismiss cookie banners via `browser_evaluate`:
   ```js
   document.querySelectorAll('[class*="cookie"] button, [class*="consent"] button, [id*="cookie"] button').forEach(b => b.click())
   ```
3. Fill with test persona data:
   - **Name:** Use a realistic test persona name (alternate between 2-3 personas)
   - **Email:** test.persona@gmail.com (use a real inbox you control for tracking responses)
   - **Phone:** A test phone number you monitor
   - **Message:** Personalised enquiry about their services (make it realistic and specific to their business)
4. Submit the form
5. Take confirmation screenshot via `browser_take_screenshot`
6. Note submission time for response tracking

### STEP 3 -- If No Form Found

- Log as "no_form" with email/phone if found from screenshots
- You can still use the website audit data (CTA quality, mobile experience, trust signals) in outreach emails even without a form submission

### STEP 4 -- Log and Track

Update Pipeline Sheet:
- Column U: `local_test_{date}` (or your test ID)
- Column V: `submitted` or `no_form`

Log: `[HH:MM] Mystery Test | {business_name} | Form: yes/no | Submitted: yes/no | success`

## For B2B / Agency Prospects

If you are targeting agencies, test the **CLIENT's** site (the agency's customer), not the agency's own site. This makes the proof more compelling: "I tested one of your clients and here is what happened."

Store the client website in Pipeline Sheet column W.

## Checking Results (Phase 2)

Read Pipeline Sheet columns U and V -- filter rows where V = "submitted":

Check your test inbox (test.persona@gmail.com) for responses. Update column V:
- `responded_{minutes}m` -- e.g., "responded_47m"
- `timeout_24h` -- no response within 24 hours
- Leave as `submitted` if still waiting

### Response Time Grading

| Response Time | Grade |
|---------------|-------|
| < 5 minutes | A |
| 5-15 minutes | B |
| 15-30 minutes | C |
| 30-60 minutes | D |
| > 60 min / no response | F |

## AI Website Audit (Optional Enhancement)

If you have access to Gemini Vision or similar, you can generate an AI audit of the prospect's website:

```bash
# Take screenshots, then send to Gemini Vision for analysis
curl -s "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-exp:generateContent?key=YOUR_GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{"parts": [
      {"text": "Audit this website screenshot for lead conversion readiness. Grade A-F. Evaluate: CTA clarity, form UX, trust signals, mobile experience, page speed indicators. List 3-5 quick wins."},
      {"inline_data": {"mime_type": "image/png", "data": "BASE64_SCREENSHOT_DATA"}}
    ]}]
  }'
```

This gives you an `aiGrade` and `quickWins` to include in Day 6 emails.

## Day 6 Email Integration

Day 6 "proof" emails include your mystery test findings:

**With response data:**
- "Your team took {response_time} to respond to a web enquiry -- that is a {response_grade}."
- Include 2-3 quick wins from the AI audit if available
- Link to a results page if you have one: `yoursite.com/results/{test_id}`

**Without response (timeout):**
- "I submitted an enquiry through your website 24 hours ago and have not heard back yet."
- This is your strongest email -- the proof is undeniable

**No form found:**
- "I tried to submit an enquiry on your website but could not find a contact form."
- Pivot to the website audit findings instead

## Calculator Math (for ROI framing)

```
monthly_leads = estimated from ad count (1-3 ads: 20, 3-8 ads: 50, 8+: 100)
lead_cost = niche average cost per lead
current_conversion = {F: 0.05, D: 0.10, C: 0.15, B: 0.25, A: 0.35}[response_grade]
optimal_conversion = 0.35
monthly_waste = monthly_leads * lead_cost * (optimal_conversion - current_conversion)
annual_waste = monthly_waste * 12
```

Use these numbers in your email: "At your current response time, you are likely losing ~${monthly_waste}/month in leads that go to competitors who respond faster."

## Building Your Own Proof Tool

If you do not have a mystery test system, here are simpler alternatives:

### Option A: Screenshot + AI Audit Only
Skip the form fill. Just screenshot their site + run through Gemini Vision. Grade their CTA, mobile experience, and trust signals. This still creates personalized proof data for emails.

### Option B: Free Calculator / Scanner Page
Build a simple page on your site (e.g., `/tools/audit`) that:
1. Accepts a URL
2. Takes automated screenshots
3. Runs AI analysis
4. Displays a grade + findings
5. Gates the full report behind an email capture

### Option C: Manual Spot-Check
Visit the prospect's site yourself, note 3-5 specific issues, reference them in your Day 6 email. Does not scale, but converts well for Tier 1 prospects.
