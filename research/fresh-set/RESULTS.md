# MegaLens and CodeRabbit on the fresh set: results

This page reports the second sealed set, registered in [README.md](README.md). The set has 43 pull requests. One was found invalid before any run (the bug existed before the pull request), so the scored results exclude that item. Of those 42, 27 have a bug that the frozen labels call catchable and 15 do not. We make MegaLens, so this is a benchmark of our own product.

## How this was run, in order
1. **Frozen before any MegaLens run, and before any CodeRabbit review on the set was opened:**
   The set and its labels, the bug descriptions, and the labelling rules.

   Their hashes are in [README.md](README.md).
2. **Frozen before the first run:** the MegaLens build (commit `80fe5c2b3dd79da72a199c21558f638d461213c2`) and the hash of its review feature switches. Both are in [README.md](README.md). The build did not change during the runs.
3. **Three MegaLens runs, each on every valid pull request.**
   1. Route 1 is the MCP review tool, one run.
   2. Route 2 is the coding assistant plugin, two runs.
   3. Each pull request got up to 5 counted attempts per run. Refusals by the account's call quota would not have counted as attempts (amendment [A9](../pipeline-v2-step6/AMENDMENT-A9.md)). There were none.
   4. There is no second route 1 run. The route 2 results are also shown as "either run": a bug counts if it was caught in at least one of the two runs, with no extra review.
   5. All three runs are published. None was picked as the better one.
4. **CodeRabbit is its saved reviews.** Each repository's real review from before the merge is used. It was not run again. All 42 valid pull requests have a real CodeRabbit review.
5. **Blind scoring.**
   1. Every output was anonymised. Two AI reviewers, blinded to the tool and run, compared each output with the bug description and gave "hit", "partial" or "miss".
   2. A bug counts as caught only when both reviewers say "hit". If they disagree, it is not a catch.
   3. Verified MegaLens findings were audited one by one. Every finding either tool posted went into one mixed anonymous pool and was judged real, not real or unsure.
