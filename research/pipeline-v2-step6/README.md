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

Still to be hashed before any MegaLens run on the claim set: the frozen labels and the frozen build and settings.
