# Step 6 pre-registration, amendment A8: same-items head-to-head (17 Nov 2026)

This amendment is added to the frozen step 6 pre-registration (sha256 9dde543395d0d788caee6f342db0c0cca79482fa8a35b7d8aa0d5aa828305208). It was written and published before any head-to-head result, catch result or tally output was opened. Section A8.9 lists exactly what had been run or seen when it was written. Where this amendment and the original differ, this amendment applies.

## A8.1 Matched PRs (M)
The head-to-head uses only sealed PRs that **both** tools actually reviewed:
- CodeRabbit reviewed a PR if it has a saved pre-merge review with inline comments. These are the 43 PRs labelled `real_review` in the claim set.
- MegaLens reviewed a PR if the run being compared has a completed final record for it. For a route union (Run 1 OR Run 2), a PR counts if at least one counted run completed it.
- M is computed separately for each MegaLens run and for each route union. The report gives |M| and lists, by count, the PRs dropped from the 43 because MegaLens did not complete them.

## A8.2 Eligible bugs (E)
E is the set of frozen, blind-labelled catchable bugs (`catchable: yes`) on M, from the labels frozen before any run. E is the primary denominator for both tools.
All bugs on M, including the ones that are not catchable, are reported next to it as a secondary row.

## A8.3 Same rule and reviewers
Both tools are scored on E with the same exact-catch rule (both blind reviewers say `hit`), the same two blind reviewers, the same frozen reviewer prompts and the same frozen labels. A catch on a bug outside E never counts in either tool's numerator.

## A8.4 What the head-to-head reports
- MegaLens k1/|E| and CodeRabbit k2/|E|, each with its 95% Wilson interval.
- The paired split on E: caught by both, only by MegaLens, only by CodeRabbit, by neither.
- An exact two-sided McNemar test on the discordant bugs (only-MegaLens against only-CodeRabbit).
- **Conclusion rule:** "better" may be said only if the McNemar p is below 0.05 (amendment A6). Otherwise the result is "no significant difference". The paired test replaces the interval-overlap screen of section 2.2 for this comparison, because the two tools are scored on the same bugs. Both intervals are still printed.
- This is reported for each MegaLens run and for each route union.

## A8.5 Sensitivity row (so dropping failed PRs cannot hide failures)
The earlier definition is also reported, labelled "sensitivity": all 43 `real_review` PRs, with a MegaLens PR that did not complete counted as a MegaLens miss. MegaLens completion on the 43 is printed as k/43.

## A8.6 MegaLens on all 100 PRs
MegaLens on all 100 PRs, under section 2.1, is reported in its own table. It is never mixed into a head-to-head number, row or chart.

## A8.7 Precision of posted findings (new metric, both tools)
- **Posted finding:** every finding a tool shows the user on a PR in M.
  - MegaLens: every finding in the completed final record, verified and unverified.
  - CodeRabbit: every inline review comment of its saved review, extracted as in the existing preparation. Review summaries and conversation entries are excluded.
- **How it is assessed:** findings from both tools go into one anonymous, shuffled pool. Tool, run, cost and latency are removed. The same two blind audit reviewers assess each finding against the PR diff, with the frozen audit prompt.
- **Real:** both reviewers say `real`. Disagreements are reported as `contested`, and two `unsure` decisions are reported separately.
- **Precision:** real/posted with its 95% Wilson interval. It is reported two ways, with contested counted as not real and with contested counted as real.
- **Scope:** CodeRabbit on M, and Run 1 of each MegaLens route on that run's M. Run 2 is added if it is assessed by report time; otherwise the report marks it pending and publishes it later. Runs are never chosen by their result.
- Precision is descriptive. No significance claim is made for it unless a paired design is registered first.

## A8.8 False verified
- MegaLens: W/V as in section 2.3, unchanged, with the upper bound reported both ways (excluding and including contested) and the sample-size caveat.
- CodeRabbit: it has no verified label, so W/V is not measurable for CodeRabbit, and the report says so. Its posted-finding precision (A8.7) is the comparable measure.

## A8.9 What had been run or seen when this was written (17 Nov 2026, about 18:10 UTC)
Run, with outputs saved to disk but not opened:
- Recall decisions (both reviewers) for MegaLens route 1, Run 1, and for the CodeRabbit baseline.
- Audit decisions of route 1, Run 1 verified findings: one reviewer completed. The other reviewer covered 37 of 42 cases and is being re-run on all 42.

Seen by the operator: one aggregate line from that incomplete audit reviewer, for route 1, Run 1 verified findings only: 32 real, 3 not real, 2 unsure, out of 37.

Not seen: any catch judgment, any catch rate, any head-to-head count, and any tally output. One tally attempt stopped with an input-completeness error before computing anything.

The amendment changes no reviewer prompt, label, packet or run. The new precision audit (A8.7) uses new packets, prepared after this amendment is published.
