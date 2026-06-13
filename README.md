# Daily Football Tips Routine

This routine runs every day at **2:00 PM** and posts the following day's football betting tips to Slack.

---

## Overview

The routine generates and posts three tip sections for the following day's matches:

1. **Daily Football Acca** — match result/handicap selections with detailed reasoning
2. **Daily BTTS Acca** — Both Teams To Score Yes/No selections with reasoning
3. **Daily Bet Builder** — 3 legs from a single match with reasoning for each leg

All content is posted as a single combined Slack thread to channel **C0AVBC6256C**.

---

## Step-by-Step Instructions

### Step 1 — Fetch fixture data

Use the **n8n MCP tool** (workflow ID: `2Ki8OH2pDXd7J0DI`) to fetch the following day's football fixtures.

Expected data from the workflow:
- Fixture list (teams, kick-off times, competition)
- Current odds for match result, handicap, BTTS
- Team form (recent results)
- Injury and suspension news
- Any other relevant match context

If the workflow returns incomplete data (blank fields, missing odds, impossible values, or dates outside the expected range), flag the issue and stop. Do not proceed with bad data.

If the workflow cannot return data for a specific field, supplement **only from these approved sources**:
- BBC Sport
- Eurosport
- Flashscore

Do not use any other source.

---

### Step 2 — Validate all data

Before generating any content, check:
- [ ] All fixtures have valid team names and kick-off times
- [ ] Dates match the expected next-day range
- [ ] Odds are present and within realistic ranges
- [ ] No blank or null fields in key columns

If validation fails on any fixture, exclude that fixture and log the issue. If fewer than 2 valid fixtures remain, post the following to Slack and stop:

> Routine failed: insufficient valid fixture data to generate tips.

---

### Step 3 — Select picks

#### Daily Football Acca
- Select **3 to 5 matches** from the following day's fixtures
- For each match, choose the strongest selection (match result, handicap, or other market)
- Base selections on: current form, head-to-head, injuries, odds value, and competition context
- Avoid heavy favourites priced below 1/5 unless conviction is extremely high
- Aim for a balanced acca with genuine reasoning behind every pick

#### Daily BTTS Acca
- Select **3 to 5 matches** (can be the same matches as the football acca or different — choose whichever gives the strongest tips)
- For each match, pick BTTS Yes or BTTS No
- Base the selection on: recent scoring and clean sheet records for both teams, attacking quality, defensive solidity, and match context

#### Daily Bet Builder
- Select **1 match** from the following day's fixtures — choose the match with the most interesting single-game angles
- Build **3 legs** from that one match
- Leg types can include: player to score, player to score or assist, player shots on target, player fouls won, team to win a half, total goals markets, or similar
- Each leg must have a clear, stats-based reason for inclusion

---

### Step 4 — Generate written content

Write all content as per the format below. Content must:
- Sound natural and human — no AI-sounding phrasing, bullet points, or generic filler
- Include real data (form, stats, injuries) woven into flowing paragraphs
- Match the tone of the examples in this README
- Be specific — name players, reference recent results, cite relevant stats

#### Football Acca format (repeat per selection):
```
[Match Name] - [Selection]
[Paragraph 1: Context on the weaker/underdog side — form, problems, why they struggle here]
[Paragraph 2: Case for the selection — form, key players, why they win/cover]
[Paragraph 3: Closing conviction — why this is a confident pick, reference odds if relevant]
```

#### BTTS format (repeat per selection):
```
[Match Name] - BTTS [Yes/No]
[Paragraph 1: Scoring/defensive form for side A — why they score or don't]
[Paragraph 2: Scoring/defensive form for side B — why they score or don't]
[Paragraph 3: Conclusion — why Yes or No is the right call]
```

#### Bet Builder format (repeat per leg):
```
[Player/Team — Leg Selection]
[Paragraph: Why this leg lands — reference current form, role in the team, opponent's tendencies, relevant stats. 3–5 sentences.]
```

---

### Step 5 — Run all content through /the-humanizer

This step is **mandatory and cannot be skipped.**

Before posting anything to Slack, run every section of generated content through `/the-humanizer`.

If the humanizer output changes the meaning of any selection or tip, revert to the pre-humanizer version for that selection only and flag it in the Slack thread.

---

### Step 6 — Post to Slack

Post to channel **C0AVBC6256C** only. Use the Slack MCP tool — no bot token.

**Structure:**

**Main message** (keep under 4,500 characters):
```
:football: Daily Tips — [Date of matches, e.g. Sunday 15 June]

Today's three tips sections are ready. Full breakdowns in the thread below.

:white_check_mark: Football Acca: [X selections]
:white_check_mark: BTTS Acca: [X selections]
:white_check_mark: Bet Builder: [Match name] — 3 legs
```

**Thread reply 1 — Football Acca:**
Full written content for all football acca selections.

**Thread reply 2 — BTTS Acca:**
Full written content for all BTTS selections.

**Thread reply 3 — Bet Builder:**
Full written content for all 3 bet builder legs, preceded by the match name and a one-line intro.

If any single thread reply exceeds 4,500 characters, split it into further replies and label them (e.g. "Football Acca — continued").

---

## Failure handling

| Situation | Action |
|---|---|
| README.md not accessible at session start | Post to Slack: `Routine failed: README.md not accessible` and stop |
| n8n workflow fails to return data | Post to Slack: `Routine failed: n8n workflow returned no data` and stop |
| Validation fails on all/most fixtures | Post to Slack: `Routine failed: insufficient valid fixture data` and stop |
| /the-humanizer unavailable | Stop. Do not post unhumanized content |
| Slack post fails | Retry once. If it fails again, stop and log the error |

---

## Content tone — example extracts

The writing should match this style:

> Qatar have been appalling in the build-up, losing three of their last four competitive matches. Their 2025 Arab Cup campaign ended at the group stage following a 3-0 defeat to Tunisia — poor form for a side about to face one of the tournament favourites.

> Brazil will create and score, but keeping Morocco off the scoresheet looks like a big ask. Hard to see a clean sheet for either side.

> The McGinn leg could be the hidden gem of the builder. Haiti play with plenty of energy and physicality, averaging a high foul count throughout qualification, and they are likely to spend much of the match trying to disrupt Scotland's rhythm.

Always: specific, confident, grounded in data, written in a natural voice.

---

## Reference

- Slack channel: `C0AVBC6256C`
- n8n workflow ID: `2Ki8OH2pDXd7J0DI`
- Approved data sources: BBC Sport, Eurosport, Flashscore
- Mandatory skill: `/the-humanizer` (run before any Slack post)
- Schedule: daily at 2:00 PM, tips for the following day's matches
