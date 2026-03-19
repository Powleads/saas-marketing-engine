# Night Shift Agents

Launch these specialist agents during Wave 3 (02:07) to plan tomorrow's content in parallel. Each agent runs independently and saves output to `tmp/night-prep/`.

## Agent 1 -- Trend Researcher

```
subagent_type: "Trend Researcher"
prompt: "Research trending topics in {your_product}'s problem space for {tomorrow_date}. Check Reddit trending posts in the subreddits from the config file. Identify which posts got the most engagement today and what topics resonated. Also check LinkedIn trending articles and relevant newsletters. Output: 10 content angles ranked by timeliness and engagement potential. Save to tmp/night-prep/trends-{date}.md"
```

## Agent 2 -- LinkedIn Content Creator

```
subagent_type: "LinkedIn Content Creator"
prompt: "Write 2 LinkedIn posts for {your_name}, founder of {your_product}. Post 1 (7AM): build-in-public or data-driven insight about your industry / running a SaaS / growth tactics. Post 2 (12PM): hot take or vulnerable founder moment about entrepreneurship or your space. Voice: direct, first-person, no hashtag spam, end with question not CTA. Can mention {your_product} in LinkedIn posts (not comments). Content pillars rotate: build-in-public, vulnerable, data, hot take, origin story. Save to tmp/night-prep/linkedin-posts-{date}.md"
```

## Agent 3 -- Reddit Community Builder

```
subagent_type: "Reddit Community Builder"
prompt: "Analyse the subreddits from the config file for tomorrow's best threads to comment on. For each sub: find 5-8 threads posted in last 12 hours with 2-20 comments and rising engagement. Score by: post freshness (newer = better), comment count (fewer = better for first-responder), relevance to our topics. Pre-write 40 comments across all threads -- each must read the full post + existing comments, add a unique angle, follow anti-AI voice rules (no filler openers, reference specific post content, casual tone). Save threads + comments to tmp/night-prep/reddit-threads-{date}.md"
```

## Agent 4 -- Social Media Strategist

```
subagent_type: "Social Media Strategist"
prompt: "Plan tomorrow's complete social content calendar for {your_product}. Include: LinkedIn (2 posts -- AM and PM, copy ready), Instagram (1 story or carousel -- concept + copy), Reddit (which subs to prioritise based on today's engagement data), Facebook (1 group post if relevant). Consider: what performed well today, content pillar rotation, upcoming email sequences that need social support. Format as a time-blocked calendar with copy for each item. Save to tmp/night-prep/social-calendar-{date}.md"
```

## Optional Agents (run if time allows)

### Agent 5 -- Instagram Curator

```
subagent_type: "Instagram Curator"
prompt: "Create 3 Instagram content ideas for {your_product}'s IG account. Focus on: carousel posts showing data/insights, story polls about your ICP's pain points, short-form educational content. Include copy, visual direction, and hashtag suggestions (max 5 per post). Save to tmp/night-prep/instagram-content-{date}.md"
```

### Agent 6 -- Carousel Growth Engine

```
subagent_type: "Carousel Growth Engine"
prompt: "Generate a 6-slide LinkedIn carousel for tomorrow's AM post. Topic from content pillar rotation. Slides: Hook -> Problem -> Data -> Solution -> Proof -> CTA. Write the text for each slide. Save slide text to tmp/night-prep/carousel-{date}.md"
```

## How to Use Agent Outputs

1. Agents run in parallel during Wave 3 (02:07-03:07)
2. Wave 4 (03:37) reviews outputs, edits/improves, fills gaps
3. Wave 5 (05:07) does final review + saves everything to `tmp/night-prep/`
4. Morning blitz (07:03) executes the prepped content

## Customizing Agents

These are starting templates. Adapt them to your product and market:

- **Replace placeholders:** `{your_product}`, `{your_name}`, `{tomorrow_date}`, `{date}`
- **Add niche-specific topics:** If you sell to dentists, add dental industry topics to the Trend Researcher
- **Adjust subreddit lists:** Match your config file's subreddit definitions
- **Change content pillars:** Align with your brand voice and what resonates with your audience

## Running Agents Manually

You can also run individual agents outside the night shift:

```
# In Claude Code:
"Run the Trend Researcher agent from docs/night-agents.md for tomorrow"
"Run the Reddit Community Builder agent for tonight's prep"
```

This is useful when you want to prep content for a specific campaign or event.
