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
INPUT: Brand niche + target audience + tone
   ↓
STEP 1: Search X for hot crypto/AI posts (last 24h)
   ↓
STEP 2: Filter high-signal posts (impressions, RT/quote > 10, no ads, has comments)
   ↓
STEP 3: Cluster into trend themes
   ↓
STEP 4: Match trends to brand angle
   ↓
OUTPUT: Trend Alert + Content Draft per trend
```

---

## Step 1 — Collect Brand Context (If Not Provided)

Before scanning, confirm with the user:

| Field | Question |
|---|---|
| **Brand niche** | What space does the brand operate in? (e.g., crypto exchange, AI SaaS, Web3 wallet, DeFi protocol...) |
| **Target audience** | Who follows the brand? (e.g., retail investors, developers, crypto newcomers, VCs...) |
| **Tone** | What is the brand's voice? (e.g., educational, alpha-seeking, casual, professional, degen-friendly...) |
| **Content format** | What formats does the brand typically post? (Thread, single tweet, meme, infographic caption...) |

If the user already provided these in the prompt, skip asking — extract directly.

---

## Step 2 — Search X for Trending Posts (Last 24 Hours)

Use `web_search` to find high-engagement posts. Run **multiple parallel searches** across different angles:

### Search Query Templates

**Crypto trends:**
```
site:twitter.com OR site:x.com crypto [subtopic] since:[yesterday_date] min_retweets:10
```

**AI trends:**
```
site:twitter.com OR site:x.com AI [subtopic] since:[yesterday_date] min_retweets:10
```

**Suggested subtopics to scan:**

- Crypto (scan all that are relevant to brand):
  - Market & Price: `Bitcoin`, `Ethereum`, `BTC price`, `ETH price`, `altcoin season`, `crypto crash`, `crypto rally`, `market cap`, `crypto dominance`
  - Infrastructure & Tech: `Layer 2`, `L2`, `rollup`, `zkEVM`, `Solana`, `Avalanche`, `Cosmos`, `modular blockchain`, `cross-chain bridge`, `interoperability`
  - DeFi: `DeFi`, `DEX`, `yield farming`, `liquidity pool`, `stablecoin`, `lending protocol`, `TVL`, `restaking`, `liquid staking`, `EigenLayer`
  - Tokens & Trends: `memecoin`, `airdrop`, `token launch`, `IDO`, `presale`, `new listing`, `pump and dump`, `rug pull`, `whale alert`, `on-chain data`
  - Institutional & Macro: `Bitcoin ETF`, `crypto ETF`, `institutional crypto`, `BlackRock crypto`, `crypto regulation`, `SEC crypto`, `CBDC`, `crypto tax`, `macro crypto`, `Fed crypto`
  - Culture & Community: `NFT`, `Web3`, `DAO`, `crypto meme`, `crypto Twitter drama`, `KOL call`, `degen`, `crypto influencer`, `bear market`, `bull market`

- AI (scan all that are relevant to brand):
  - Models & Labs: `GPT-5`, `Claude`, `Gemini`, `Llama`, `Mistral`, `Grok`, `open source LLM`, `model release`, `AI benchmark`, `foundation model`
  - Agents & Automation: `AI agent`, `autonomous agent`, `multi-agent`, `AI workflow`, `AI automation`, `agentic AI`, `computer use`, `AI coding`, `Cursor`, `Devin`
  - Tools & Products: `AI tool`, `AI app`, `AI SaaS`, `ChatGPT`, `Perplexity`, `Midjourney`, `Sora`, `AI video`, `AI image`, `AI voice`
  - Research & Breakthroughs: `AI research`, `AI paper`, `reasoning model`, `AI safety`, `alignment`, `AI consciousness`, `AI benchmark`, `emergent behavior`, `AGI`, `superintelligence`
  - Business & Industry: `AI startup`, `AI funding`, `AI valuation`, `enterprise AI`, `AI chip`, `Nvidia`, `AI infrastructure`, `AI investment`, `AI acquisition`, `AI IPO`
  - Culture & Debate: `AI job loss`, `AI regulation`, `AI copyright`, `AI ethics`, `prompt engineering`, `AI hype`, `AI doomer`, `AI accelerationism`, `e/acc`, `AI meme`

Also search trending hashtags:
```
web_search: trending crypto twitter 24h
web_search: trending AI twitter today
web_search: crypto twitter viral post today
```

Use `web_fetch` to retrieve full post content when snippets are too short.

---

## Step 3 — Filter High-Signal Posts

For each post found, apply this filter checklist:

| Criteria | Rule |
|---|---|
| ✅ Impressions | High — prioritize posts described as "viral", "going crazy", or frequently referenced by others |
| ✅ Retweet / Quote | > 10 RT or QT — only keep posts meeting this threshold |
| ✅ Comments | Must have replies/comments — skip posts with zero engagement discussion |
| ❌ Ads / Sponsored | Skip any post marked as "Promoted", "Ad", or from clearly sponsored accounts |
| ❌ Bot / Spam | Skip accounts with no followers or generic spam patterns |
| ✅ Recency | Must be within last 24 hours from current timestamp |
| ✅ Account priority | Prioritize posts from blue-verified accounts (public figures, creators), gold-verified accounts (official organizations, brands), and official project accounts — these carry higher credibility and signal strength |

**Scoring (optional, for ranking):**
- 3 pts: > 100 RT
- 2 pts: > 50 RT
- 1 pt: > 10 RT
- Bonus 2 pts: post is from a gold-verified account or official project account
- Bonus 1 pt: post is from a blue-verified account (known creator or KOL)
- Bonus 1 pt: multiple quote tweets with active discussion
- Bonus 1 pt: reply from a known influencer or KOL

Keep **top 5–10 posts** per theme cluster.

---

## Step 4 — Cluster Into Trend Themes

Group filtered posts into **trend clusters** (2–5 clusters max):

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
| 🔥 HIGH | Directly relevant to the brand's product, service, or audience |
| ⚡ MEDIUM | Tangentially relevant — can be tied in with a smart angle |
| ❄️ LOW | Not relevant — skip or flag as awareness only |

**Only output content drafts for 🔥 HIGH and ⚡ MEDIUM trends.**

---

## Output Format — Trend Alert

Output one block per trend, using this structure:

---

### 🚨 TREND ALERT #[N]: [Trend Name]

**📍 Where it's hot:** [Platform area — crypto Twitter / AI Twitter / general tech Twitter]

**🔥 Why it's relevant:**
[2–3 sentences: What happened, why people are talking, why this matters to the brand's audience]

**📊 Signal strength:**
- Top post: [@handle] — [RT count] RTs / [Quote count] QTs
- Trend velocity: [Rising fast / Peaked / Fading]
- Estimated lifespan: [< 6h / 24h / 3–7 days]

**🎯 Brand relevance:** [HIGH / MEDIUM] — [1 sentence explaining why]

**💡 Content angle for brand:**
[The specific lens or hook the brand should use — what POV, what emotion, what CTA]

**✍️ Draft Content:**

> **Option A — [Format: e.g., Single Tweet / Hook Tweet]**
> [Draft tweet text — ready to post or lightly edit]
> [Suggested hashtags if applicable]

> **Option B — [Format: e.g., Thread opener / Meme caption]**
> [Alternative angle draft]

---

## Output Format — Full Run Summary

After all trend alerts, add a summary table:

```
## 📋 TREND SUMMARY — [Date] [Time]

