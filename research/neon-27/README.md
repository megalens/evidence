# Gemini 4 Neon on the 27 catchable fresh-set bugs (22–23 Nov 2026)

This folder reports a single-run evaluation of **Gemini 4 Neon** (`gemini-4-neon-high`) as a standalone code reviewer on the **27 catchable bugs** from the sealed fresh set registered in [../fresh-set/README.md](../fresh-set/README.md).

It sits beside the MegaLens Standard (engine v2) and CodeRabbit figures published in [../fresh-set/ENGINE-V2.md](../fresh-set/ENGINE-V2.md). We make MegaLens; this is our own test run and is not a pre-registered benchmark.

## What was tested
- **Model:** Gemini 4 Neon (`gemini-4-neon-high`, `--effort high`).
- **Scope:** All **27 catchable bugs** from the 42 valid pull requests in the fresh set (24 labelled catchable from the repository alone; 3 labelled catchable needing a pinned official document). Pull requests were merged between 5 Aug and 29 Oct 2026, each from a distinct public repository with an OSI licence.
- **Not run:** The 15 valid pull requests labelled not catchable, and the 1 invalid pull request excluded at registration, were not run.
- **Reference baselines on the same 27 bugs** (from [../fresh-set/ENGINE-V2.md](../fresh-set/ENGINE-V2.md), not re-run here):
  - MegaLens Standard (engine v2), run 1 and run 2 (two independent runs).
  - CodeRabbit's saved pre-merge reviews on the same pull requests.

## How it was run
1. **Runner:** Antigravity CLI (`agy` v1.5.15), run non-interactively in a fresh disposable copy of each PR-head repository checkout:
   ```text
   agy -p <(prompt not published: it is part of how MegaLens reviews)> --model gemini-4-neon-high --effort high --dangerously-skip-permissions --output-format json
   ```
2. **Review prompt:** Verbatim text in [(prompt not published: it is part of how MegaLens reviews)]((prompt not published: it is part of how MegaLens reviews)) (`sha256: ad8db7d16f74a167fb3d4efcb4f7357696089841f794627e51989554c06810a2`). The prompt instructs a read-only review over the current repository changes and requires structured JSON output.
3. **Read-only check:** Before and after each run, `git status --porcelain` and `git diff HEAD` were hashed. Every run left the checkout unchanged (`checkout_changed = false` on all 27 items), and each temporary copy was deleted after the run.
4. **One run per pull request:** Gemini 4 Neon was run once per item, with up to 4 retries spaced 2 minutes apart on HTTP 429 (`RESOURCE_EXHAUSTED`).
5. **Dates and batching (22–23 Nov 2026):**
   - Full-27 completion and recall judging took place on **22 Nov 2026**; precision judging took place on **23 Nov 2026**.
   - Reviews were collected in two batches: an initial 13-item batch on 21 Nov 2026 (concurrency 2) and the remaining 14 items on 22 Nov 2026 (concurrency 1). The single failed item occurred in the concurrency-2 batch and failed again on a concurrency-1 retry (8 attempts total, all returning HTTP 429 `RESOURCE_EXHAUSTED`); the 14 items run at concurrency 1 had 0 failures and 0 retries.
