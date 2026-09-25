# Evidence: Cursor's own review vs a MegaLens review of the same plan

Pages: https://megalens.ai/blog/cursor-alternatives and https://megalens.ai/case-studies/cursor-vs-claude-code

Both reviews were run on 25 April 2026 against the same document, the build plan in the public
repository [megalens/pr-drafter](https://github.com/megalens/pr-drafter) (`build-plan.md`). The files
here are cut from the two original review outputs. `source.sha256` fingerprints those originals, so
anyone we give them to, such as an independent auditor, can check this cut against them.

| On the page | Value | Where to check it |
|---|---|---|
| Cursor's own review | 23 findings, 3 critical | `cursor-solo-findings.csv` |
| MegaLens review | 45 findings, 7 critical | `megalens-findings.csv` |
| Difference | +22 findings, +4 critical | 45 minus 23, and 7 minus 3 |
| Found by GPT 5.4 alone, missed by the other three models | 7 of 45 | `megalens-findings.csv`, column `raised_by_gpt_5_4_only` |
| Git config can run arbitrary code (critical) | finding 7 | `megalens-findings.csv` |
| YAML config can execute code (critical) | finding 4 | `megalens-findings.csv` |
| OAuth login can be stolen by a local process | finding 16 | `megalens-findings.csv` |
| Time | 7 min (418 s) | original output, `Total time` |
| Provider cost on our own key | $0.22 | original output, `Total cost` |

**How to read the difference.** +22 and +4 are differences in counts. The two lists were not matched
one to one, so some of the 45 may restate something Cursor also raised in other words.

**What is not here.** The original output labels each piece of evidence with the name of an internal
reviewing role, and records which models missed each finding. Those labels describe how the review
works inside, so this cut keeps only what came out: severity, category, title, and the models that
raised each finding, sorted A to Z. One category, which only ever labelled a single model's own
findings, is shown as "(not categorised)" for the same reason.

The models in this April review (Grok 4.1 Fast, DeepSeek V3.2, Gemini 3.1 Pro, GPT 5.4) are not the
ones MegaLens uses today, so re-running the plan now will not reproduce these exact findings.
