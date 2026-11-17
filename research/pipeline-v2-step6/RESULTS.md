# MegaLens and CodeRabbit on 100 sealed PRs: results (18 Nov 2026)

## How this was run, in order
1. **Frozen before the claim set was opened (17 Nov):**
   - the pre-registration;
   - the scoring rules;
   - the claim set manifest.

   Their hashes are in [README.md](README.md).
2. **Frozen before any MegaLens run on the claim set:**
   - the labels: which bugs could be caught from what was visible at review time;
   - the build of the service;
   - its review feature switches.
3. **Amendment [A8](AMENDMENT-A8.md):**
   - What it changed: the head-to-head became a same-items comparison, and precision was added.
   - When: after route 1, run 1 had run, and after part of one reviewer's audit of that run had been seen. That part is listed in section 9.
   - It came before any catch result was seen.
4. **Amendment [A9](AMENDMENT-A9.md):**
   - What it changed:
     - quota refusals no longer count as attempts;
     - affected runs resumed;
     - the service's per-account call limit was raised during the campaign;
     - everything is shown under the original protocol too.
   - When: after an operational failure had been observed (the service refused calls for quota), and before any re-run or any result was seen.
5. **What did not change after the claim set was opened:**
   - the labels;
   - the build;
   - the review feature switches;
   - the reviewers' prompts;
   - the catch rule.

The setup:
- MegaLens ran on its frozen build `f2b1b52` through two routes.
  - Route 1 is the MCP review tool.
  - Route 2 is the coding-assistant plugin.
- Each route ran twice. Both runs are published, and neither was picked as the better one.
- CodeRabbit is represented by the real reviews it posted on these PRs at the time. It was not re-run.
- Two independent reviewers assess each anonymised output against the bug description, without knowing which tool wrote it.
- A bug counts as caught only when both reviewers say "hit". When they disagree, it is not a catch. The number of such disagreements:

| Run | Disagreements |
|---|---|
| Route 1, run 1 | 4 |
| Route 1, run 2 | 4 |
| Route 2, run 1 | 3 |
| Route 2, run 2 | 2 |
| CodeRabbit | 2 |

## 1. Short answer

Under the governing rules (A8 with A9), MegaLens caught more of the catchable bugs than CodeRabbit on the PRs both tools reviewed, in every run. The paired test was significant each time (p = 0.021 or lower).
- Per run, MegaLens caught 15 to 23 of 31 to 33 catchable bugs. CodeRabbit caught 5 of the same bugs.
- The sample is small: 43 PRs and 31 to 33 catchable bugs in the comparison. The intervals are wide.
- On route 1, run 1, the 95% intervals of the two tools overlap slightly. See section 2.
- Under the original protocol, where quota refusals use up attempts (section 7), only route 2 shows a significant difference.
  - Route 2, run 2 is significant in every original-protocol table.
  - Route 2, run 1 is significant on its own M but not on all 43 PRs.
  - Route 1 is not significant in any original-protocol table.

## 2. Head-to-head (A8, the governing comparison)

The comparison covers only the PRs both tools actually reviewed (M), and only the frozen catchable bugs on those PRs (E). It uses the same catch rule and the same reviewers for both tools. The test is an exact two-sided McNemar test on the bugs only one tool caught. "Better" is said only when p < 0.05.

M is 40 PRs for route 1: route 1 did not complete 3 of the 43 PRs (section 6). M is 43 PRs for route 2. E is 31 bugs for route 1 and 33 for route 2.

**Primary: catchable bugs on M**

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 15/31 (32% to 65%) | 5/31 (7.1% to 33%) | 2 | 13 | 3 | 13 | 0.021 | MegaLens better |
| Route 1, run 2 | 19/31 (44% to 76%) | 5/31 (7.1% to 33%) | 3 | 16 | 2 | 10 | 0.0013 | MegaLens better |
| Route 2, run 1 | 17/33 (35% to 67%) | 5/33 (6.7% to 31%) | 3 | 14 | 2 | 14 | 0.0042 | MegaLens better |
| Route 2, run 2 | 23/33 (53% to 83%) | 5/33 (6.7% to 31%) | 4 | 19 | 1 | 9 | 0.00004 | MegaLens better |
| Route 1, either run | 21/31 (50% to 81%) | 5/31 (7.1% to 33%) | 3 | 18 | 2 | 8 | 0.0004 | MegaLens better |
| Route 2, either run | 23/33 (53% to 83%) | 5/33 (6.7% to 31%) | 4 | 19 | 1 | 9 | 0.00004 | MegaLens better |

