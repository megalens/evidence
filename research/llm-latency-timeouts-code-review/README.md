# Evidence: "LLM latency and timeouts: what 363 code review runs showed"

Page: https://megalens.ai/research/llm-latency-timeouts-code-review

The test ran in September 2026 on the same review code customers use. The answer key is the 20-finding
sample from our earlier study (see `../ai-code-review-false-positives/`): 10 real bugs, 7 false alarms.

| On the page | Value | Where to check it |
|---|---|---|
| Full review runs in the main test | 363 | `main-test.csv`: 11 lineups × 11 codebases × 3 repetitions |
| Starting lineup | 9 of 10 known bugs found (7, 9 and 7 in the three repetitions) | `main-test.csv`, first row |
| Fastest lineup | 4 of 10, median review 82.0 s against 90.8 s | `main-test.csv`, the lineup with `gemini-3.5-flash-lite` |
| Every swap found fewer | 4 to 8 of 10 | `main-test.csv`, column `known_bugs_found_of_10` |
| Grok 4.6 timed out | 33 of 33, on the task needing the longest written answer | `timeouts.csv` |
| GLM 5.3 Flash timed out | 32 of 33 | `timeouts.csv` |
| DeepSeek V4 Flash timed out | 27 of 33 | `timeouts.csv` |
| DeepSeek V4.1 Flash | timed out in 12 of 33; reviews with it found 5 of 10 | `timeouts.csv`, `follow-up-tests.csv` |
| Speed table (seven models) | median time, answer length, tokens per second, both correlations | `speed.csv` |
| Answer-length correlation range | 0.59 to 0.99 | `speed.csv` |

A bug counted as found when at least 2 of the 3 repetitions found it.

## What is not here, and why

- **The codebases' names and code.** They are public projects whose owners did not ask to be
  reviewed; their names appear in every record's file name. `private-records.sha256` fingerprints
  all 711 records, so an independent auditor we give them to can confirm nothing has changed.
- **Which part of the review each swapped model took, and any cost.** Lineups are named only by the
  model that was swapped in. The same model was tried in two places once; those rows are marked
  test A and test B.

## Honest limits

Ten known bugs is a small answer key. Across its three repetitions the starting lineup found 7, 9 and
7, so a difference of one bug is within that variation.
