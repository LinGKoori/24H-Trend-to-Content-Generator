# Skill Card — 24h Trend-to-Content Generator

| Field | Details |
|---|---|
| **Skill Name** | 24h Trend-to-Content Generator |
| **What It Automates** | Automatically scans X (Twitter) for the last 24 hours, filters high-signal posts (RT/QT > 10, has comments, no ads), clusters them by trend theme, and outputs ready-to-post draft content for the brand — replacing the entire manual process of finding trends and writing content. |
| **BEFORE: Manual Process** | Scrolling X manually for 1–2 hours/day: finding trends, evaluating virality, filtering irrelevant posts, then ideating angles and writing content from scratch — not counting additional research time to understand trend context. |
| **AFTER: With Skill** | Input a brand spec `.md` file or a quick brand description (< 1 min). The skill auto-selects relevant subtopics, scans, filters, clusters, and returns 5+ Trend Alerts with ready-to-post draft content — entire process takes 5–10 minutes. Saves ~1.5–2 hours per day. |
| **Tools / AI Used** | Claude (claude.ai) — `web_search` + `web_fetch` to scan X in real time. Input: brand spec `.md` or manual context. Output: Trend Alert blocks + Summary table with urgency ranking. |
| **Limitations** | X (Twitter) limits public data visibility via web search — RT/impression counts may not be 100% accurate. The skill does not access the X API directly, so some private or hidden posts may be missed. |
| **Roadmap** | 1. Integrate X API for precise metrics (impressions, RT, reply count). 2. Add AI scoring to rank trends by relevance per brand. 3. Expand coverage to LinkedIn and Farcaster for Web3 brands. |
