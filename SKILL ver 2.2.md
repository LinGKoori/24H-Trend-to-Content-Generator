---
name: trend-to-content-generator
description: >
  24h Trend-to-Content Generator for crypto and AI brands on X (Twitter).
  Use this skill whenever the user wants to: find trending crypto or AI topics on X/Twitter,
  generate content ideas based on viral posts, scan social media for hot trends,
  create brand content from trending topics, monitor what's hot in crypto/AI in the last 24 hours,
  or get content suggestions based on real-time Twitter/X trends.
  Trigger this skill even if the user just says "find trend", "write content from trend",
  "what's hot", "scan X", or mentions monitoring social media for content opportunities.
---

# 24h Trend-to-Content Generator

A skill that scans X (Twitter) for the hottest crypto and AI trends in the past 24 hours, filters for high-signal posts, and outputs structured content alerts with ready-to-use draft posts for a brand.

---

## When to Use This Skill

- User wants to find trending crypto or AI topics on X/Twitter
- User wants content ideas based on what's going viral right now
- User mentions "trend", "viral", "hot topic", "what's trending", "content idea from trend"
- User wants to monitor social media for brand content opportunities

---

## Workflow Overview

```
INPUT: Brand spec .md file (primary) OR manual brand context
         |
STEP 1: Extract brand context -> select 3-5 focused subtopics
         |
STEP 2: Search X using focused subtopics (last 24h)
         |
STEP 3: Filter high-signal posts (RT/quote > 10, no ads, has comments)
         |
STEP 4: Cluster into trend themes
         |
STEP 5: Match trends to brand angle
         |
OUTPUT: Trend Alert + Content Draft per trend
```

---

## Step 1 — Extract Brand Context

### Primary Input: Brand Spec .md File

If the user provides a `.md` file (brand guidebook / brand spec), **read it first** and extract the following fields automatically:

| Field | What to look for in the .md file |
|---|---|
| **Brand niche** | Product description, "about", category, or positioning section |
| **Target audience** | Audience, customer persona, or ICP section |
| **Tone** | Tone of voice, brand voice, or communication guidelines section |
| **Content format** | Content examples, content pillars, or social media guidelines section |
| **Keywords / topics** | Any mention of relevant themes, competitors, or industry focus |
| **KOL / Account list** | Any list of trusted accounts, KOLs, or partners the brand follows — extract handles if present |

After extracting, **confirm with the user** in one message:
> "I've extracted the following brand context from your spec file: [summary]. Does this look correct before I start scanning?"

### Fallback: Manual Input

If no spec file is provided, ask the user for these fields directly:

| Field | Question |
|---|---|
| **Brand niche** | What space does the brand operate in? (e.g., crypto exchange, AI SaaS, Web3 wallet, DeFi protocol...) |
| **Target audience** | Who follows the brand? (e.g., retail investors, developers, crypto newcomers, VCs...) |
| **Tone** | What is the brand's voice? (e.g., educational, alpha-seeking, casual, professional, degen-friendly...) |
| **Content format** | What formats does the brand typically post? (Thread, single tweet, meme, infographic caption...) |

### Accepting Both

If the user provides both a spec file and additional manual context, **spec file takes priority**. Use manual input only to fill gaps or override specific fields the user explicitly calls out.

### Optional: KOL / Account List

The user may provide a list of 10–20 X handles that the brand follows or trusts (KOLs, partner projects, alpha accounts). This is **optional** — if provided, it upgrades the sourcing step:

- **If KOL list is provided:** Scan posts from those accounts first (priority sourcing), then expand to similar accounts and broad search to fill gaps.
- **If no KOL list is provided:** Use standard broad search across selected subtopics as normal.

The KOL list can come from:
- The brand spec .md file (extract handles automatically if listed)
- Manual input: user pastes a list of @handles in the prompt

When KOL list is provided, state before scanning:
> "KOL list detected — scanning posts from [N] priority accounts first, then expanding to broader search."

---

## Step 1b — Select Focused Subtopics

**Do not scan all subtopics.** Based on the extracted brand niche and audience, select **3–5 subtopics** most relevant to the brand before running any searches.

### Selection Logic