**Secondary: all bugs on M**, including bugs labelled not catchable

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 16/40 (26% to 55%) | 7/40 (8.7% to 32%) | 3 | 13 | 4 | 20 | 0.049 | MegaLens better |
| Route 1, run 2 | 21/40 (37% to 67%) | 7/40 (8.7% to 32%) | 4 | 17 | 3 | 16 | 0.0026 | MegaLens better |
| Route 2, run 1 | 19/43 (30% to 59%) | 7/43 (8.1% to 30%) | 4 | 15 | 3 | 21 | 0.0075 | MegaLens better |
| Route 2, run 2 | 25/43 (43% to 72%) | 7/43 (8.1% to 30%) | 5 | 20 | 2 | 16 | 0.00012 | MegaLens better |
| Route 1, either run | 24/40 (45% to 74%) | 7/40 (8.7% to 32%) | 5 | 19 | 2 | 14 | 0.00022 | MegaLens better |
| Route 2, either run | 25/43 (43% to 72%) | 7/43 (8.1% to 30%) | 5 | 20 | 2 | 16 | 0.00012 | MegaLens better |

**Sensitivity: all 43 PRs that have a CodeRabbit review.** A MegaLens non-completion counts as a miss.

Completion on those 43 PRs:

| MegaLens run | Completed (95% Wilson) |
|---|---|
| Route 1, run 1 | 40/43 (81% to 98%) |
| Route 1, run 2 | 40/43 (81% to 98%) |
| Route 2, run 1 | 43/43 (92% to 100%) |
| Route 2, run 2 | 43/43 (92% to 100%) |
| Route 1, either run | 40/43 (81% to 98%) |
| Route 2, either run | 43/43 (92% to 100%) |

Catchable bugs:

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 15/33 (30% to 62%) | 5/33 (6.7% to 31%) | 2 | 13 | 3 | 15 | 0.021 | MegaLens better |
| Route 1, run 2 | 19/33 (41% to 73%) | 5/33 (6.7% to 31%) | 3 | 16 | 2 | 12 | 0.0013 | MegaLens better |
| Route 2, run 1 | 17/33 (35% to 67%) | 5/33 (6.7% to 31%) | 3 | 14 | 2 | 14 | 0.0042 | MegaLens better |
| Route 2, run 2 | 23/33 (53% to 83%) | 5/33 (6.7% to 31%) | 4 | 19 | 1 | 9 | 0.00004 | MegaLens better |
| Route 1, either run | 21/33 (47% to 78%) | 5/33 (6.7% to 31%) | 3 | 18 | 2 | 10 | 0.0004 | MegaLens better |
| Route 2, either run | 23/33 (53% to 83%) | 5/33 (6.7% to 31%) | 4 | 19 | 1 | 9 | 0.00004 | MegaLens better |

All bugs:

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 16/43 (24% to 52%) | 7/43 (8.1% to 30%) | 3 | 13 | 4 | 23 | 0.049 | MegaLens better |
| Route 1, run 2 | 21/43 (35% to 63%) | 7/43 (8.1% to 30%) | 4 | 17 | 3 | 19 | 0.0026 | MegaLens better |
| Route 2, run 1 | 19/43 (30% to 59%) | 7/43 (8.1% to 30%) | 4 | 15 | 3 | 21 | 0.0075 | MegaLens better |
| Route 2, run 2 | 25/43 (43% to 72%) | 7/43 (8.1% to 30%) | 5 | 20 | 2 | 16 | 0.00012 | MegaLens better |
| Route 1, either run | 24/43 (41% to 70%) | 7/43 (8.1% to 30%) | 5 | 19 | 2 | 17 | 0.00022 | MegaLens better |
| Route 2, either run | 25/43 (43% to 72%) | 7/43 (8.1% to 30%) | 5 | 20 | 2 | 16 | 0.00012 | MegaLens better |

