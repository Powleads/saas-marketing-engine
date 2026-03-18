# Channel Playbooks

Platform-specific posting flows for the marketing engine. Each channel has quirks — follow these exactly to avoid bans and maximize engagement.

---

## Reddit

### Why old.reddit.com
Use `old.reddit.com` for all Reddit operations. The old interface is more reliable for Playwright automation — fewer dynamic elements, simpler DOM, consistent class names.

### Finding Threads
1. Navigate to `old.reddit.com/r/{subreddit}/new/` for each subreddit in config
2. Scan titles for relevance to your product's problem space
3. Score threads:
   - **Hot targets:** 2-6 hours old, 5-50 upvotes, <20 comments (rising, not saturated)
   - **Good targets:** <24 hours old, any upvotes, <50 comments
   - **Skip:** 48+ hours old, 100+ comments (buried), locked threads
4. Open thread, read the FULL post and ALL top-level comments
5. Only comment if you genuinely have something to add

### Posting Flow (Lexical Editor)
1. Navigate to thread URL on old.reddit.com
2. Scroll to comment box at bottom
3. Click the comment text area to focus it
4. **TYPE the comment character by character** — do NOT paste. Pasting triggers spam filters on some subs.
5. Review what you typed in the preview
6. Click the "save" button (old Reddit) or "Comment" button
7. Wait for page confirmation (comment appears in thread)
8. Wait 60 seconds before navigating to next thread

### Subreddit Rules
- Read sidebar rules before first post in any sub
- Some subs require flair — check before posting
- Never post the same comment in multiple threads
- Never post in a thread where you've already commented
- If a sub has "no self-promotion" rules, stick to 100% value comments

### Account Health
- New accounts: start with 5-10 pure value comments (no product angle at all) over 3 days
- Build to 100+ comment karma before any soft mentions
- Post ratio: 10 value comments per 1 soft mention
- If comment gets removed, skip that subreddit for 48 hours
- If banned, add to Banned Subreddits in config, never return

---

## LinkedIn

### Commenting Strategy
Two approaches, alternate between them:

**ICP Hit List Commenting:**
1. Keep a list of 20-30 ICP profiles in your config (linkedin_targets)
2. Visit their profiles, check recent posts
3. Leave thoughtful comments that add genuine insight
4. This gets you on their radar + visible to their network

**Content Search Commenting:**
1. Search LinkedIn for your niche keywords
2. Filter by "Posts" and "Past week"
3. Find posts with 10-100 reactions (engaged but not saturated)
4. Comment with substance — not "Great post!"

### Comment Formula
- Open with a specific reference to what they said
- Add your perspective or experience
- Optionally disagree with one small thing (drives engagement)
- Keep under 100 words
- No hashtags in comments