| Brand Type | Recommended Subtopic Focus |
|---|---|
| Crypto exchange / trading platform | Market & Price, Tokens & Trends, Institutional & Macro |
| DeFi protocol | DeFi, Infrastructure & Tech, Tokens & Trends |
| Web3 / NFT project | Culture & Community, Tokens & Trends, Infrastructure & Tech |
| AI SaaS / AI productivity tool | Tools & Products, Agents & Automation, Business & Industry |
| AI research / foundation model | Models & Labs, Research & Breakthroughs, Agents & Automation |
| Crypto + AI crossover brand | Market & Price, Agents & Automation, Tokens & Trends |
| General tech / VC / media brand | Business & Industry, Culture & Debate, Models & Labs |

After selecting, state clearly before scanning:
> "Based on your brand spec, I'll focus the scan on: [subtopic 1], [subtopic 2], [subtopic 3]. Starting now."

### Full Subtopic Reference List

> See `references/subtopics.md` for the complete list of 50+ subtopics across 12 categories.
> Read that file when selecting subtopics for a run — do not scan all of them every time.

---

## Step 1c — Fetch Top 3 Fundraising Rounds (First Run of Day Only)

### When to run this step

**Only execute Step 1c on the first run of the day (GMT+7 timezone).**

To determine if this is the first run:
- Ask the user: *"Is this your first scan today (GMT+7)?"*
- If **yes** → run Step 1c and include the Fundraising Radar section in output
- If **no** → skip Step 1c entirely, proceed directly to Step 2, and omit the Fundraising Radar section from output

If the user does not know or does not answer, default to **skipping** Step 1c to avoid duplicate data.

---

Before scanning X, pull the latest fundraising data from two sources in parallel:

```
web_fetch: https://cryptorank.io/funding-rounds
web_fetch: https://crypto-fundraising.info/
```

### VC Tier Filter

Only include rounds where at least one investor qualifies as **top-tier** per CryptoRank standards. Top-tier VCs include (non-exhaustive):

Paradigm, a16z (Andreessen Horowitz), Sequoia Capital, Multicoin Capital, Pantera Capital, Dragonfly Capital, Polychain Capital, Coinbase Ventures, Binance Labs, Jump Crypto, Galaxy Digital, Animoca Brands, Spartan Group, Delphi Digital, Framework Ventures, Electric Capital, Variant Fund, 1kx, GSR, CMT Digital.

Exclude rounds backed only by unknown/unranked funds with no CryptoRank profile or Moni Score below 500.

### Selection Criteria

From the filtered list, pick the **top 3 most notable rounds** based on:
- Raise amount (larger = higher priority)
- VC tier (more top-tier names = higher priority)
- Project relevance to crypto/DeFi/AI ecosystem
- Recency (within last 24h first, then last 48h if fewer than 3 found)

### Output Format — Fundraising Section

Add this block at the end of the run output, after all Trend Alerts:

---

## 💰 FUNDRAISING RADAR — Top 3 Rounds (Last 24h)
*Sources: CryptoRank · crypto-fundraising.info*

