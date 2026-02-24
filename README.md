# 24H-Trend-to-Content-Generator 
> An AI skill that scans X (Twitter) for the hottest crypto and AI trends in the past 24 hours — filters for high-signal posts, clusters them into themes, and outputs ready-to-post brand content with real source links.

---

## What It Does

Most brand teams spend 1–2 hours per day manually scrolling X, evaluating which trends are worth reacting to, and writing content from scratch. By the time a post goes live, the trend has already peaked.

This skill compresses that entire workflow into **5–10 minutes**:

1. Reads your brand spec → understands niche, audience, tone
2. Selects the 3–5 most relevant subtopics to scan (not all of them)
3. Searches X for high-signal posts from the last 24 hours
4. Filters out ads, bots, low-engagement posts, and unverified accounts
5. Clusters posts into trend themes
6. Outputs **minimum 5 Trend Alerts** — each with signal data, real tweet links, and 2 draft posts ready to publish

---

## Input

The skill accepts three input modes — use any combination:

| Input | Required | Description |
|---|---|---|
| **Brand spec `.md` file** | Optional (primary) | Drop your brand guidebook. The skill auto-extracts niche, audience, tone, and KOL list |
| **Manual context** | Optional (fallback) | Describe your brand in plain text if no spec file is available |
| **KOL / Account list** | Optional (upgrade) | Provide 10–20 trusted @handles — skill will priority-scan those accounts before expanding to broad search |

If both spec file and manual input are provided, **spec file takes priority**.

---

## Output — Per Trend Alert

Each of the 5+ Trend Alerts includes:

```
TREND ALERT #N: [Trend Name]

Where it's hot       -> crypto Twitter / AI Twitter / general tech Twitter
Why it's relevant    -> 2-3 sentence context: what happened, why it matters
Signal strength      -> velocity (rising / peaked / fading) + estimated lifespan
Source posts         -> minimum 3 real X posts with URLs, handles, verified status, engagement
Brand relevance      -> HIGH / MEDIUM + 1-line reason
Content angle        -> specific hook, POV, and CTA for the brand
Draft Content        -> Option A (e.g. single tweet) + Option B (e.g. thread opener)
```

Followed by a **Trend Summary Table** with urgency ranking and a single TOP PICK recommendation.

---

## Filtering Logic

Posts are only included if they pass all of the following:

- Retweet or Quote count > 10
- Has replies/comments (zero-comment posts are excluded)
- Not sponsored or promoted
- Not from bot/spam accounts
- Posted within the last 24 hours
- Priority given to: gold-verified accounts, blue-verified accounts, and official project accounts

---

## Sourcing Modes

| Mode | When | How |
|---|---|---|
| **Priority Sourcing** | KOL list provided | Scans trusted accounts first, expands to broad search if fewer than 5 posts found per subtopic |
| **Broad Search** | No KOL list | Scans selected subtopics across all of X |

---

## Subtopic Coverage

The skill maintains a curated list of 50+ subtopics across 12 categories:

**Crypto:** Market & Price · Infrastructure & Tech · DeFi · Tokens & Trends · Institutional & Macro · Culture & Community

**AI:** Models & Labs · Agents & Automation · Tools & Products · Research & Breakthroughs · Business & Industry · Culture & Debate

Based on brand niche, only **3–5 subtopics** are selected per run — keeping searches focused and fast.

---

## Tools Required

| Tool | Purpose |
|---|---|
| `web_search` | Find trending posts and news across X |
| `web_fetch` | Retrieve full post content when snippets are insufficient |

No API keys required. Runs on any platform with web search access.

---

## Platform

Built and demo'd on **Claude Project** (claude.ai).

Compatible with: Claude Project · Custom GPT · Any LLM platform with web search access

---

## Demo Output Sample

**Brand:** Whales Market — trustless OTC DEX for pre-TGE tokens and airdrop allocations

**Trend Alert #1 (from live run):**

> $DIME (Paradex) TGE incoming — OTC pricing at $0.19-0.20/pt, implying $0.28-$0.57 at listing. Whales Market is the direct exit venue for farmers who want to lock gains before TGE chaos.
>
> **Draft Tweet (Option A):**
> "$DIME TGE: late Feb / early March. 20% supply to XP holders. Fully unlocked.
> OTC now: $0.19-0.20/pt = $0.28 at $500M FDV / $0.57 at $1B FDV.
> Sell now or gamble on listing? [whales.market]"

---

## File Structure

```
trend-to-content-generator/
├── README.md       <- this file
└── SKILL.md        <- full skill instructions for AI agent
```

---

## Built For

**Cook a Skill** — Internal Hackathon
Skill category: Social Media · Content · Crypto/AI Trend Intelligence
