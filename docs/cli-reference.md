# CLI Reference

Commands for Gmail, Google Sheets, Playwright, and Gemini used by the marketing engine.

## Gmail via `gws` CLI

### Create Draft and Send
```bash
# 1. Create draft via gmail_create_draft MCP -> returns draft ID
# 2. Send the draft:
gws gmail users drafts send --params '{"userId": "me"}' --json '{"id": "DRAFT_ID"}'
```

### Direct Send (skip draft)
```bash
gws gmail users messages send --params '{"userId": "me"}' --json '{"raw": "BASE64URL_ENCODED_MESSAGE"}'
```

### Reply Detection
```bash
# Check for replies from a specific prospect:
gws gmail users messages list --params '{"userId": "me", "q": "from:prospect@example.com is:unread"}'

# Read the reply:
gws gmail users messages get --params '{"userId": "me", "id": "MESSAGE_ID", "format": "full"}'

# Check for all recent unread replies:
gws gmail users messages list --params '{"userId": "me", "q": "is:unread newer_than:1d"}'
```

### Complex JSON (special characters)
When JSON contains special characters (!, quotes, etc.), write to a temp file first:
```bash
cat > /tmp/email-params.json << 'EOF'
{"userId":"me","q":"from:prospect@email.com is:unread newer_than:7d"}
EOF
gws gmail users messages list --params "$(cat /tmp/email-params.json)"
```

### Get Profile (verify auth)
```bash
gws gmail users getProfile --params '{"userId":"me"}'
```

---

## Google Sheets via `gws` CLI

Replace `YOUR_SHEET_ID` with your actual Google Sheet ID (the long string between `/d/` and `/edit` in the Sheet URL).

### Read Pipeline Tab
```bash
gws sheets spreadsheets values get --params '{"spreadsheetId":"YOUR_SHEET_ID","range":"Pipeline!A:T"}'
```

### Read Config Tab
```bash
gws sheets spreadsheets values get --params '{"spreadsheetId":"YOUR_SHEET_ID","range":"Config!A1:B12"}'
```

### Update a Cell
```bash
gws sheets spreadsheets values update \
  --params '{"spreadsheetId":"YOUR_SHEET_ID","range":"Pipeline!N5","valueInputOption":"USER_ENTERED"}' \
  --json '{"values":[["hot_reply"]]}'
```

### Append a New Row
```bash
gws sheets spreadsheets values append \
  --params '{"spreadsheetId":"YOUR_SHEET_ID","range":"Pipeline!A:T","valueInputOption":"USER_ENTERED"}' \
  --json '{"values":[["Business","niche","city","1","email","phone","website","4.5","120","yes","@ig","fb.com/page","new","2026-03-18","","","","0","","notes"]]}'
```

### Create a New Sheet
```bash
gws sheets spreadsheets create --json '{"properties":{"title":"Marketing Engine CRM"},"sheets":[{"properties":{"title":"Pipeline"}},{"properties":{"title":"Config"}}]}'
```

---

## Playwright CLI (stateless screenshots)

Use CLI for anything that does not need login or DOM interaction. Massively cheaper in tokens than MCP snapshots.

### Full-Page Screenshot (desktop)
```bash
npx playwright screenshot "{url}" /tmp/screenshot.png --full-page
```

### Mobile Viewport Screenshot
```bash
npx playwright screenshot "{url}" /tmp/mobile.png --full-page --viewport-size="375,812"
```

### Multiple Pages Fast
```bash
npx playwright screenshot "{url}" /tmp/homepage.png --full-page
npx playwright screenshot "{url}/contact" /tmp/contact.png --full-page 2>/dev/null
npx playwright screenshot "{url}/pricing" /tmp/pricing.png --full-page 2>/dev/null
npx playwright screenshot "{url}/about" /tmp/about.png --full-page 2>/dev/null
```

### Token Savings
- CLI screenshot + visual read = ~2K tokens
- MCP navigate + snapshot = 50-90K tokens
- That is a **25-45x reduction** per page

After taking a screenshot, read the PNG with the `Read` tool -- Claude can visually analyse screenshots to find CTAs, forms, emails, phone numbers, trust signals, and mobile issues.

---

## Image Generation (Gemini)

For creating social media images, carousel slides, or visual content:

```bash
curl -s "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-exp:generateContent?key=YOUR_GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"contents": [{"parts": [{"text": "Your image prompt here"}]}], "generationConfig": {"responseModalities": ["IMAGE", "TEXT"]}}'
```

---

## Gemini Vision (Website Audit)

For AI-powered website analysis using screenshots:

```bash
# 1. Take screenshot
npx playwright screenshot "{url}" /tmp/audit.png --full-page

# 2. Base64 encode it
BASE64=$(base64 -w0 /tmp/audit.png)  # Linux
# BASE64=$(base64 -i /tmp/audit.png)  # macOS

# 3. Send to Gemini Vision
curl -s "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-exp:generateContent?key=YOUR_GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"contents\": [{\"parts\": [
      {\"text\": \"Audit this website for lead conversion. Grade A-F. Evaluate: CTA clarity, form UX, trust signals, mobile readiness. List 3-5 quick wins.\"},
      {\"inline_data\": {\"mime_type\": \"image/png\", \"data\": \"$BASE64\"}}
    ]}]
  }"
```

---

## Useful Bash Patterns

### Base64 URL-encode an email for Gmail API
```bash
# Construct raw email
RAW_EMAIL="From: you@yourdomain.com
To: prospect@example.com
Subject: Your subject here
Content-Type: text/plain; charset=utf-8

Email body here."

# Base64 URL encode (replace +/ with -_ and strip padding)
ENCODED=$(echo -n "$RAW_EMAIL" | base64 -w0 | tr '+/' '-_' | tr -d '=')

# Send
gws gmail users messages send --params '{"userId": "me"}' --json "{\"raw\": \"$ENCODED\"}"
```

### Check if a website has a contact form (quick)
```bash
npx playwright screenshot "{url}/contact" /tmp/check.png --full-page 2>/dev/null
# Then read /tmp/check.png visually to see if there is a form
```