**#1 — [Project Name] | $[Amount] | [Stage]**
- Category: [e.g., DeFi / Infrastructure / AI / Gaming]
- Lead investors: [VC names — top-tier only, flag if none]
- Why it matters: [1-2 sentences: what the project does, why this raise is significant]
- Signal for brand: [Is this relevant to the brand's niche? Yes/No + reason]

**#2 — [Project Name] | $[Amount] | [Stage]**
[same format]

**#3 — [Project Name] | $[Amount] | [Stage]**
[same format]

---

If fewer than 3 rounds with top-tier VCs are found in the last 24h, extend to last 48h and note: *(Extended to 48h window — limited top-tier activity in last 24h)*

---

## Step 2 — Search X for Trending Posts (Last 24 Hours)

Use `web_search` with the **3–5 selected subtopics only**. The sourcing strategy depends on whether a KOL list was provided:

### Mode A: KOL List Provided (Priority Sourcing)

Scan posts from the brand's trusted accounts first:

```
site:x.com from:@[handle1] OR from:@[handle2] OR from:@[handle3] [selected_subtopic] since:[yesterday_date]
```

Run this across all provided handles and selected subtopics. If fewer than 5 high-signal posts are found per subtopic, expand to broad search (Mode B) to fill gaps.

### Mode B: No KOL List (Standard Broad Search)

**Crypto trends:**
```
site:twitter.com OR site:x.com crypto [selected_subtopic] since:[yesterday_date] min_retweets:10
```

**AI trends:**
```
site:twitter.com OR site:x.com AI [selected_subtopic] since:[yesterday_date] min_retweets:10
```

**Always include 1–2 broad trending searches regardless of mode:**
```
web_search: trending crypto twitter 24h
web_search: trending AI twitter today
```

Use `web_fetch` to retrieve full post content when snippets are too short.

---

## Step 3 — Filter High-Signal Posts

For each post found, apply this filter checklist:

| Criteria | Rule |
|---|---|
| Impressions | High — prioritize posts described as "viral", "going crazy", or frequently referenced by others |
| Retweet / Quote | > 10 RT or QT — only keep posts meeting this threshold |
| Comments | Must have replies/comments — skip posts with zero engagement discussion |
| Ads / Sponsored | Skip any post marked as "Promoted", "Ad", or from clearly sponsored accounts |
| Bot / Spam | Skip accounts with no followers or generic spam patterns |
| Recency | Must be within last 24 hours from current timestamp |
| Account priority | Prioritize posts from blue-verified accounts (public figures, creators), gold-verified accounts (official organizations, brands), and official project accounts — these carry higher credibility and signal strength |

**Scoring (for ranking within each cluster):**
- 3 pts: > 100 RT
- 2 pts: > 50 RT
- 1 pt: > 10 RT
- Bonus 2 pts: post from a gold-verified account or official project account
- Bonus 1 pt: post from a blue-verified account (known creator or KOL)
- Bonus 1 pt: multiple quote tweets with active discussion
- Bonus 1 pt: reply from a known influencer or KOL

Keep **top 5–10 posts** per theme cluster.

---

## Step 4 — Cluster Into Trend Themes

Group filtered posts into **trend clusters** (5–8 clusters max):

```
TREND CLUSTER:
- Theme name (1 line): What is this trend about?
- Signal: How many posts, from whom
- Why it's hot right now: News event, price move, new product launch, controversy?
- Lifespan estimate: Flash trend (< 6h) / Day trend (24h) / Week trend
```

**Example clusters:**
- "Bitcoin ETF outflow concern" — 12 posts, avg 80 RT
- "New Claude model drop" — 8 posts, avg 200 RT, multiple developer QTs
- "Solana memecoin rug" — 20 posts, high drama + community commentary

---

## Step 5 — Match Trend to Brand Angle

For each trend cluster, evaluate relevance to the brand:

| Score | Meaning |
|---|---|
| HIGH | Directly relevant to the brand's product, service, or audience |
| MEDIUM | Tangentially relevant — can be tied in with a smart angle |
| LOW | Not relevant — skip or flag as awareness only |

**Output content drafts for all HIGH and MEDIUM trends. Minimum 5 Trend Alerts per run — if fewer than 5 HIGH/MEDIUM trends are found, expand the search to additional subtopics. If after expanding the search fewer than 3 trends are found, output whatever was found and append this note at the top of the Trend Summary:

> ⚠️ Low trend activity detected in the last 24h. Only [N] relevant trend(s) found after full scan. Consider checking again in 6–12h.**

---

## Output Format — Trend Alert

**CRITICAL: All output must be in English only — no exceptions, regardless of the user's input language.**

Output one block per trend, using this structure:

---

### TREND ALERT #[N]: [Trend Name]

**Where it's hot:** [Platform area — crypto Twitter / AI Twitter / general tech Twitter]

**Why it's relevant:**
[2-3 sentences: What happened, why people are talking, why this matters to the brand's audience]

**Signal strength:**
- Trend velocity: [Rising fast / Peaked / Fading]
- Estimated lifespan: [< 6h / 24h / 3-7 days]

**Source posts (minimum 3 required):**
Use web_search and web_fetch to find real, linkable posts. For each post include:
- Post URL (x.com or twitter.com direct link)
- @handle — verified status (gold / blue / project / unverified)
- Engagement summary (RT count, QT count, or "high engagement" if exact count unavailable)
- 1-line summary of what the post says

If a direct tweet URL cannot be confirmed, link to the best available source (news article, thread aggregator) and note it as "via [source]". Never fabricate URLs.

**Brand relevance:** [HIGH / MEDIUM] — [1 sentence explaining why]

**Content angle for brand:**
[The specific lens or hook the brand should use — what POV, what emotion, what CTA]

**Draft Content:**

Option A — [Format: e.g., Single Tweet / Hook Tweet]
[Draft tweet text in English — ready to post or lightly edit]
[Suggested hashtags if applicable]

Option B — [Format: e.g., Thread opener / Meme caption]
[Alternative angle draft in English]

---

## Output Format — Full Run Summary

After all trend alerts, add a summary table:

```
TREND SUMMARY — [Date] [Time]
Subtopics scanned: [list of 3-5 subtopics used]

| # | Trend          | Relevance | Recommended Action    | Urgency |
|---|----------------|-----------|-----------------------|---------|
| 1 | [Name]         | HIGH      | Post now              | < 2h    |
| 2 | [Name]         | MEDIUM    | Schedule for tonight  | < 6h    |
| 3 | [Name]         | HIGH      | Post now              | < 2h    |
| 4 | [Name]         | MEDIUM    | Schedule for tonight  | < 6h    |
| 5 | [Name]         | LOW       | Monitor only          | -       |

TOP PICK: Trend #[N] — [One line reason why this is the best opportunity right now]
```

---

## Constraints & Rules

### Engagement Estimation

If exact RT/QT counts are not available via web_search, estimate based on contextual signals:

| Signal | Threshold | Classification |
|---|---|---|
| Quoted by other accounts | 10+ accounts quoting | High engagement |
| Retweets visible/mentioned | 20+ retweets | High engagement |
| Bookmarks mentioned | 50+ bookmarks | High engagement |
| Referenced in multiple articles/threads | 2+ sources | High engagement |

Always add a disclaimer when data is estimated:
> "(Engagement estimated based on contextual signals — exact metrics unavailable)"

Never skip a post solely because exact counts are unavailable. Use context to make the call.

---

### Content & Draft Rules

- **Tweet length:** Each draft tweet must not exceed 2,000 characters. Keep single tweets under 280 characters when possible — flag if a draft exceeds 280 chars and suggest breaking it into a thread.
- **Tone matching:** Always match the brand's tone extracted from the spec file or manual input. Never default to a generic neutral tone. If tone is "degen/alpha" — write degen. If "educational" — write educational.
- **Thread format:** Thread openers should end with a clear hook that makes the reader want to see the next tweet (cliffhanger, surprising stat, or bold claim).

---

### Data Integrity Rules

- **No hallucinated URLs:** Never fabricate tweet links or article URLs. If a direct tweet URL cannot be confirmed, link to the best available secondary source and label it clearly as "via [source name]".
- **No fabricated handles:** Never invent @handles. If the original author cannot be confirmed, attribute to the source publication instead.
- **No fabricated metrics:** Never invent RT counts, impression numbers, or follower counts. Always use real data or clearly labeled estimates.

---

### Content Safety Rules

- **No unverified token promotion:** Do not recommend or highlight tokens that show rug pull signals (no liquidity lock, anonymous team, sudden volume spike with no organic community). Flag these as "high risk" if they appear in trend data.
- **No scam amplification:** If a trending post is promoting a clear scam, phishing link, or pump-and-dump scheme — exclude it from sourcing entirely, even if engagement is high.
- **No political content:** Exclude any trend that is primarily political rather than crypto/AI-related, even if it intersects with the space.
- **Disclaimer on estimated data:** Any alert where engagement data is estimated (not confirmed) must include the estimation disclaimer in the Signal Strength section.

---

### Operational Rules

- **Always timestamp the scan** — state clearly: "Scanned at [time], trends reflect last 24h from this point"
- **Always state which subtopics were scanned** — include in the summary output
- **Prioritize original posts over aggregators** — find the actual tweet, not just a newsletter summarizing it
- **Ads detection:** Any post from accounts with a "Promoted" label, or accounts that only post promotional content with no organic replies — skip
- **Fallback:** If web search returns limited X data, use `web_fetch` on Twitter aggregator sites or nitter mirrors as backup

---

## Tools Required

- `web_search` — find trending posts and news
- `web_fetch` — retrieve full post/article content when snippets are insufficient

No special API keys required. Works with standard web search.
