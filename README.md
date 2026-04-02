# Final Weapon Monthly Analytics Report — Claude Skill

A custom Claude skill that transforms raw analytics exports into a comprehensive monthly report for [Final Weapon](https://finalweapon.net), a Japanese gaming publication covering JRPGs, anime, Nintendo, and niche gaming culture.

Drop your data files into the conversation, say "run the report," and get a shareable markdown report with cross-platform KPIs, editorial insights, and prioritized content recommendations.

---

## What it does

Given any combination of **Google Analytics**, **Mediavine**, and **YouTube Studio** exports, the skill:

1. **Parses** each data source, handling comment headers, ZIP archives, encoding quirks, and missing columns gracefully.
2. **Classifies every YouTube video** into one of 15+ content types — recognizing Final Weapon's named series, podcast brands, Shorts subtypes, reviews, previews, guides, and opinion pieces.
3. **Isolates release guide performance** from the rest of the site, since weekly release/schedule guides are the dominant traffic and revenue driver on the Mediavine side.
4. **Computes cross-platform KPIs** including combined revenue, weighted RPM, guide vs. non-guide revenue split, and per-content-type efficiency metrics.
5. **Runs 11 editorial signal analyses** — traffic/revenue mismatches, engagement outliers, YouTube–site alignment gaps, series health checks, CTR anomalies, release guide health, and more.
6. **Writes a structured markdown report** following a detailed template with an executive summary, per-platform deep dives, named series updates, insights & opportunities narrative, and prioritized content recommendations for the next month.

---

## Accepted data sources

| Source | Format | Required? |
|---|---|---|
| Google Analytics | `.csv` with `#`-prefixed comment header rows | No |
| Mediavine | `.csv` with columns: `slug`, `views`, `revenue`, `rpm`, etc. | No |
| YouTube Studio | `.zip` containing `Table data.csv`, `Chart data.csv`, `Totals.csv` — or a standalone `Table data.csv` | No |

At least one source is needed. The report adapts to whatever is provided and notes any missing sources.

---

## YouTube content taxonomy

The skill classifies every video using this priority order:

| Priority | Type | Series / Description |
|---|---|---|
| 1 | `podcast` | *Switch Point* (Nintendo podcast) |
| 1 | `podcast_weaponized` | *Weaponized* (general gaming podcast) |
| 2 | `series_utp` | *Under The Plate* — Final Fantasy VII |
| 2 | `series_btc` | *Beyond The Conduit* — Xeno series (Xenoblade, Xenogears, Xenosaga) |
| 2 | `series_seed` | *SeeD Archives* — Final Fantasy VIII |
| 2 | `series_lost_pages` | *The Lost Pages* — Kingdom Hearts |
| 2 | `series_moonlight` | *In The Moonlight* — Type-Moon / Fate |
| 2 | `series_roe` | *Recollection of Evil* — Resident Evil |
| 2 | `series_memory_card` | *memory//card* — Gaming retrospectives |
| 3 | `short_review` | Shorts repurposing a review |
| 3 | `short_evergreen` | Original Shorts (news hooks, tips, opinions) |
| 4 | `review` | Standalone game/product reviews |
| 5 | `preview` | Hands-on previews and impressions |
| 6 | `oneoff_guide` | Standalone guides, how-tos, tips |
| 7 | `oneoff_opinion` | Everything else (default) |

Full classification rules, examples, edge cases, and performance benchmarks are in [`references/youtube-content-taxonomy.md`](references/youtube-content-taxonomy.md).

---

## Skill structure

```
final-weapon-analytics-report/
├── SKILL.md                         # Main skill instructions
├── scripts/
│   └── parse_slug.py                # Mediavine slug parser + release guide detection
└── references/
    ├── report-template.md           # Full report template with all sections
    ├── editorial-signals.md         # How to interpret each analytical signal
    └── youtube-content-taxonomy.md  # Video classification rules and benchmarks
```

---

## Installation

Download the `.skill` file from [Releases](../../releases) and upload it to Claude as a custom skill. Or point Claude at this repo's `SKILL.md` directly.

---

## Usage

Upload your analytics exports to a Claude conversation and say any of:

- "Run the report"
- "Analyze this month"
- "What did we do this month?"
- "Share with the team"

The skill produces a markdown file ready to share with Final Weapon admins.

---

## Report sections

The output report includes:

- **At a Glance** — Cross-platform KPIs with guide vs. non-guide revenue split
- **Google Analytics** — Top pages, traffic patterns, daily spikes
- **Release Guide Performance** — Top guides by revenue, health flags, new guide opportunities
- **Mediavine Ad Revenue** — Non-guide revenue breakdown, high-RPM opportunities, series grouping
- **YouTube Channel Performance** — Content type table, top videos, Shorts breakdown, named series updates, podcast health, CTR leaders
- **Insights & Opportunities** — 9 editorial insights written in plain English
- **Content Recommendations** — Prioritized action tables for site, release guides, and YouTube

---

*Built for Final Weapon. Powered by Claude.*
