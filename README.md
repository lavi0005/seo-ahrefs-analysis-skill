# SEO Skill: Complete Ahrefs Analysis

A Claude skill that turns Ahrefs exports into a prioritized SEO analysis: keyword opportunities, backlink health, content gaps, and an action plan.

## What it does

| Module | Input (Ahrefs export) | Output |
|---|---|---|
| Keywords | Organic keywords | Ranking distribution, striking-distance keywords (pos 4-20), quick wins, losers, top pages, traffic concentration, branded split |
| Backlinks | Backlinks / Referring domains | DR quality mix, dofollow ratio, link concentration, anchor-text mix, manual-review candidates |
| Content gap | Content Gap | Keywords competitors rank for and you don't, scored by volume, difficulty and competitor count, with cluster hints |
| Technical | Site Audit issues | Guidance in `references/analysis-modules.md` |

Ahrefs exports are often UTF-16 tab-separated; the scripts detect this automatically.

## Repository layout

```
skills/ahrefs-seo-analysis/
  SKILL.md                  # skill entry point (triggering + workflow)
  references/               # export guide, judgment rules, report template
  scripts/                  # analyze_keywords.py, analyze_backlinks.py, content_gap.py
examples/                   # synthetic sample exports for testing
```

## Use with Claude

1. Zip the skill folder: `cd skills && zip -r ahrefs-seo-analysis.zip ahrefs-seo-analysis`
2. In Claude, go to Settings > Capabilities > Skills and upload the zip (Claude Code: copy the folder to `~/.claude/skills/`).
3. Ask: "Analyze these Ahrefs exports for example.com and give me a prioritized SEO plan," and attach your CSVs.

## Use the scripts directly

```bash
pip install -r requirements.txt
cd skills/ahrefs-seo-analysis

python scripts/analyze_keywords.py ../../examples/sample-organic-keywords.csv --dr 25 --brand acme
python scripts/analyze_backlinks.py ../../examples/sample-backlinks.csv --brand acme
python scripts/content_gap.py ../../examples/sample-content-gap.csv --target example.com
```

Each script prints a Markdown report and writes CSVs to `out/`.

## Principles

- Ahrefs traffic and volume are estimates; cross-check with Google Search Console.
- Keyword difficulty is judged relative to the site's DR.
- Link issues are flagged for review; no blanket disavows.
- Every report lists data gaps and confidence.

## License

MIT
