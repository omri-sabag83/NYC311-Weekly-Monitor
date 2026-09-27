# NYC 311 Weekly Monitor

A recurring weekly monitoring report on [NYC 311 service requests](https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2020-to-Present/erm2-nwe9/about_data) —
built for a once-a-week skim by an analyst or manager, not a comprehensive
statistics dump. Each report calls out meaningful changes in request volume,
categories, and geography compared with recent weeks, flags anomalies or
emerging trends, and names areas worth a closer look.

**This repo holds only the deliverable.** It's produced by a separate
automation — [`Automations/04_nyc311_weekly_monitor`](https://github.com/omri-sabag83/Automations/tree/main/04_nyc311_weekly_monitor) —
which runs every Monday 09:00, fetches that week's data itself, and writes
the finished report straight into [`Reports/`](Reports/). See that folder's
README for the schedule, the guardrails, and how it's tested.

## Reports

One Markdown file per week, named for the week it covers (the date is the
week's **start** — the previous Sunday):

```
Reports/weekly_service_requests_YY_MM_DD.md
Reports/charts/                              chart images the reports reference
```

Files are never aggregated, edited, or pruned — each is a standalone,
self-contained snapshot. Every report has two parts: a brief, text-only
**Executive Summary** (≤ 1 page) stating the exact time range covered and
when it was generated, followed by a separate **Detailed Findings** body
whose sections vary week to week — no fixed template, though real charts
(generated fresh each run, not a fixed set) are the norm there — and which
closes with a "Data & Methodology" section listing the exact queries used.

## Methodology, in brief

- **Data**: NYC 311 Service Requests, pulled live each run from the city's
  Socrata SODA API (`https://data.cityofnewyork.us/resource/erm2-nwe9.json`,
  public, no auth) — never a static or pre-downloaded file.
- **Window**: each report covers exactly one calendar week, Sunday 00:00 to
  the following Sunday 00:00, in **America/New_York** (the dataset's own
  timezone).
- **Analysis**: unlike a fixed dashboard, the dimensions examined, what
  counts as an anomaly, and which charts (if any) get drawn are all decided
  fresh each run — by Claude, working directly against the API, not from a
  fixed template. Recent weeks (not analyzed as their own subject) are used
  only as a comparison baseline.
- **Standards applied**: findings are tagged Observation / Hypothesis /
  Conclusion; correlation is distinguished from causation; rate-vs-mix is
  decomposed before attributing a shift to one cause; data-quality caveats
  (e.g. still-open requests near the window edge, geocoding gaps) are
  surfaced up front rather than buried.

## Status

Private while the workflow is being built and shaken out. NYC 311 is public
open data with no sensitivity concerns, so this may move to a public repo
once a few weeks of reports have proven the process out.