**Two points to read with these tables:**
- On route 1, run 1, the Wilson intervals of the two tools overlap slightly:
  - catchable bugs: 32% to 65% against 7.1% to 33%;
  - all bugs: 26% to 55% against 8.7% to 32%.
  - Amendment A6 makes the paired test decide "better", and the amendments take precedence over the base text. Under the base text's older rule ("overlap = inconclusive"), this run would read "inconclusive".
  - Its all-bugs p = 0.049 is close to the 0.05 line.
- Run to run variation is real. Route 2 caught 17 of 33 in run 1 and 23 of 33 in run 2.

## 3. MegaLens on all 100 PRs (separate from the head-to-head)

A PR that did not complete counts as a miss.

| Run | All bugs (n=100) | Catchable bugs (n=77) | Catchable from the repository alone (n=66) |
|---|---|---|---|
| Route 1, run 1 | 39/100 (30% to 49%) | 35/77 (35% to 57%) | 34/66 (40% to 63%) |
| Route 1, run 2 | 44/100 (35% to 54%) | 39/77 (40% to 62%) | 37/66 (44% to 67%) |
| Route 2, run 1 | 45/100 (36% to 55%) | 40/77 (41% to 63%) | 38/66 (46% to 69%) |
| Route 2, run 2 | 54/100 (44% to 63%) | 48/77 (51% to 72%) | 46/66 (58% to 79%) |
| Route 1, either run | 48/100 (38% to 58%) | 42/77 (43% to 65%) | 40/66 (49% to 71%) |
| Route 2, either run | 56/100 (46% to 65%) | 49/77 (52% to 73%) | 47/66 (59% to 81%) |

## 4. Precision of posted findings (descriptive, no significance claim)

Every finding each tool posted on the 43 PRs went into one anonymous mixed pool. The same two reviewers judged each finding as real, not real or unsure.
- "Contested" means the two reviewers gave different labels.
- The table shows the precision twice: once counting contested findings as not real, and once counting them as real.
- Findings where both reviewers said "unsure" are counted separately.

| Scope | Posted | Real (both) | Contested | Both unsure | Contested counted as not real (95% Wilson) | Contested counted as real (95% Wilson) |
|---|---|---|---|---|---|---|
| CodeRabbit | 160 | 36 | 20 | 14 | 36/160 (17% to 30%) | 56/160 (28% to 43%) |
| MegaLens, all 4 runs | 738 | 299 | 272 | 66 | 299/738 (37% to 44%) | 571/738 (74% to 80%) |
| Route 1, run 1 | 174 | 78 | 68 | 9 | 78/174 (38% to 52%) | 146/174 (78% to 89%) |
| Route 1, run 2 | 185 | 74 | 70 | 20 | 74/185 (33% to 47%) | 144/185 (71% to 83%) |
| Route 2, run 1 | 185 | 72 | 66 | 17 | 72/185 (32% to 46%) | 138/185 (68% to 80%) |
| Route 2, run 2 | 194 | 75 | 68 | 20 | 75/194 (32% to 46%) | 143/194 (67% to 79%) |

- The reported precision changes a lot depending on how contested findings are counted. For MegaLens, 272 of 738 findings were contested.
- On the 40 PRs that route 1 completed, CodeRabbit's figures are 36/152 (18% to 31%) and 55/152 (29% to 44%).

## 5. False "verified" (MegaLens only)

MegaLens marks some findings as verified.
- W counts the verified findings that both reviewers judged not real.
- V is the number of verified findings.
- CodeRabbit has no verified label, so this cannot be measured for it.

