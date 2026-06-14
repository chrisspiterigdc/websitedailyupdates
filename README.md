# Daily Football Tips Routine

This routine runs every day at **2:00 PM BST** and posts upcoming football betting tips to Slack.

---

## Overview

The routine generates and posts three tip sections for upcoming matches:

1. **Daily Football Acca** — match result/handicap selections with detailed reasoning
2. **Daily BTTS Acca** — Both Teams To Score Yes/No selections with reasoning
3. **Daily Bet Builder** — 3 legs from a single match with reasoning for each leg

All content is posted as a single combined Slack thread to channel **C0AT6L5DSNQ**.

---

## Step-by-Step Instructions

### Step 1 — Fetch fixture data

Use the **n8n MCP tool** (workflow ID: `Wd4kzM0Oa7G6dv4X`) to fetch upcoming fixtures. Call `execute_workflow` with no input — the workflow calculates the correct time window automatically (fixtures starting between 8 and 30 hours from now).

Expected data from the workflow:
- Fixture list (teams, kick-off times, competition)
- Status (unplayed only)

If the workflow returns no fixtures or fails, post to Slack and stop:
> Routine failed: n8n workflow returned no data

If the workflow cannot return data for a specific field, supplement **only from these approved sources**:
- BBC Sport
- Eurosport
- Flashscore

Do not use any other source.

---

### Step 2 — Validate all data

Before generating any content, check:
- [ ] All fixtures have valid team names and kick-off times
- [ ] Status is "unplayed"
- [ ] At least 2 valid fixtures returned

If fewer than 2 valid fixtures remain after validation, post to Slack and stop:
> Routine failed: insufficient valid fixture data to generate tips

---

### Step 3 — Select picks

#### Daily Football Acca
- Select **3 to 5 matches** from the fixture list
- For each match, choose the strongest selection (match result, handicap, or other market)
- Base selections on: current form, head-to-head, injuries, odds value, and competition context
- Avoid heavy favourites priced below 1/5 unless conviction is extremely high
- Aim for a balanced acca with genuine reasoning behind every pick

#### Daily BTTS Acca
- Select **3 to 5 matches** (can overlap with football acca or be different)
- For each match, pick BTTS Yes or BTTS No
- Base on: recent scoring and clean sheet records, attacking quality, defensive solidity, match context

#### Daily Bet Builder
- Select **1 match** from the fixture list
- Build **3 legs** from that one match
- Leg types: player to score, player to score or assist, player shots on target, player fouls won, team to win a half, total goals markets, or similar
- Each leg must have a clear, stats-based reason for inclusion

---

### Step 4 — Generate written content

Write all content in the style of the examples below. Content must:
- Sound natural and human — no AI-sounding phrasing, bullet points, or generic filler
- Include real data (form, stats, injuries) woven into flowing paragraphs
- Be specific — name players, reference recent results, cite relevant stats
- Use no em dashes (—) anywhere — replace with commas, periods, colons, or parentheses

#### Football Acca format (repeat per selection):
```
[Match Name] - [Selection]
[Paragraph 1: Context on the weaker/underdog side — form, problems, why they struggle here]
[Paragraph 2: Case for the selection — form, key players, why they win/cover]
[Paragraph 3: Closing conviction — why this is a confident pick]
```

#### BTTS format (repeat per selection):
```
[Match Name] - BTTS [Yes/No]
[Paragraph 1: Scoring/defensive form for side A]
[Paragraph 2: Scoring/defensive form for side B]
[Paragraph 3: Conclusion — why Yes or No]
```

#### Bet Builder format (repeat per leg):
```
[Player/Team — Leg Selection]
[Paragraph: Why this leg lands — form, role, opponent tendencies, stats. 3-5 sentences.]
```

---

### Step 5 — Humanize all content

Before posting, review every section of generated content and apply the humanizer rules.

If the `/the-humanizer` skill is available as a callable command, run the content through it.

If `/the-humanizer` is not available as a callable command, read `.claude/skills/the-humanizer/SKILL.md` from this repository and apply the rewrite rules manually. **Do not stop — continue with the manual application.**

Key rules to apply in either case:
- No em dashes anywhere
- No AI vocabulary (delve, leverage, tapestry, nuanced, foster, utilize, seamless, robust, holistic, etc.)
- No filler openers ("In today's...", "When it comes to...", "At the end of the day")
- No summary closers — end with a conviction statement or open point
- Vary sentence length — mix short punchy lines with longer analytical ones
- Be specific: name players, cite stats, reference real results

---

### Step 6 — Post to Slack

Post to channel **C0AT6L5DSNQ** only. Use the Slack MCP tool.

**Main message:**
```
:football: Daily Tips — [Date]

Tips ready. Full breakdowns in the thread below.

:white_check_mark: Football Acca: [X selections]
:white_check_mark: BTTS Acca: [X selections]
:white_check_mark: Bet Builder: [Match name] — 3 legs
```

**Thread reply 1 — Football Acca**
**Thread reply 2 — BTTS Acca**
**Thread reply 3 — Bet Builder**

If any reply exceeds 4,500 characters, split into further replies labelled (e.g. "Football Acca — continued").

---

## Failure handling

| Situation | Action |
|---|---|
| README.md not accessible | Post to Slack: `Routine failed: README.md not accessible` and stop |
| n8n workflow fails | Post to Slack: `Routine failed: n8n workflow returned no data` and stop |
| Fewer than 2 valid fixtures | Post to Slack: `Routine failed: insufficient valid fixture data` and stop |
| `/the-humanizer` skill not callable | Read `.claude/skills/the-humanizer/SKILL.md` and apply rules manually. Do not stop. |
| Slack post fails | Retry once. If it fails again, stop and log the error. |

---

## Content tone — example extracts

> Qatar have been appalling in the build-up, losing three of their last four competitive matches. Their 2025 Arab Cup campaign ended at the group stage following a 3-0 defeat to Tunisia, poor form for a side about to face one of the tournament favourites.

> Brazil will create and score, but keeping Morocco off the scoresheet looks like a big ask. Hard to see a clean sheet for either side.

> The McGinn leg could be the hidden gem of the builder. Haiti play with plenty of energy and physicality, averaging a high foul count throughout qualification, and they are likely to spend much of the match trying to disrupt Scotland's rhythm.

Always: specific, confident, grounded in data, written in a natural voice.

---

## Reference

- Slack channel: `C0AT6L5DSNQ`
- n8n workflow ID: `Wd4kzM0Oa7G6dv4X` (fetches World Cup fixtures, 8-30hr window)
- Approved data sources: BBC Sport, Eurosport, Flashscore
- Humanizer skill: `.claude/skills/the-humanizer/SKILL.md`
- Schedule: daily at 13:00 UTC (2pm BST)