### Posting (Founder Voice)
Optimal times: 7-9 AM or 12-1 PM (your ICP's timezone)

Structure:
```
Hook line (pattern interrupt — make them stop scrolling)

Short paragraph with context.

Short paragraph with insight.

Short paragraph with the takeaway.

Question that invites discussion?
```

Rules:
- 2-3 hashtags maximum, at the very end
- No emoji walls
- No "I'm humbled to announce"
- Write like you're texting a smart friend, not writing a press release
- Vary between: lessons learned, hot takes, behind-the-scenes, data/results

### Connection Requests
- Only send to people who match your ICP
- Always add a note (under 300 chars): reference their content or company
- 10-15 connection requests per day maximum
- Never pitch in the connection request

### DMs
- Only DM people who accepted your connection OR engaged with your content
- Open with something specific about their profile/posts
- Keep first message under 3 sentences
- Never send a pitch as first DM

---

## YouTube

### Finding Videos
1. Search YouTube for your niche keywords
2. Filter by "This week" for fresh content
3. Target videos with 1k-50k views (enough audience, not too saturated)
4. MUTE the video — you don't need to watch it
5. Read the title, description, and top 5 comments for context

### Comment Flow
1. Scroll past the video to the comment section
2. Click "Add a comment"
3. Type a brief, casual comment (2-4 sentences)
4. Reference something specific from the video title/description
5. Click "Comment" button
6. Wait 30 seconds before next video

### Comment Style
- Casual, conversational tone
- Short — YouTube comments are quick reads
- Reference the video content specifically
- Add a personal anecdote or perspective
- No links, no product mentions
- Your channel/profile description does the selling

### Account Setup
- YouTube profile should have: your name, what you do, link to your product
- Profile picture: your face (not a logo)
- Channel description: 1-2 sentences about what you're building

---

## Quora

### Finding Questions
1. Search Quora for your ICP's pain points (not your product category)
2. Target questions with 1k+ views but few answers (<10)
3. Check question age — recent questions get more distribution
4. Look for questions where existing answers are generic or outdated

### Answer Format
- 300-500 words
- Open with "I run a [type of business]..." or personal experience
- Structure: context -> problem -> what I learned -> actionable advice
- Include specific numbers or examples
- No product links in the body
- Your profile bio mentions your product — that's enough

### Profile Setup
- Bio: "Founder of [product] | [one-liner about what you do]"
- Credentials: add relevant topics
- Profile link: your product website

---

## Instagram

### DM Strategy
Only DM prospects who have IG handles (from enrichment phase).

1. Visit their profile
2. Like 1-2 recent posts (genuine engagement signal)
3. View their latest story if available
4. Send a personalized DM referencing something specific:
   - A recent post they made
   - A project they showcased
   - An achievement they shared

### DM Template
Keep it casual and short:
```
Hey {name} — saw your post about {topic}. Really solid work.

Quick question: {relevant question about their business that relates to your product's problem space}?

No pitch, genuinely curious.
```

### Rules
- 3 DMs max per run, 60 second spacing
- Never send a product link as first message
- Never use Instagram's "professional dashboard" auto-messages
- If they reply, have a genuine conversation before any mention of your product

---

## Facebook

### Group Strategy
1. Join 3-5 groups relevant to your ICP (from config)
2. Lurk for 3-5 days before posting (learn the group culture)
3. Answer questions and add value in comments
4. No self-promotion in groups unless explicitly allowed

### DM Strategy
Similar to Instagram — only warm outreach:
1. Find prospect's Facebook page/profile from enrichment
2. Like their business page
3. Send a personalized message about their business
4. 3 messages max per run, 60 second spacing

### Page Strategy (optional)
If you have a Facebook business page:
- Post 2-3x per week
- Mix of: founder stories, tips, behind-the-scenes, industry commentary
- Engage with every comment

---

## Gmail (via gws CLI)

### Setup
```bash
npm install -g @googleworkspace/cli
gws auth login --scopes gmail,sheets,drive
gws gmail users getProfile --params '{"userId":"me"}'
```

### Sending Sequence
1. Create draft with personalized content
2. Send draft immediately (don't let drafts pile up)
3. Log to Google Sheet: thread ID, send date, sequence day
4. 5 second spacing between sends

### Reply Handling
```bash
# Check for replies from a specific prospect:
gws gmail users messages list --params '{"userId":"me","q":"from:prospect@email.com is:unread"}'

# Read the reply:
gws gmail users messages get --params '{"userId":"me","id":"MESSAGE_ID","format":"full"}'
```

### Deliverability Tips
- Warm up new Gmail accounts: 5 sends/day for week 1, 10 for week 2, ramp to 50
- Keep emails under 200 words
- No HTML — plain text only
- Include an unsubscribe line in every email
- If bounce rate exceeds 5%, pause and clean your list
- Mix up subject lines — don't send the same subject to 50 people

### Using Temp Files for Complex JSON
When JSON contains special characters (!, quotes, etc.), write to a temp file first:
```bash
cat > /tmp/email-params.json << 'EOF'
{"userId":"me","q":"from:prospect@email.com is:unread newer_than:7d"}
EOF
gws gmail users messages list --params "$(cat /tmp/email-params.json)"
```

---

## Cross-Platform Rules

1. **Never link between platforms** — don't say "I posted about this on Reddit" in a LinkedIn comment
2. **Different voice per platform:**
   - Reddit: casual, community-first, slightly technical
   - LinkedIn: professional but human, founder voice
   - YouTube: brief and conversational
   - Quora: authoritative and detailed
   - Instagram/Facebook: friendly and personal
3. **One persona across all platforms** — same name, same face, same bio linking to your product
4. **Log everything** — every comment, DM, email, and reply goes in the marketing engine log