| Run | W/V (95% Wilson) | Contested | Both unsure | Wrong or contested, over V (95% Wilson) |
|---|---|---|---|---|
| Route 1, run 1 | 0/71 (0% to 5.1%) | 6 | 4 | 6/71 (3.9% to 17%) |
| Route 1, run 2 | 1/70 (0.3% to 7.7%) | 21 | 0 | 22/70 (22% to 43%) |
| Route 2, run 1 | 4/87 (1.8% to 11%) | 18 | 3 | 22/87 (17% to 35%) |
| Route 2, run 2 | 2/93 (0.6% to 7.5%) | 37 | 0 | 39/93 (32% to 52%) |

- To show a rate of 1% or less would need about 381 verified findings with none wrong, or about 563 with one wrong. No such claim is made here.

## 6. Completion and attempts

| Run | Completed (95% Wilson) | All attempts | Counted attempts | Quota-refused attempts (A9) | Counted retries |
|---|---|---|---|---|---|
| Route 1, run 1 | 93/100 (86% to 97%) | 315 | 130 | 185 | 30 |
| Route 1, run 2 | 93/100 (86% to 97%) | 480 | 137 | 343 | 37 |
| Route 2, run 1 | 100/100 (96% to 100%) | 238 | 101 | 137 | 1 |
| Route 2, run 2 | 100/100 (96% to 100%) | 102 | 102 | 0 | 2 |

Counted failed attempts, by cause. Every attempt is in `results/attempts.csv`.

| Run | Input over the service's size limit | Transport error or timeout | Plugin delivery check failed |
|---|---|---|---|
| Route 1, run 1 | 35 attempts on 7 PRs | 2 attempts on 2 PRs | 0 |
| Route 1, run 2 | 35 attempts on 7 PRs | 9 attempts on 8 PRs | 0 |
| Route 2, run 1 | 0 | 0 | 1 attempt |
| Route 2, run 2 | 0 | 0 | 2 attempts |

- The 7 PRs that route 1 did not complete in each run were too large for the service's input limit. They count as misses in section 3.
- CodeRabbit has a review on 43 of the 100 PRs. The other 57 had only a summary or no review, so they are not in the head-to-head.

## 7. Original protocol (A9.5: quota refusals counted as failed attempts)

This section shows the effect of A9. A PR-run whose first 5 attempts all failed, quota refusals included, counts as a miss. Under A9, the affected runs resumed with their remaining counted attempts. Under the original protocol, they would have stopped there.

Route 2, run 2 met no quota refusals, so it is the same under both protocols.

**MegaLens on all 100 PRs, original protocol**

| Run | Completed (95% Wilson) | All bugs (95% Wilson) | Catchable bugs (95% Wilson) |
|---|---|---|---|
| Route 1, run 1 | 61/100 (51% to 70%) | 21/100 (14% to 30%) | 18/77 (15% to 34%) |
| Route 1, run 2 | 25/100 (18% to 34%) | 11/100 (6.3% to 19%) | 9/77 (6.3% to 21%) |
| Route 2, run 1 | 75/100 (66% to 82%) | 33/100 (25% to 43%) | 30/77 (29% to 50%) |
| Route 2, run 2 | 100/100 (96% to 100%) | 54/100 (44% to 63%) | 48/77 (51% to 72%) |
| Route 1, either run | 67/100 (57% to 75%) | 27/100 (19% to 36%) | 23/77 (21% to 41%) |
| Route 2, either run | 100/100 (96% to 100%) | 54/100 (44% to 63%) | 48/77 (51% to 72%) |

**Head-to-head, original protocol, on each run's own M**

M is the set of the 43 PRs that the run completed within its first 5 attempts:

| Run | PRs in M |
|---|---|
| Route 1, run 1 | 25 |
| Route 1, run 2 | 10 |
| Route 2, run 1 | 32 |
| Route 2, run 2 | 43 |
| Route 1, either run | 29 |
| Route 2, either run | 43 |

