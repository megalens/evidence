# MegaLens and CodeRabbit on the fresh set: second look

This page reports the second look registered in [README.md](README.md#second-look-registered-19-nov-2026-before-any-second-look-run) before any second look run. The first look results in [RESULTS.md](RESULTS.md) stand as published. We make MegaLens, so this is a benchmark of our own product.

The second look reuses a set whose first results we had already seen. The changes between the looks were chosen on separate development sets, not on this set, but this is not an untouched replication.

## What was run
1. MegaLens build `dee6ffdb894afd67a6aa5477bf5ec59a7448b113` with production's review switches (hash in the README). The build did not change during the runs.
2. The same arms as the first look: route 1 (MCP review tool) one run, route 2 (coding assistant plugin) two runs, every valid pull request, up to 5 counted attempts each.
3. The same frozen labels and bug descriptions, the same two blind reviewers and their unchanged instructions, the same tests (A6, A8, A9). Population: 43 items, 1 invalid, 42 valid, 27 catchable.
4. CodeRabbit is its saved reviews, the same bytes as the first look. Its catch judgments from the first look were reused. Its posted comments were judged again inside the new mixed precision pool.
5. As registered, scoring reads every finding in the answer, including the section the tool's own check rejected. The main list result is reported beside it.

Reviewer disagreements on whether a bug was hit, counted as not caught: route 1 run 1: 1, route 2 run 1: 2, route 2 run 2: 3, CodeRabbit: 1.

## 1. Head to head on the same pull requests

Rows use only pull requests where MegaLens completed a review. Route 1 completed 40 of 42, route 2 run 1 completed 42, route 2 run 2 completed 41. Exact two sided McNemar p values, not adjusted for multiple comparisons.

**Primary: catchable bugs**

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 13/26 (32% to 68%) | 4/26 (6.1% to 34%) | 3 | 10 | 1 | 12 | 0.0117 | MegaLens better |
| Route 2, run 1 | 10/27 (22% to 56%) | 4/27 (5.9% to 32%) | 3 | 7 | 1 | 16 | 0.0703 | no significant difference |
| Route 2, run 2 | 12/26 (29% to 65%) | 4/26 (6.1% to 34%) | 3 | 9 | 1 | 13 | 0.0215 | MegaLens better |
| Route 2, either run | 12/27 (28% to 63%) | 4/27 (5.9% to 32%) | 3 | 9 | 1 | 14 | 0.0215 | MegaLens better |

**Secondary: all bugs**

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 17/40 (29% to 58%) | 6/40 (7.1% to 29%) | 4 | 13 | 2 | 21 | 0.0074 | MegaLens better |
| Route 2, run 1 | 13/42 (19% to 46%) | 6/42 (6.7% to 28%) | 5 | 8 | 1 | 28 | 0.0391 | MegaLens better |
| Route 2, run 2 | 16/41 (26% to 54%) | 6/41 (6.9% to 28%) | 5 | 11 | 1 | 24 | 0.0063 | MegaLens better |
| Route 2, either run | 16/42 (25% to 53%) | 6/42 (6.7% to 28%) | 5 | 11 | 1 | 25 | 0.0063 | MegaLens better |

Points to read with these tables:
1. **Main list only.** In route 1, 2 of the 13 catchable catches appear only in the rejected section; both were CodeRabbit misses. Counting the main list alone, route 1 caught 11/26 (only MegaLens 8, only CodeRabbit 1, exact p = 0.039). In route 2, the one rejected section catch in each run is a bug labelled not catchable, so the primary rows do not change.
2. **Sensitivity.** Counting incomplete MegaLens reviews as misses on all 42 valid pull requests gives the same results column for every row.
3. **Run to run.** Two catchable bugs were caught in route 2 run 2 and not in run 1; none the other way. In the first look the count was one.
4. **Both tools together** (catchable, same pull requests): route 1 with CodeRabbit 14/26, route 2 run 1 with CodeRabbit 11/27, route 2 run 2 with CodeRabbit 13/26, against 4 for CodeRabbit alone. Descriptive only.

## 2. What users see

Counts over completed reviews, invalid item excluded. "Main list" is the list of findings shown as the answer; the rejected section is shown separately.

| Run | Main list items per PR | Rejected section per PR | Main list items with a line number | Main list items with a file | Main list items marked verified |
|---|---|---|---|---|---|
| Route 1, run 1 | 3.5 | 2.1 | 60/141 | 103/141 | 29/141 |
| Route 2, run 1 | 3.3 | 2.0 | 61/137 | 117/137 | 52/137 |
| Route 2, run 2 | 3.1 | 1.3 | 48/128 | 88/128 | 39/128 |

In the first look the same runs showed 4.7 to 4.9 items per pull request, no line numbers, and no separate rejected section. CodeRabbit posted 3.8 comments per pull request on these items.

## 3. Precision of posted findings (descriptive only)

Every posted finding from the three runs and every CodeRabbit comment went into one new anonymous pool (786 findings), judged real, not real or unsure by both reviewers.

| Group | Posted | Both real (95% Wilson) | Both not real (95% Wilson) | Disagree | Both unsure |
|---|---|---|---|---|---|
| MegaLens main list | 406 | 162/406 (35% to 45%) | 55/406 (11% to 17%) | 161 | 28 |
| MegaLens rejected section | 221 | 39/221 (13% to 23%) | 75/221 (28% to 40%) | 102 | 5 |
| MegaLens, all sections | 627 | 201/627 (29% to 36%) | 130/627 (18% to 24%) | 263 | 33 |
| CodeRabbit | 159 | 18/159 (7.3% to 17%) | 109/159 (61% to 75%) | 30 | 2 |

CodeRabbit's comments were the same bytes as in the first look, where 25/159 (16%) were judged real by both. In this pool the figure is 18/159 (11%). Judging the same text again moved the result, so differences of that size between pools should not be read as change.

## 4. False "verified" (MegaLens only)

| Run | W/V (95% Wilson) | Contested | Both unsure |
|---|---|---|---|
| Route 1, run 1 | 1/29 (0.6% to 17%) | 6 | 0 |
| Route 2, run 1 | 1/52 (0.3% to 10%) | 13 | 6 |
| Route 2, run 2 | 0/39 (0% to 9.0%) | 19 | 0 |

W is verified findings both reviewers judged not real. Together: 2/120. As in the first look, no claim of a rate at or below 1% is made; that needs about 381 verified findings with none wrong.

## 5. Operational disclosures
1. **Account credits.** Route 2 run 2 stopped part way when the internal benchmark account ran out of service credits. The service refused those calls before any review ran. After a top up, the affected items were run again; the refused attempts are kept in the private run folder and are not counted as attempts, in the spirit of A9 (refusals by the account, not results).
2. **One real failure.** One item failed in route 2 run 2 after all five attempts because the project's own assistant settings block the command that reads the change. It counts as incomplete, as any failure would.
3. **One reviewer's precision pass.** On the precision pool, one reviewer twice judged only the first 37 of 786 findings, because it read only the start of the long case file. It was run a third time with the same instructions plus one line stating the number of cases, and judged all 786. The other reviewer judged all 786 on its first run. The partial outputs are kept privately.
4. Identities are keyed hashes as in the first look. No CodeRabbit review text is published.

Files: [results-second-look/summary.json](results-second-look/summary.json) (every count above), [results-second-look/per-item.csv](results-second-look/per-item.csv) (per item outcomes under keyed IDs).