6. **Blind recall scoring (22 Nov 2026):**
   - **Judges:** `gpt-6.2-sol` and `gpt-6.2-astra` (both OpenAI models — neither from Google, so no model judged its own company's output).
   - **Rule:** Same frozen bug descriptions and catch rule as [../fresh-set/RESULTS.md](../fresh-set/RESULTS.md). Each judge independently gave `hit`, `partial`, or `miss` on anonymised packets. A bug counts as caught only when both judges say `hit`. An incomplete/failed review counts as a miss.
7. **Blind precision sample (23 Nov 2026):**
   - Across the 26 completed reviews, Gemini 4 Neon produced **76 findings** (mean 2.9 per completed PR; 3 completed PRs returned 0 findings).
   - **60 of the 76 findings** were sampled (fixed seed `20261120`) and placed into one shuffled blind pool of 120 findings alongside 60 sampled CodeRabbit comments from the same 42-PR fresh set.
   - The same two judges (`gpt-6.2-sol` and `gpt-6.2-astra`) rated each finding `real`, `not_real`, or `unsure` from the PR diff alone.
8. **Identities:** Pull requests appear only as the same 16-hex-character keyed hashes published in [../fresh-set/results/per-item.csv](../fresh-set/results/per-item.csv). No repository names, source code, or model outputs quoting third-party code are published.

## 1. Recall on the 27 catchable bugs

All rates are shown as `k/n` with a 95% Wilson interval. The 1 failed Gemini 4 Neon review counts as a miss in the all-27 row.

| System | Catchable bugs (n=27) | 95% Wilson | Repo-only (n=24) | Needs official doc (n=3) | Judge pair |
|---|---:|---|---:|---:|---|
| Gemini 4 Neon (1 run) | 16/27 | 41% to 75% | 14/24 (39% to 76%) | 2/3 (21% to 94%) | `gpt-6.2-sol` + `gpt-6.2-astra` |
| MegaLens Standard, run 1 | 15/27 | 37% to 72% | — | — | Published baseline ([ENGINE-V2.md](../fresh-set/ENGINE-V2.md)) |
| MegaLens Standard, run 2 | 14/27 | 34% to 69% | — | — | Published baseline ([ENGINE-V2.md](../fresh-set/ENGINE-V2.md)) |
| CodeRabbit (saved reviews) | 4/27 | 5.9% to 32% | — | — | Published baseline ([ENGINE-V2.md](../fresh-set/ENGINE-V2.md)) |

Points to read with this table:
- **Completed reviews only:** Gemini 4 Neon completed **26/27 (82% to 99%)** reviews. On those 26 completed reviews, it caught **16/26 (43% to 78%)**.
- **Judge agreement:** **0 disagreements** out of 27 (`gpt-6.2-sol` and `gpt-6.2-astra` gave the identical label on every item: 16 `hit`, 3 `partial`, 7 `miss`, plus 1 failed review counted as `miss`).
- **Runtime:** Across the 26 completed reviews, median wall time was **6.5 minutes** (range 2.4 to 12.2 minutes).
- **Overlap with MegaLens Standard (descriptive only):**
  - Gemini 4 Neon caught **3 bugs** that MegaLens Standard missed in both runs.
  - MegaLens Standard caught **2 bugs** that Gemini 4 Neon missed (1 in both runs, 1 in run 1 only).
  - Union of Gemini 4 Neon and MegaLens Standard run 1: **18/27 (48% to 81%)**.
  - Union of Gemini 4 Neon and MegaLens Standard run 2: **17/27 (44% to 78%)**.
- **Paired comparisons (descriptive, exact two-sided McNemar on n=27, cross-judge-pair caveat applies):**
  - Gemini 4 Neon vs MegaLens Standard run 1: both 13, only Neon 3, only Standard 2, neither 9 (`p = 1.00`, no significant difference).
  - Gemini 4 Neon vs MegaLens Standard run 2: both 13, only Neon 3, only Standard 1, neither 10 (`p = 0.625`, no significant difference).
  - Gemini 4 Neon vs CodeRabbit: both 3, only Neon 13, only CodeRabbit 1, neither 10 (`p = 0.0018`; same on the 26 completed PRs).

Per-bug outcomes are in [per-bug.csv](per-bug.csv).

## 2. Precision on 60 sampled findings (descriptive only)

Both arms below were judged in the same 120-finding shuffled blind pool by `gpt-6.2-sol` and `gpt-6.2-astra` on 23 Nov 2026.

| Arm (same blind pool, same judges) | Sampled | Both `real` (95% Wilson) | Both `not_real` (95% Wilson) | Both `unsure` | Split |
|---|---:|---|---|---:|---:|
| Gemini 4 Neon | 60 | 37/60 (49% to 73%) | 0/60 (0% to 6.0%) | 18 | 5 |
| CodeRabbit (control in same pool) | 60 | 13/60 (13% to 34%) | 37/60 (49% to 73%) | 5 | 5 |

- For Gemini 4 Neon, all 5 splits were `real` vs `unsure` (3 `sol=unsure, astra=real`; 2 `sol=real, astra=unsure`); neither judge marked any sampled Neon finding `not_real`. Counting splits as real gives **42/60 (58% to 80%)**.
- For comparison, in [../fresh-set/ENGINE-V2.md](../fresh-set/ENGINE-V2.md) (scored by a different judge pair), MegaLens Standard scored 43/60 (59% to 82%) both real with 0 both not real, and CodeRabbit scored 9/60 (8% to 26%) both real with 12 both not real. Because the same CodeRabbit comments moved from 9/60 to 13/60 across judge pairs, cross-pair precision figures should not be compared directly.

The 60 sampled Gemini 4 Neon finding labels are in [precision.csv](precision.csv).

## 3. Limits (please read)
1. **Small sample (n = 27).** Confidence intervals are wide (41% to 75% for Gemini 4 Neon vs 37% to 72% and 34% to 69% for MegaLens Standard). This sample cannot distinguish small differences in recall.
2. **Single run.** Gemini 4 Neon was run once per pull request. Run-to-run variance is not measured here.
3. **Different judge pair from published baselines.** Gemini 4 Neon recall and precision were scored by `gpt-6.2-sol` + `gpt-6.2-astra`, whereas the MegaLens Standard and CodeRabbit figures in [../fresh-set/ENGINE-V2.md](../fresh-set/ENGINE-V2.md) were scored by a different two-company judge pair. Cross-pair comparisons are confounded by judge differences.
4. **Same-company judges.** Both `gpt-6.2-sol` and `gpt-6.2-astra` are OpenAI models. They are independent of Google (Gemini 4 Neon), but not independent of each other's model family.
5. **Not pre-registered and not unseen.** This run happened after the fresh set results had already been opened and published. Only the 27 catchable bugs were run, and the run was expanded from an initial 13-item batch (21 Nov) to all 27 items (22 Nov).
6. **One failed review (HTTP 429).** One item failed with `RESOURCE_EXHAUSTED` across 8 attempts and is counted as a miss; reliability under concurrent load via Antigravity CLI is a practical limitation observed in this run.
7. **Diff-only AI precision audit.** The 60-finding precision sample was judged by AI models reading only the PR diff (without the surrounding repository), which left 18/60 findings marked `unsure` by both judges. No human reviewed the Neon precision sample.
8. **CodeRabbit selection bias and training data.** As disclosed in [../fresh-set/RESULTS.md](../fresh-set/RESULTS.md) and [../fresh-set/ENGINE-V2.md](../fresh-set/ENGINE-V2.md), these bugs were merged despite a CodeRabbit review (so this is not a measure of everything CodeRabbit catches) and were later fixed in public (so training-data exposure cannot be ruled out).
9. **Identities withheld.** Items are keyed hashes matching [../fresh-set/results/per-item.csv](../fresh-set/results/per-item.csv); repository names, code, and finding text are not published.

## 4. Files and fingerprints

| File | Contents | sha256 |
|---|---|---|
| [per-bug.csv](per-bug.csv) | 27 rows, sorted by keyed `item_id` (matching `../fresh-set/results/per-item.csv`): Neon hit/miss, completion flag, both judges' recall labels, MegaLens Standard run 1 and run 2, and CodeRabbit. | `9cf2beedcfce36f3a22627fcd8c2744f59cce10f494e314e9f5742d88619095c` |
| [precision.csv](precision.csv) | 60 rows, one per sampled Gemini 4 Neon finding from the 120-finding blind pool: blind pool `finding_id`, `sol_label` (`gpt-6.2-sol`), and `astra_label` (`gpt-6.2-astra`). | `16796dc63e3d24b7e28590c7f4a242447a0717e1b7cd0e582c7d505ff3cf9742` |
| [(prompt not published: it is part of how MegaLens reviews)]((prompt not published: it is part of how MegaLens reviews)) | Exact review prompt passed to `agy -p`. | `ad8db7d16f74a167fb3d4efcb4f7357696089841f794627e51989554c06810a2` |

Private operator records fingerprinted for auditability (not published here because they contain repository names or finding text):

| Private record | sha256 |
|---|---|
| Full-27 recall tally (`TALLY-NEON.json`) | `46308825c07090c92ef5f5537f59874de4aa67780cb761f896a9b70890523978` |
| Precision tally (`PRECISION.json`) | `f5db0be9279611feb0c33215218e69559434f2f7477d7dc38f6d31931746ad69` |