Catchable bugs on M:

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 7/20 (18% to 57%) | 4/20 (8.1% to 42%) | 1 | 6 | 3 | 10 | 0.508 | no significant difference |
| Route 1, run 2 | 5/8 (31% to 86%) | 1/8 (2.2% to 47%) | 1 | 4 | 0 | 3 | 0.125 | no significant difference |
| Route 2, run 1 | 13/26 (32% to 68%) | 4/26 (6.1% to 34%) | 2 | 11 | 2 | 11 | 0.022 | MegaLens better |
| Route 2, run 2 | 23/33 (53% to 83%) | 5/33 (6.7% to 31%) | 4 | 19 | 1 | 9 | 0.00004 | MegaLens better |
| Route 1, either run | 11/24 (28% to 65%) | 4/24 (6.7% to 36%) | 1 | 10 | 3 | 10 | 0.092 | no significant difference |
| Route 2, either run | 23/33 (53% to 83%) | 5/33 (6.7% to 31%) | 4 | 19 | 1 | 9 | 0.00004 | MegaLens better |

All bugs on M:

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 8/25 (17% to 52%) | 6/25 (11% to 43%) | 2 | 6 | 4 | 13 | 0.754 | no significant difference |
| Route 1, run 2 | 6/10 (31% to 83%) | 1/10 (1.8% to 40%) | 1 | 5 | 0 | 4 | 0.062 | no significant difference |
| Route 2, run 1 | 15/32 (31% to 64%) | 5/32 (6.9% to 32%) | 3 | 12 | 2 | 15 | 0.013 | MegaLens better |
| Route 2, run 2 | 25/43 (43% to 72%) | 7/43 (8.1% to 30%) | 5 | 20 | 2 | 16 | 0.00012 | MegaLens better |
| Route 1, either run | 13/29 (28% to 62%) | 6/29 (9.8% to 38%) | 2 | 11 | 4 | 12 | 0.118 | no significant difference |
| Route 2, either run | 25/43 (43% to 72%) | 7/43 (8.1% to 30%) | 5 | 20 | 2 | 16 | 0.00012 | MegaLens better |

**Head-to-head, original protocol, on all 43 PRs** (a non-completion is a miss)

Catchable bugs:

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 7/33 (11% to 38%) | 5/33 (6.7% to 31%) | 1 | 6 | 4 | 22 | 0.754 | no significant difference |
| Route 1, run 2 | 5/33 (6.7% to 31%) | 5/33 (6.7% to 31%) | 1 | 4 | 4 | 24 | 1 | no significant difference |
| Route 2, run 1 | 13/33 (25% to 56%) | 5/33 (6.7% to 31%) | 2 | 11 | 3 | 17 | 0.057 | no significant difference |
| Route 2, run 2 | 23/33 (53% to 83%) | 5/33 (6.7% to 31%) | 4 | 19 | 1 | 9 | 0.00004 | MegaLens better |
| Route 1, either run | 11/33 (20% to 50%) | 5/33 (6.7% to 31%) | 1 | 10 | 4 | 18 | 0.18 | no significant difference |
| Route 2, either run | 23/33 (53% to 83%) | 5/33 (6.7% to 31%) | 4 | 19 | 1 | 9 | 0.00004 | MegaLens better |

All bugs:

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 8/43 (9.7% to 33%) | 7/43 (8.1% to 30%) | 2 | 6 | 5 | 30 | 1 | no significant difference |
| Route 1, run 2 | 6/43 (6.6% to 27%) | 7/43 (8.1% to 30%) | 1 | 5 | 6 | 31 | 1 | no significant difference |
| Route 2, run 1 | 15/43 (22% to 50%) | 7/43 (8.1% to 30%) | 3 | 12 | 4 | 24 | 0.077 | no significant difference |
| Route 2, run 2 | 25/43 (43% to 72%) | 7/43 (8.1% to 30%) | 5 | 20 | 2 | 16 | 0.00012 | MegaLens better |
| Route 1, either run | 13/43 (19% to 45%) | 7/43 (8.1% to 30%) | 2 | 11 | 5 | 25 | 0.21 | no significant difference |
| Route 2, either run | 25/43 (43% to 72%) | 7/43 (8.1% to 30%) | 5 | 20 | 2 | 16 | 0.00012 | MegaLens better |

## 8. The service setting changed for the campaign (A9.4)

