---
name: ahrefs-seo-analysis
description: Run a complete SEO analysis from Ahrefs data - organic keywords, striking-distance and quick-win opportunities, backlink profile health, referring domains, anchor text, content gaps against competitors, top pages, traffic losses, and a prioritized action plan. Use this skill whenever the user shares Ahrefs exports (CSV), mentions Ahrefs, Site Explorer, Keywords Explorer, Content Gap, Site Audit, Domain Rating (DR), backlink audit, competitor SEO comparison, or asks for an SEO report, keyword opportunities, or link-building priorities, even if they do not name Ahrefs explicitly.
---

# Ahrefs SEO Analysis

Turn raw Ahrefs exports (or Ahrefs MCP data, if connected) into a prioritized, client-ready SEO analysis.

## Workflow

1. **Intake** - Confirm the target domain, market/country, goal (leads, sales, traffic), and top 2-5 competitors. If the user gave exports, list what was provided and what is missing (see `references/ahrefs-exports.md`).
2. **Choose modules** - Run only what the data supports (see table below). Say clearly which modules were skipped and which export would unlock them.
3. **Run scripts** - Prefer the bundled scripts for deterministic number-crunching; read the generated CSVs/markdown rather than eyeballing raw exports.
4. **Interpret** - Apply the judgment rules in `references/analysis-modules.md`. Numbers without a "so what" are not a finding.
5. **Report** - Use `references/report-template.md`. Lead with the 5 highest-impact actions, then supporting evidence.

## Modules and scripts

| Module | Input export | Script |
|---|---|---|
| Keyword opportunities (striking distance, quick wins, losers, top pages) | Site Explorer > Organic keywords | `scripts/analyze_keywords.py` |
| Backlink health (DR mix, dofollow ratio, anchors, risky links) | Site Explorer > Backlinks and/or Referring domains | `scripts/analyze_backlinks.py` |
| Content gap | Content Gap export (target + competitors) | `scripts/content_gap.py` |
| Technical | Site Audit issues export | no script - follow `references/analysis-modules.md` |

Run from the skill folder:

```bash
python scripts/analyze_keywords.py organic-keywords.csv --dr 35 --out out/keywords
python scripts/analyze_backlinks.py backlinks.csv --brand "acme" --out out/backlinks
python scripts/content_gap.py content-gap.csv --target example.com --min-competitors 2 --out out/gap
```

Requires Python 3.9+ and `pandas` (`pip install pandas`).

## Rules that matter

- **Ahrefs exports are often UTF-16 tab-separated.** The scripts handle this; if you read files yourself, do not assume UTF-8 commas.
- **Traffic and volume are estimates.** Say "estimated". Never present Ahrefs traffic as analytics truth; recommend cross-checking with Google Search Console.
- **KD is relative.** Judge feasibility against the target's DR, not in isolation. A KD 30 keyword is easy for DR 60, hard for DR 10.
- **Intent before volume.** Do not recommend a high-volume keyword whose intent does not match the business offer.
- **Do not recommend a blanket disavow.** Flag candidates, explain why, and treat disavow as a last resort. Google ignores most spammy links.
- **Be market-specific.** Check that the export country matches the market the business serves.
- **State confidence and gaps.** If only one export was provided, say the analysis is partial.

## Output

Deliver the report in the format the user wants (chat summary by default; Markdown or a document if asked). Always include: executive summary, top 5 prioritized actions with expected impact and effort, evidence tables, and what data would improve the analysis.
