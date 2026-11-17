# Pipeline v2, step 6: pre-registration hashes (17 Nov 2026)

These hashes were published before the sealed 100-PR claim set was opened for labelling. The files they describe are released with the results, and anyone can check that they did not change.

| File | sha256 |
|---|---|
| Pre-registration (rules, arms, metrics, failure handling, order of work) | 9dde543395d0d788caee6f342db0c0cca79482fa8a35b7d8aa0d5aa828305208 |
| Scoring and labelling rules (locked in step 0, with its 17 Nov addendum) | 5da513ff8a54085896e35e82181e466f7e8b5b3d2d68901914d112dfdaa66920 |
| Claim set manifest (100 public PRs, sealed 9 Oct 2026) | 9e7c3d361013026d32f78b1a2a8a887b0449574d50dfb3af77953e1c114ac14b |

What is fixed by the pre-registration:
- Every run is published, not the best one. Each route runs twice; a run that does not complete gets up to 4 more attempts, and every attempt is reported.
- A bug counts as caught only when two independent blind reviewers of the results both say so.
- MegaLens and CodeRabbit are compared only on the same PRs and the same bugs, with a paired test. "Better" is claimed only if that test is significant; otherwise the result is reported as no significant difference.
- Rates are reported as k/n with 95% Wilson intervals. There is no pass/fail gate.

## Labels frozen (17 Nov 2026, before any MegaLens run on the claim set)

Two independent blind labellers labelled all 100 PRs. They agreed on 83 of 100 catchable calls; the 30 items with a disagreement, a tag mismatch or a failed citation check were settled by an adjudicator. Result: 77 of 100 bugs are catchable (66 from the repository alone, 11 needing a pinned official document), 23 are not.

| File | sha256 |
|---|---|
| Final labels (100 items) | 7fe0f7efa077d415b2f89dbac08c8be43935c5ec5ad3b47c83e0e6ebd5a23888 |
| Adjudication, part 1 | dd584a2d40d4b95a4ea882ab8c4c52a196d34028f59a6e4e6e15b72caf7cd709 |
| Adjudication, part 2 | 3c23ef6f9c7ada79364630160361f401a3b3946717b5ce56ce3e8337af3d9695 |
| Archived documentation citations (index) | 14d4a35e9adb3ff85a302c481a3e1cad1f2f48da23326f5a9e4b51cb8d53c209 |

## Build and settings frozen (17 Nov 2026, before any run)

| Item | Value |
|---|---|
| Build (git commit of the service that runs every review) | f2b1b520fbe5d03813271a922d91f1c61cc974cc |
| Settings file (feature switches only, no secrets), sha256 | f0ef0017c4580954261c5e626b14c5b1df3dd83a027bc7a27b32535f38fb7762 |

Every run record carries the build that served it; a run on any other build does not count.

## Amendment A8: same-items head-to-head (17 Nov 2026, before any result was opened)

Written and published before any catch result, head-to-head count or tally output was opened. Full text: [AMENDMENT-A8.md](AMENDMENT-A8.md), sha256 `7ef4e58e933b6da4a5a08e769825c3ae86ea7f99f680ceb4bc3519e7dee5adf0`. In short:
- The head-to-head uses only the PRs that both tools actually reviewed (M), and only the frozen catchable bugs on those PRs (E). Both tools are scored on E with the same catch rule and the same blind reviewers.
- Reported: each tool's k/|E| with a 95% Wilson interval, the paired split (both, only MegaLens, only CodeRabbit, neither) and an exact two-sided McNemar test on the discordant bugs. "Better" only if p < 0.05.
- MegaLens on all 100 PRs is a separate table, never mixed into the head-to-head. A sensitivity row keeps the earlier all-43 definition, with MegaLens failures counted as misses.
- New: precision of posted findings for both tools, assessed blind in one mixed pool. False verified is reported for MegaLens; CodeRabbit has no verified label, so it is not measurable there.
- Section A8.9 of the amendment lists exactly what had already been run or seen when it was written.
