# Evidence: "We ran AI code review on 30 apps"

Page: https://megalens.ai/research/ai-code-review-false-positives

Every number on that page, and the file here it comes from. The files were generated from our run
records, not written by hand (`summary.json` is the generator's own output).

| On the page | Value | Where to check it |
|---|---|---|
| Apps reviewed | 30 | `runs.csv`, 30 rows, each with its run ID and start time (22 September 2026, 19:53 to 20:11 UTC) |
| Runs that finished | 28 (2 stopped part-way) | `runs.csv`, column `outcome` |
| Findings | 160, a median of 5 per app, at least 2 per finished run | `runs.csv`, column `findings`; totals in `summary.json` |
| Models per finished run | three or four | `runs.csv`, column `models_that_answered` (sorted A to Z) |
| Model fees for the batch | $5.67 | `summary.json`, `batch_model_fees_usd` |
| Sample | 20 findings | `sample.csv`, one row per finding |
| Agreed by both checkers | 16: 9 real, 7 not real | `sample.csv`, column `outcome` |
| False-positive rate | 7 of 16, about 44% | `sample.csv` |
| Unresolved | 4 | `sample.csv` |
| Labelled critical | 9, none rated critical by both checkers | `sample.csv`, columns `label_on_the_finding`, `checker_a_…`, `checker_b_…` |
| Raised by several models, real | 5 of 10 | `sample.csv`, column `models_that_raised_it` |
| Raised by one model, real | 4 of 5 | `sample.csv` |
| Which model raised each finding | named per finding | `sample.csv`, column `raised_by` (sorted A to Z; F19 has no recorded model) |
| Update: "verified" label precision | 10 of 14 (71%) before, 10 of 11 (91%) after | `summary.json`, `update_2026_09_23` |
| Update: labelled critical | 9 before, 2 after | `update-2026-09-23.csv` |
| Update: real findings with a severity inside both checkers' range | 2 of 9 before, 7 of 9 after | `update-2026-09-23.csv`, `checker_severity_range` |

## What is not here, and why

- **The apps' names, their code and the findings' text.** They are public projects whose owners did
  not ask to be reviewed. The findings are identified only as F01 to F20.
- **How MegaLens runs a review.** Which model does which job, in what order, with what settings and
  instructions is our own work and is deliberately left out. These files show what came out of each
  run, not how it was produced, and nothing left out is needed to check a number on the page.

`private-records.sha256` fingerprints the full internal records these files were cut from. Anyone we
give those records to, such as an independent auditor, can confirm they have not changed since this
was published.

## Honest limits

A sample of 20 has a wide error bar. The two checkers are AI models from different companies
(Anthropic Claude and OpenAI GPT). No human reviewed the findings, and no exploit was attempted.
