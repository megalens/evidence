# Fresh set: pre-registration hashes (18 Nov 2026)

A second sealed benchmark set: pull requests merged after the earlier set, so that models are unlikely to have seen the later fixes in their training data. Its labels were frozen, and these hashes published, before any MegaLens run on the set and before any CodeRabbit review on it was opened.

## How the set was built
1. **Sources:** public repositories with an OSI licence, reached only through GitHub's public API. We hold no account, trial or subscription with any code review vendor.
2. **Each pull request:**
   - was merged between 5 Aug and 29 Oct 2026;
   - received a pre-merge CodeRabbit review with inline comments, checked from the comment author and comment type only. No review text was read;
   - has a later fix, merged by 1 Nov 2026, that changes lines the pull request added or touched.
3. **Size:** 43 pull requests, at most one per repository.
   - No repository overlaps with the earlier 100-PR set or the 30-PR development set.
   - Languages: TypeScript 16, Python 9, Go 4, JavaScript 3, C# 2, Shell 2, and one each of Swift, Kotlin, Rust, C++, PowerShell, Dart and C.
4. **Bug descriptions:**
   - written from the pull request and its later fix;
   - every description checked by an independent reviewer against both patches;
   - 6 corrected;
   - 1 item found invalid (the bug existed before the pull request) and kept in the manifest, but excluded from scoring.

## Labels
- **Labellers:** two independent blind labellers from two different model families, using the same locked rules as the earlier set (rules sha256 below). Neither saw the fix or any review.
- **Agreement:** the labellers agreed fully on 27 of 43 items. The other 16 were settled by an adjudicator, under the locked rule that a still-disputed item is not catchable.
- **Result:** of the 42 valid items, 27 are catchable and 15 are not.
  - Catchable from the repository alone: 24.
  - Needing a pinned official document: 3, each with its citation checked.

| File | sha256 |
|---|---|
| Claim set manifest | faf5a506a4cb87b998dfadb7099fd15408d70633b302f25015b88c8dd9acd654 |
| Final labels | bb8a6ded86ef2572ee33c23b2eafb2005101e50ba467f982a548ce8dfb578d76 |
| Adjudication | fee13b0aeb479a223005a7cde7b75e3594ee71cd2f4d12a93b42904e677bc86b |
| Bug-description audit, including the corrections | 49312f2a7e30a17964342245453df1566ebed0f6ac94a18ecfaf3483ea8b3773 |
| Bug descriptions (files concatenated in sorted order) | d1afb899c737966e2009792ca6e02a64fb020e9f2c0330cf4d7f6ae1680c3819 |
| Labelling rules (same file as the earlier set) | 5da513ff8a54085896e35e82181e466f7e8b5b3d2d68901914d112dfdaa66920 |

## Protocol (registered now, before any run)
- **Arms:**
  - MegaLens through the coding-assistant plugin (route 2), two runs;
  - MegaLens through the MCP review tool (route 1), one run;
  - CodeRabbit's saved pre-merge reviews.
- **Build:** the MegaLens build and its review feature switches will be frozen and their hashes committed here before the first run.
- **Rules carried over from the earlier set:**
  - its pre-registration;
  - amendment A8, the same-items head-to-head: MegaLens and CodeRabbit are compared only on pull requests both reviewed, and only on the frozen catchable bugs there. A bug counts as caught only when two blind reviewers both say "hit";
  - the exact two-sided McNemar test (A6). "Better" is claimed only when p < 0.05;
  - precision of posted findings from one mixed blind pool;
  - false verified, for MegaLens only;
  - amendment A9: refusals by the account's call quota do not count as attempts.
- **Reporting:** every rate is shown as k/n with a 95% Wilson interval. Every run is published, not the best one.
- **Identities:** repository and pull request identities are withheld pending a separate decision.
  - Per-item results will use a keyed hash, so the identities cannot be recovered by guessing.
  - The list of pull requests is not published.
  - These hashes are not anonymisation; the identities are simply withheld.

## Disclosures
1. While the set was being built, two incidental exposures occurred. Neither was review text on a benchmarked pull request.
   - A fix commit message carried a one-line auto-generated release note for the fix.
   - One item's sealed note says its fix followed a review finding. It does not say what the finding was.
2. 14 of the 43 repositories use a copyleft licence (GPL or AGPL). The earlier set used only permissive licences.

## Run configuration (frozen 18 Nov 2026, before the first run)
| Item | Value |
|---|---|
| MegaLens build (git commit) | 80fe5c2b3dd79da72a199c21558f638d461213c2 |
| Review feature switches (SHA-256 of the sorted on/off list of the production server) | e01d04e8ecaee42890751fa7333ca88a38a7c6c2e5ebd5eaff19dcc3c4920d6e |

- **Where the runs happen:** a separate test server on the same machine as production, not the production server itself. It runs the same build as production.
- **Switch parity:** the test server has the same review feature switches as production. It differs only in five switches that do not affect a review: two for the GitHub app, which these runs do not use, and three that mark it as a test server (alerts silenced, test-only modes). It also allows more calls per hour and per day, so that a whole run fits in one go.
- **Large-input packing:** this build contains a new large-input packing feature. Its switch is off on production and off for these runs.

## Second look (registered 19 Nov 2026, before any second-look run)
The first-look results in RESULTS.md stand as published. A second look measures a newer build on the same set. It is a second look, not a replacement, and nothing in it was tuned on this set: every change was decided on the separate development sets.

| Item | Value |
|---|---|
| MegaLens build (git commit) | dee6ffdb894afd67a6aa5477bf5ec59a7448b113 |
| Review feature switches, production, at registration | 1f5f7cd9f5445124b37b9661d8c713b1fc128d351e228b3b2f6aec139f9879f0 |

- **Switch hash method (published from now on):** SHA-256 of the newline-joined, sorted list `NAME=on` / `NAME=off` over the build's registered review switch names, each read with the build's own switch reader from the production settings. The first-look hash above was computed before this method was written down; it is not recomputed or compared here.
- **What changed for users since the first look:** three display switches were turned on, one at a time, after checks on the development sets:
  1. a line number on each finding whose quoted code line is found exactly once in the sent code;
  2. each verified finding shows the explanation its check gave, labelled as not re-checked;
  3. the main list is sorted by strength, findings the check rejected move to a separate section, and near-duplicates are merged only when file, title and quoted lines agree.
  None of them changes how a finding is checked or what a review costs. The build also contains a code-retrieval feature that is off on production and off for these runs.
- **Arms and rules:** the same as the first look. Plugin route two runs, MCP route one run, all 43 items. The same two blind reviewers, the same frozen labels, the same tests (A6, A8, A9), every run published. CodeRabbit's arm is not re-run; its saved reviews are the same.
- **Scoring note:** scoring reads every finding in the answer, including the rejected section, exactly as the first look read findings the check had disputed. The results will also report how many catches sit in the main list.
- **Where the runs happen:** the same separate test server as the first look, on this build, with production's switches.

**Second look results:** [SECOND-LOOK.md](SECOND-LOOK.md), with counts in `results-second-look/`.