6. **Tally.** The head to head follows amendment [A8](../pipeline-v2-step6/AMENDMENT-A8.md): both tools are compared only on pull requests both reviewed, and the paired test is an exact two sided McNemar test, following A6 as restated in [A8, section 4](../pipeline-v2-step6/AMENDMENT-A8.md#a84-what-the-head-to-head-reports). The result column says "MegaLens better" when the individual comparison has p below 0.05. These p values are not adjusted for multiple comparisons. The label refers only to the catch result for that row. Table rates are shown as k/n with a 95% Wilson interval.
7. **Identities.** Pull requests appear only as keyed hashes (the first 16 hex characters of an HMAC with a private key). This is not anonymisation. The identities are withheld, as registered.

Reviewer disagreements on whether a bug was hit, counted as not caught:

| Run | Disagreements |
|---|---|
| Route 1, run 1 | 2 |
| Route 2, run 1 | 0 |
| Route 2, run 2 | 1 |
| CodeRabbit | 1 |

## 1. Head to head on the same pull requests

The comparison uses only pull requests where MegaLens completed a review, so each row has its own count. Route 1 run 1 completed 40 of the 42, route 2 run 1 completed 41, and route 2 run 2 completed 42.

**Primary: catchable bugs**

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 10/26 (22% to 57%) | 4/26 (6.1% to 34%) | 2 | 8 | 2 | 14 | 0.109 | no significant difference |
| Route 2, run 1 | 11/26 (26% to 61%) | 4/26 (6.1% to 34%) | 3 | 8 | 1 | 14 | 0.039 | MegaLens better |
| Route 2, run 2 | 12/27 (28% to 63%) | 4/27 (5.9% to 32%) | 3 | 9 | 1 | 14 | 0.021 | MegaLens better |
| Route 2, either run | 12/27 (28% to 63%) | 4/27 (5.9% to 32%) | 3 | 9 | 1 | 14 | 0.021 | MegaLens better |

**Secondary: all bugs**, including those labelled not catchable

| MegaLens run | MegaLens (95% Wilson) | CodeRabbit (95% Wilson) | Both | Only MegaLens | Only CodeRabbit | Neither | Exact p | Result |
|---|---|---|---|---|---|---|---|---|
| Route 1, run 1 | 14/40 (22% to 50%) | 6/40 (7.1% to 29%) | 3 | 11 | 3 | 23 | 0.057 | no significant difference |
| Route 2, run 1 | 14/41 (22% to 49%) | 6/41 (6.9% to 28%) | 5 | 9 | 1 | 26 | 0.021 | MegaLens better |
| Route 2, run 2 | 16/42 (25% to 53%) | 6/42 (6.7% to 28%) | 5 | 11 | 1 | 25 | 0.006 | MegaLens better |
| Route 2, either run | 16/42 (25% to 53%) | 6/42 (6.7% to 28%) | 5 | 11 | 1 | 25 | 0.006 | MegaLens better |

The exact p values are 0.109375, 0.0390625, 0.021484375 and 0.021484375 for the primary table, and 0.057373046875, 0.021484375, 0.00634765625 and 0.00634765625 for the secondary table. The same values are in `results/summary.json`.

Points to read with these tables:
1. Route 1, run 1 is the only run where the result reads "no significant difference". The route 1 and route 2 runs used the same build, and the sample is too small to say whether the route matters.
2. Every bug that route 2 caught in run 1 was also caught in run 2, so "either run" equals run 2.
3. Neither tool caught 14 of the 26 catchable bugs in the matched route 2 run 1 comparison, or 14 of the 27 in the run 2 and either run comparisons.
4. **Sensitivity, all 42 valid pull requests, with a MegaLens non completion counted as a miss.** The paired p values and result labels do not change. Catchable bugs: route 1 run 1 10/27 against 4/27 (p = 0.109), route 2 run 1 11/27 against 4/27 (p = 0.039). All bugs: route 1 run 1 14/42 against 6/42 (p = 0.057), route 2 run 1 14/42 against 6/42 (p = 0.021). Route 2 run 2 is unchanged because it completed every pull request.

## 2. Both tools together against CodeRabbit alone

This table counts bugs caught by at least one tool on the same pull requests and compares that total with CodeRabbit alone.

| MegaLens run | Bugs | Both tools together (95% Wilson) | CodeRabbit alone (95% Wilson) |
|---|---|---|---|
| Route 1, run 1 | Catchable | 12/26 (29% to 65%) | 4/26 (6.1% to 34%) |
| Route 1, run 1 | All | 17/40 (29% to 58%) | 6/40 (7.1% to 29%) |
| Route 2, run 1 | Catchable | 12/26 (29% to 65%) | 4/26 (6.1% to 34%) |
| Route 2, run 1 | All | 15/41 (24% to 52%) | 6/41 (6.9% to 28%) |
| Route 2, run 2 | Catchable | 13/27 (31% to 66%) | 4/27 (5.9% to 32%) |
| Route 2, run 2 | All | 17/42 (27% to 56%) | 6/42 (6.7% to 28%) |
| Route 2, either run | Catchable | 13/27 (31% to 66%) | 4/27 (5.9% to 32%) |
| Route 2, either run | All | 17/42 (27% to 56%) | 6/42 (6.7% to 28%) |

These figures are descriptive. No significance claim is made for this comparison. The union includes every CodeRabbit catch, so its count cannot be lower than CodeRabbit's count on the same items. CodeRabbit added 1 bug to MegaLens's catches in each route 2 comparison on catchable bugs, and 2 in the route 1 comparison.

## 3. MegaLens on all 42 valid pull requests

This is separate from the head to head. A pull request that did not complete counts as a miss.

| Run | All bugs (n=42) | Catchable bugs (n=27) | Catchable from the repository alone (n=24) | Catchable, needing a pinned official document (n=3) |
|---|---|---|---|---|
| Route 1, run 1 | 14/42 (21% to 48%) | 10/27 (22% to 56%) | 10/24 (24% to 61%) | 0/3 (0% to 56%) |
| Route 2, run 1 | 14/42 (21% to 48%) | 11/27 (25% to 59%) | 11/24 (28% to 65%) | 0/3 (0% to 56%) |
| Route 2, run 2 | 16/42 (25% to 53%) | 12/27 (28% to 63%) | 12/24 (31% to 69%) | 0/3 (0% to 56%) |
| Route 2, either run | 16/42 (25% to 53%) | 12/27 (28% to 63%) | 12/24 (31% to 69%) | 0/3 (0% to 56%) |

The three bugs that need a pinned official document were not caught in any run. Three bugs are too few to say anything more.

## 4. Completion and attempts

| Run | Completed (95% Wilson) | All attempts | Counted attempts | Quota refused | Counted retries |
|---|---|---|---|---|---|
| Route 1, run 1 | 40/42 (84% to 99%) | 50 | 50 | 0 | 8 |
| Route 2, run 1 | 41/42 (88% to 100%) | 47 | 47 | 0 | 5 |
| Route 2, run 2 | 42/42 (92% to 100%) | 45 | 45 | 0 | 3 |
| Route 2, either run | 42/42 (92% to 100%) | 92 | 92 | 0 | 8 |
| CodeRabbit | 42/42 (92% to 100%) | not applicable | not applicable | not applicable | not applicable |

1. A retry is a counted attempt after the first one on that pull request in that run.
2. Route 1 run 1 did not complete 2 pull requests, and route 2 run 1 did not complete 1. Each of those three used all 5 counted attempts and ended as failed. In the tables above they are dropped from the head to head and counted as misses on all 42.
3. The attempt counts exclude the one invalid pull request, which was also run. Including it, the run files show 51, 48 and 46 attempts.
4. No call was refused for quota, so the amendment A9 change makes no difference to this set.

## 5. Precision of posted findings (descriptive only)

Every finding posted on the valid pull requests went into one anonymous pool of 743 findings (159 from CodeRabbit and 584 from MegaLens, counting each MegaLens run separately). The two reviewers judged each as real, not real or unsure. "Contested" means they gave different labels. The table shows the precision twice, once counting contested findings as not real and once counting them as real. No significance claim is made.

| Scope | Posted | Real (both) | Contested | Both unsure | Contested counted as not real (95% Wilson) | Contested counted as real (95% Wilson) |
|---|---|---|---|---|---|---|
| CodeRabbit | 159 | 25 | 20 | 0 | 25/159 (11% to 22%) | 45/159 (22% to 36%) |
| MegaLens, all 3 runs | 584 | 158 | 210 | 63 | 158/584 (24% to 31%) | 368/584 (59% to 67%) |
| Route 1, run 1 | 196 | 51 | 64 | 21 | 51/196 (20% to 33%) | 115/196 (52% to 65%) |
| Route 2, run 1 | 191 | 54 | 72 | 21 | 54/191 (22% to 35%) | 126/191 (59% to 72%) |
| Route 2, run 2 | 197 | 53 | 74 | 21 | 53/197 (21% to 33%) | 127/197 (58% to 71%) |

1. Counting contested findings as real changes the MegaLens numerator from 158 to 368 out of 584. These are descriptive estimates under two treatments of reviewer disagreement. They do not establish a single precision rate for either tool.
2. CodeRabbit is pooled once, on the pull requests of all included MegaLens runs. On each run's own matched set it is 23/152 and 42/152 (route 1 run 1), 25/156 and 44/156 (route 2 run 1), and 25/159 and 45/159 (route 2 run 2), for the two ways of counting.
3. Findings where both reviewers said "unsure" remain in the posted total used as the denominator for both estimates. They are listed separately and contribute to neither numerator.

## 6. False "verified" (MegaLens only)

MegaLens marks some findings as verified. W is the number of verified findings that both reviewers judged not real. V is the number of verified findings. CodeRabbit has no verified label, so this cannot be measured for it.

| Run | W/V (95% Wilson) | Contested | Both unsure | Wrong or contested, over V (95% Wilson) |
|---|---|---|---|---|
| Route 1, run 1 | 0/28 (0% to 12%) | 6 | 3 | 6/28 (10% to 40%) |
| Route 2, run 1 | 1/44 (0.4% to 12%) | 16 | 0 | 17/44 (26% to 53%) |
| Route 2, run 2 | 0/42 (0% to 8.4%) | 17 | 5 | 17/42 (27% to 56%) |

1. With contested findings not counted as wrong, the upper endpoints of the 95% Wilson intervals are approximately 12.1%, 11.8% and 8.4% for the three runs. Counting every contested finding as wrong raises those endpoints to approximately 40% to 56%. Findings both reviewers marked unsure remain in V and outside both wrong counts.
2. Under this calculation, an upper endpoint of 1% or less for the 95% Wilson interval requires at least 381 verified findings with none wrong, or 563 with one wrong. This set has 28 to 44 per run. No such claim is made.

## 7. Limits

1. **Small n.** The head to head has 26 or 27 catchable bugs and 40 to 42 bugs in all. The intervals are wide, and most of them overlap between the two tools. The paired test looks only at the bugs that one tool caught and the other did not.
2. **One repository per item.** Each pull request comes from a different repository, so one repository's habits cannot dominate. It also means this set says nothing about how either tool does across many pull requests in the same repository.
3. **Copyleft share.** 14 of the 43 repositories use a copyleft licence (GPL or AGPL). The earlier set used only permissive licences.
4. **CodeRabbit is its saved reviews.** The CodeRabbit side is the review each repository got when the pull request was open, under that repository's own settings. It was not run again. MegaLens ran in November 2026 on the same changes.
5. **Training data.** The bugs were later fixed in public. The set only uses pull requests merged after the earlier set, but we cannot rule out that a model saw some of the fixes.
6. **Identities withheld.** Repositories and pull requests are identified only by keyed hashes. Nobody outside can recheck an item yet. The review texts are not published and are not quoted here. CodeRabbit's comments will only ever be referenced by hash.
7. **Not unseen any more.** The results have now been looked at, so this set is no longer an unseen set for future runs. A later change to MegaLens tested on it would be tuned on data we have read.

## 8. Operational disclosures

1. **Reviewer credits ran out.** The blind reviewers' workspace ran out of credits during the campaign. Judging resumed later with the same two reviewers. No reviewer was swapped.
2. **Reviewers stopping early.** Each of the two reviewers stopped early once, because it read only part of a long case file. The two cases:
   1. for route 2, run 1, one audit of verified findings covered 37 of 44 cases;
   2. for the precision pool, the other reviewer's audit covered 108 of 743 cases.

   Each was run again with the same model and brief, plus one operator line stating the case count. The partial outputs are kept aside and were not used. They are listed by hash in [file manifest](results/file-hashes.csv).
3. **A different reviewer pair was started and stopped.** It was stopped before it wrote any output. Nothing from it is used.
4. **Published copies rename the reviewers.** In [item results](results/per-item.csv) and `results/summary.json`, the two reviewers are called reviewer_a and reviewer_b, and the operator's internal word for a reviewer in field names is replaced by "reviewer". Every number and label is unchanged, checked value by value against the operator originals. The one exception is the wording of three free text sentences in the `rules` field of the summary, where the operator's word for a reviewer became "reviewer". Both the operator originals and the published copies are in [file manifest](results/file-hashes.csv).

## 9. Files

| File | Contents |
|---|---|
| [item results](results/per-item.csv) | One row for each of the 42 valid items under its keyed id: the label, the catch result for each MegaLens run, the union of the two route 2 runs and CodeRabbit, completion and attempts for each MegaLens run, and both reviewers' hit, partial or miss decision for each MegaLens run. CodeRabbit has only its catch result there. The audit decisions on verified and posted findings are not in this file. |
| `results/summary.json` | The tally, including the paired counts used to calculate the tables on this page. |
| [file manifest](results/file-hashes.csv) | The sha256 of the two data files, of the operator originals of the tally, of this page, and of the two set aside outputs. |

The manifest does not contain its own hash.
