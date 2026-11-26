# Fresh set: blind human check of sampled findings (25 Nov 2026; follow-up 26 Nov 2026)

A developer checked a sample of findings from this benchmark without being told which reviewer wrote each one. This file gives the counts. It does not publish repository names, pull request numbers, finding text or the developer's answers. Those records are hashed below.

## What was checked
- **33 items, all JavaScript or TypeScript**, from 15 pull requests in the fresh set:
  - 13 findings from MegaLens Standard (first run);
  - 5 findings from MegaLens Deep;
  - 10 comments from CodeRabbit's saved public reviews;
  - 5 calibration items with known answers: 3 real bugs (taken from the later fixes) and 2 invented findings that are false by construction.
- **How the sample was drawn:** fixed in advance with a seeded script (hash below). It took every CodeRabbit catch, the other catches in proportion, findings both AI judges had called real, and comments that at least one AI judge had called not real. Because of this design, the per-reviewer shares below are not comparable with each other or with the 60-finding precision samples on the article pages.
- **Questions per item:** (1) Is this a real problem in the pull request code? (2) For catches: does it describe the known bug (hit or partial)? (3) If real, how serious is it?

## Results
| Check | Count |
|---|---|
| Calibration items answered correctly | 5 of 5 |
| Findings both AI judges called real: the developer also called real | 24 of 24 decided (95% Wilson range, 86% to 100%); 1 more was marked "can't tell" because the code it depends on was not in the diff shown |
| Finding both AI judges called not real: the developer also called not real | 1 of 1 |
| Findings the two AI judges split on (one real, one not real) | 2; the developer called both not real. These were never counted as real in the published precision numbers, which require both judges to agree |
| Benchmark catches (both AI judges said "hit"): developer said hit | 13 of 15 (95% Wilson range, 62% to 96%) after the follow-up below; 10 of 15 in the first pass |
| Same catches: developer said partial | 2 of 15 after the follow-up; 5 of 15 in the first pass |

**The 5 partial answers in the first pass.** In 4 of the 5, the AI judges credited the review for two separate findings that together describe the known bug. The developer was shown only one of those findings, and each of the developer's reasons names the part that was in the other finding. In the fifth, the judges credited a single finding, the same one the developer saw; the developer judged it to cover only part of the bug. That case is a real difference of view. The benchmark rule counts a review as a whole ("a bug counted as caught only when both judges marked it a hit"), so all 5 stay catches under the published rule. The published catch counts remain AI-judged.

## Follow-up: the 4 credited pairs (26 Nov 2026)
The same developer was then shown both findings of each of the 4 pairs the judges had credited together, with the code and the diff, and asked: taken together, do these two findings describe the known bug (hit, partial or miss)? 37 minutes in total.

| Pair | Developer |
|---|---|
| CodeRabbit saved review (1 pair) | hit |
| MegaLens Standard (3 pairs) | 2 hit, 1 partial |

The judges credited 4 pairs and the developer agreed on 3 of them. In the remaining pair, the developer said the two findings together still cover only part of the known bug. With the single-finding case above, 2 of the 15 sampled catches stay partial in the developer's view: both are MegaLens Standard catches. By reviewer, the developer now calls the sampled catches a full hit for CodeRabbit saved reviews 4 of 4, MegaLens Standard 7 of 9, MegaLens Deep 2 of 2. The published catch counts remain AI-judged.

**By reviewer** (sample design above applies):
| Reviewer | Items | Developer: real | Not real | Can't tell |
|---|---|---|---|---|
| MegaLens Standard | 13 | 13 | 0 | 0 |
| MegaLens Deep | 5 | 5 | 0 | 0 |
| CodeRabbit saved reviews | 10 | 6 | 3 | 1 |

## What this means for the published numbers
- Precision (MegaLens Standard 43 of 60 sampled findings real by both judges; CodeRabbit 9 of 60): every both-judges-real label the developer could decide held up. On the split cases the developer did not call them real, so the published counts are not inflated by the judges.
- Known-bug catches (MegaLens Standard 15 and 14 of 27; CodeRabbit 4 of 27): unchanged and still AI-judged. The sampled catches all point at the known bug; after the follow-up the developer says 13 of 15 describe it fully and 2 of 15 only in part.

## Hashes
| File | sha256 |
|---|---|
| Sampling script | 44b22f01c5a9b8c633951068dd22c26f95aee5a2792fe42dac456fa5c9ae9508 |
| Blind pack given to the developer | 2326ac9574ba0473e0e98ccb343b5ed0bc248ce2f4701235d2769d8081edc7d9 |
| Private key (which reviewer wrote each item) | 64ddc0750d48fb375edce6ec3e1f324415e3ce66f1a09b8267f84e003aae127a |
| Developer's answers (CSV export) | 5ac0924c213c9ef04e158e075ff9afedbcaba99796dde7f3707d34478730dec6 |
| Follow-up pack builder script (26 Nov) | 208b2c3ee77da27967311c37bcfe7031f6f63a5288cc498bb6289f0f06e371a9 |
| Follow-up pack given to the developer (26 Nov) | 79fe6b01399b937cc9d7bacb1f4d5c593122753cdf9494c14dbaf8bf11e9adaa |
| Follow-up private key (which pair is which) | 54f1ccbe522345f0a42f6cdef0f681a424f3003cbca151037e0c82a271925740 |
| Follow-up answers (CSV export, 26 Nov) | cf3144760647007db40337d76b56aba2e6f104caca3c92fdf7ef033f7fb6b577 |