| # | Trend | Relevance | Recommended Action | Urgency |
|---|---|---|---|---|
| 1 | [Name] | 🔥 HIGH | Post now | < 2h |
| 2 | [Name] | ⚡ MEDIUM | Schedule for later today | < 6h |
| 3 | [Name] | ❄️ LOW | Monitor only | — |

💡 TOP PICK: Trend #[N] — [One line reason why this is the best opportunity right now]
```

---

## Example Run

**User input:**
> Brand: crypto exchange targeting retail investors. Tone: friendly, educational, slightly degen. Format: single tweet + thread opener.

**Claude action:**
1. Search X for trending crypto posts in the last 24h
2. Filter by RT > 10, has comments, not sponsored
3. Cluster into 3 themes: BTC price movement / new altcoin narrative / regulatory news
4. Match to brand (exchange — price + altcoin highly relevant, regulation medium)
5. Output 2–3 trend alerts with ready-to-post draft content

---

## Important Notes

- **Always timestamp the scan** — state clearly: "Scanned at [time], trends reflect last 24h from this point"
- **Never fabricate post data** — if RT counts are not visible in search results, say "estimated high engagement" and note the uncertainty
- **Prioritize original posts over aggregators** — find the actual tweet, not just a newsletter summarizing it
- **Ads detection**: Any post from accounts with a "Promoted" label, or accounts that only post promotional content with no organic replies — skip
- **Fallback**: If web search returns limited X data, use `web_fetch` on Twitter aggregator sites or nitter mirrors as backup

---

## Tools Required

- `web_search` — find trending posts and news
- `web_fetch` — retrieve full post/article content when snippets are insufficient

No special API keys required. Works with standard web search.