- All runs used one internal account.
- That account's per-account limit was raised to 150 review calls an hour and 500 a day, from 17 Nov 18:44:30 to 23:10:47 UTC.
- The defaults (60 and 100) were restored right after the last run.
- The service's settings file was then identical to the one in place before the raise, and the build did not change.

## 9. Disclosures and corrections

1. **Seen before A8** (A8.9): part of one reviewer's audit of route 1, run 1's verified findings, 37 decisions in total: 32 real, 3 not real and 2 unsure.
   - Those decisions were superseded under A9.3 and are not used here.
   - They are published, labels only, in `results/superseded-decisions.csv`.
   - No catch result was seen before A8.
2. **Correction to A9.1.** A9.1 says that no failure type other than quota refusals occurred before A9, "except 4 refusals in route 1, run 1 for input over the 500KB limit". That undercounted. Before A9, these counted failures had also occurred:
   - route 1, run 1: 34 failed attempts on 7 PRs for input over the service's size limits (500 KB of attached files, or 1 MB per request);
   - route 1, run 2: 15 such attempts on 3 PRs and 5 transport errors on 4 PRs;
   - route 2, run 1: 1 failed plugin delivery check.

   A9's rule already treated all of these as counted attempts, so the correction changes no result.
3. **Incomplete audits:** in 3 of the 5 audit sets (route 1 run 1, route 1 run 2, route 2 run 2), one audit reviewer stopped after 37 cases.
   - Those incomplete outputs were set aside unopened.
   - The same reviewer was run again with the same frozen brief. Only one sentence was added to the request: every case must get exactly one decision, and the count must be checked.
   - The new outputs cover every case. Coverage was checked by counting case identifiers only.
   - The incomplete outputs are listed by hash in `results/file-hashes.csv`.
4. **Route 2 delivery checks:** the harness checked whether the plugin sent exactly the expected bundle in a single review call.
   - Run 1: 92 of 100 final records passed. In 7 the bundle did not cover every changed file, and in 2 the host made more than one review call.
   - Run 2: 86 of 100 passed. In 7 the bundle did not cover every changed file, in 8 the host made more than one review call, and in 1 the bundle hash did not match.
   - A PR can fail more than one check.
   - These are completed reviews under the pre-registration, and they are scored as they are.
5. **Limits of the comparison:**
   - The CodeRabbit side is the real review each repository got when the PR was open, under that repository's own settings. MegaLens ran in November 2026 on the same diffs.
   - These bugs were later found and fixed in public. It cannot be ruled out that a model saw some of those fixes during training.
   - The head-to-head covers 43 PRs, with 31 to 33 catchable bugs.

## 10. Files

Published here, with every PR identified only by the sha256 of its claim-set ID:

| File | Contents |
|---|---|
| `results/summary.json` | The full tally, with every per-item outcome. The two reviewers are named reviewer_a and reviewer_b. |
| `results/per-item.csv` | One row per bug: its label, and for each run and for CodeRabbit the completion, the total attempts and both reviewers' catch decisions. |
| `results/attempts.csv` | Every attempt of every run, including quota-refused and failed ones: status, failure class, build, time and finding counts. |
| `results/verified-audit-decisions.csv` | Both reviewers' decision on every verified finding (W/V). |
| `results/precision-decisions.csv` | Both reviewers' decision on every posted finding in the precision pool, for both tools. |
| `results/superseded-decisions.csv` | Reviewer decisions made before A9, superseded and not used. |
| `results/original-protocol.json` | The A9.5 figures. |
| `results/file-hashes.csv` | The sha256 of every run record (every attempt) and every reviewer output, including the incomplete and superseded ones. |

**Not yet published: the review texts.** This covers MegaLens's findings, CodeRabbit's comments and the reviewers' short reasons.
- These texts quote file paths and code that identify the third-party repositories.
- Amendment A7 keeps those identities hashed until a separate decision allows naming them.
- Until then, every one of these files is fixed by its hash in `results/file-hashes.csv`.
- CodeRabbit's comment texts will only ever be referenced by hash.
