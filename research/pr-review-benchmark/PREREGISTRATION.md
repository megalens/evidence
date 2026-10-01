# PR review benchmark: pre-registration

Committed on 2 October 2026, before the first benchmark run. The rules below do not change after run 1.

## What this is
MegaLens reviews the first version of 30 merged pull requests from public open-source projects that were also
reviewed by a third-party automated PR reviewer. Twenty PRs are the benchmark; ten are a reserve working set. The
ground truth is the set of real bugs in each first version that were later fixed. Some of them the third-party
reviewer flagged on its first review, and some it did not. Each bug is linked to the public comment and the fix commit.

Following this repository's rule, the projects are not named here and their code is not reproduced. The private records
are fingerprinted below so that an independent auditor can confirm they have not changed since this commit.

| Private record | sha256 |
|---|---|
| PR list and ground truth (`candidates.json`, built 29-30 Sep 2026) | `10857c09c37e237fcc41b978d4fe22600390d66f87ab6f0a26c1332f803a3947` |
| First-version code archive (sorted per-file sha256 list, hashed) | `b6ecf1832e45c448a9e2d96778fcd8acda7acc2600c65f8797b825329c6963f9` |
| Full private pre-registration (adds project names and exact file rules) | `0b65efaae124f927fae6543c1f16ea636e2ea38236c5e6f874c6710e1400ffb6` |

## Holdout
- The 20 benchmark PRs are blind. Nobody on our side scores them or reads their ground truth until an independent
  cross-check in November, and nothing in MegaLens is changed because of their results.
- The 10 reserve PRs are the working set; their ground truth may be read.
- Disclosure: while checking the structure of the data file on 2 Oct, the operator saw the ground-truth entries of one
  benchmark PR. No change was made because of it.

## What MegaLens is sent
- Live service, internal test account, `tier: standard`, `skill: code_intelligence`, the same mission text for every
  run:
  > Review this pull request before it is merged. Find real defects in the changed code: logic and correctness
  > errors, security problems, data loss, race conditions, missing error handling and crashes. pr.diff is the change
  > itself; the other files are the changed files as they are after the change.
- Input: the full first-version content of each changed source file (test files, lockfiles, locales, docs and CSS
  excluded) plus the whole first-version diff as `pr.diff`.
- PRs whose changed source exceeds 150,000 bytes get the diff only, and the second sentence then reads "the changed
  files are too large to send whole". This applies to 4 of the 20 benchmark PRs, including the two largest.
- Never sent: the PR discussion, any reviewer comment, the fix commits, or files the PR did not change.
- Input asymmetry: the third-party reviewer had access to the whole repository; MegaLens gets only the changed files
  and the diff. Results are reported with that stated.

## Runs
- Benchmark: 3 runs per PR on one pinned build. The build hash is recorded with every run.
- Reserve: 2 runs per PR without the PR's intent and 2 runs with one added line, `What this change is meant to do:
  <PR title>`. This is the first intent-audit dataset.
- Every run logs the PR, build hash, run id and charge.
- A run in which any part of the review was answered by a stand-in model, or in which a provider error was recorded,
  is marked and rerun once. Both runs are kept, and the headline uses the clean one.
- Runs that stop on a provider error are rerun once. Runs that stop for any other reason count as zero findings and
  are listed.
- A weekly run of the 20 benchmark PRs (1 run each, same settings, still blind) tracks how stable the results are
  week to week.

## Scoring (fixed now)
- **Unit:** one ground-truth bug.
- **Match:** a finding matches a bug when it points at the file the bug is in (by name or by a quoted line) and
  describes the same failure from the same cause. A different failure at the same place is not a match. One finding
  matches at most one bug.
- **Headline metric, validated catch:** the matching finding is one MegaLens itself reports as validated.
- **Second metric, raised catch:** the matching finding has any status, including unverified, "could not confirm"
  and disputed. Disputed and unconfirmed findings count only here.
- **Per PR:** median and range over the 3 runs.
- **Findings that match no ground-truth bug** are graded against criteria fixed now:
  - real defect: a concrete wrong behaviour shown by a code-path trace or a reproducing input;
  - not a defect;
  - style or opinion;
  - unverifiable: needs code that was not sent.
  The operator grades them, and the grades are cross-checked blind by a separate reviewer who reads each bug's public
  comment and fix without knowing which tool raised it.
- **Truncation:** if a run of a PR reports that part of the input was cut, or that a file sent was not seen, the PR is
  reported separately and kept out of the headline totals.
- **Third-party figures:** taken from its first review on each PR, as recorded on 29-30 Sep 2026.
