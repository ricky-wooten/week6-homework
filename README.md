# Military Recruiting Research — a Claude Skill

**Foundations of AI · Week 6 Homework**

`military-recruiting-research.skill` is a packaged **Claude Skill**: a reusable set of instructions, reference notes, helper scripts, and starter data that teaches Claude how to research U.S. military recruiting and force size the same careful way every time. Instead of re-explaining the task in each conversation, the user can simply say *"update my military recruiting research"* and Claude follows a proven, documented process.

---

## What the skill is about

The skill tracks the size and recruiting health of the U.S. armed forces across:

- **All six branches:** Army, Navy, Marine Corps, Air Force, Space Force, and Coast Guard
- **All components:** Active duty, Reserve, and National Guard

It relies **only on public, unclassified sources**, such as Department of Defense budget documents, Office of People Analytics (OPA) *Population Representation in the Military Services* reports, Congressional Research Service (CRS) reports, and official recruiting press releases.

## The five research questions it answers

| # | Question | What it produces |
|---|----------|------------------|
| 1 | **Annual roll-up:** How large was each branch and component every fiscal year from FY1973 to the latest year? | A year-by-year table with totals for the whole force |
| 2 | **Recruiting goals vs. results:** Did each branch meet its annual enlisted recruiting goal, and by how much did it miss? | Goal, actual, percent of goal, and a MET / MISSED / UNKNOWN flag |
| 3 | **Authorized vs. actual:** Over the last 10 years, how did each branch's real headcount compare with the ceiling Congress authorized? | The gap in people and as a percentage |
| 4 | **Who is joining:** How have the gender, race/ethnicity, and education levels of new recruits changed since FY2000? | Demographic trend tables by branch |
| 5 | **Where recruits come from:** Which states produce the most recruits relative to their 18–24 population? | State "representation ratios" over 10 years |

If the user asks just one of these questions, Claude answers only that one. A general "update my research" runs all five.

## What it accomplishes

- **Saves repeated work.** The skill comes with a verified starting dataset, so each run only has to find what's *new* since the last update.
- **Keeps the numbers trustworthy.** Strict ground rules are built in:
  - Never estimate or fill in a missing number; a clearly marked gap is better than a guess.
  - Every number must record its source so it can be checked later.
  - Keep *actual* end strength (the real headcount on Sept 30) separate from *authorized* end strength (the limit Congress sets).
  - Record only figures labeled "Actual" in budget documents, not projections or requests.
- **Produces ready-to-use outputs.** Each run delivers a short chat summary of key findings, formatted tables, spreadsheet (CSV) files, a written report, and two charts.
- **Reports its own gaps.** The skill automatically lists which years or branches still lack data, so the user always knows what's missing.

## How it works (step by step)

1. **Copy the starter data** into a working folder.
2. **Find the gaps:** a script checks which fiscal years and branches are missing data.
3. **Research the gaps** using web search, guided by a reference guide that says exactly where each number is published and which traps to avoid.
4. **Record each new number** with a helper script that blocks duplicates, misspelled branch names, and rows without a source.
5. **Rebuild the report:** a second script regenerates every table and chart.
6. **Deliver** a summary, the tables, and the updated files to the user, with sources cited.

## What's inside the package

A `.skill` file is a zip archive. Its contents:

```
military-recruiting-research/
├── SKILL.md                         Main instructions Claude follows
├── references/
│   └── sources.md                   Where each number is published, plus known quirks
├── scripts/
│   ├── add_data.py                  Safely adds a new, sourced data row
│   └── build_report.py              Builds the tables, report, and charts
└── data/
    ├── end_strength.csv             Actual end strength, FY1973–FY2024 (318 rows, with gaps)
    ├── authorized_end_strength.csv  Congressionally authorized end strength (166 rows)
    └── recruiting_goals.csv         Recruiting goals vs. results, FY2023–FY2025 (15 rows)
```

### Outputs each run creates

| File | Contents |
|------|----------|
| `rollup_by_year.csv` | One row per fiscal year, one column per branch/component, with totals |
| `authorized_vs_actual.csv` | Gap between authorized and actual strength |
| `recruiting_results.csv` | Goal vs. achieved, with percent of goal and result flag |
| `report.md` | Summary tables in readable form |
| `chart_total_force.png` | Active vs. Reserve + Guard totals over time |
| `chart_active_by_branch.png` | Active-duty size by branch over time |

## Known data gaps (as of September 2026)

The skill is honest about what it hasn't found yet. Open gaps at the last update included FY2023 and FY2025 end-strength actuals, Coast Guard end strength, recruiting goals before FY2023 and most Reserve/Guard goals, some FY2024 recruiting results, and the demographic and state data for questions 4 and 5. Each future run works to close these.

## How to use it

1. Add `military-recruiting-research.skill` to Claude through its Skills settings.
2. Ask a question in plain language, for example:
   - *"Use my military recruiting skill to update the research."*
   - *"Did every branch make its recruiting goal this year?"*
   - *"Give me the total force roll-up with the Coast Guard added."*

Claude recognizes the topic and loads the skill automatically.
