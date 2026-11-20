". Nothing here is committed.

ENTRY START

# Engine v2 on the fresh set (November 2026)

### What this is
A test of MegaLens Standard (engine v2) on real public pull requests, compared with CodeRabbit's saved public reviews on the same pull requests. We did not run CodeRabbit ourselves. We counted only what its saved reviews on those public pull requests already showed.

### The set
- 42 valid public pull requests, merged between 5 August 2026 and 29 October 2026.
- 27 known bugs that were merged even though CodeRabbit had reviewed the change.
- These are bugs that were merged despite a CodeRabbit review, so this is not a measure of everything CodeRabbit catches.

### Results
| Measure | Result |
|---|---|
| Known bugs found, MegaLens Standard, run 1 | 15 of 27 |
| Known bugs found, MegaLens Standard, run 2 (independent) | 14 of 27 |
| Known bugs flagged, CodeRabbit saved reviews | 4 of 27 |
| Exact McNemar test, run 1 vs CodeRabbit | p = 0.0034 |
| Exact McNemar test, run 2 vs CodeRabbit | p = 0.0063 |
| Sampled MegaLens Standard findings judged real | 43 of 60; 0 judged not real by both judges |
| Sampled CodeRabbit comments | 9 real, 12 not real, of 60 sampled |
| MegaLens Deep on the 11 bugs both Standard and CodeRabbit missed | 8 of 11 (descriptive only; that subset was chosen because of the misses) |

The other 39 of the 60 CodeRabbit comments were split between the two judges or marked unsure by at least one; only comments both judges called real (or both called not real) are counted. For MegaLens Standard the same split is 43 real, 0 not real, 17 split or unsure.
Wilson 95% intervals: Standard 15/27 = 37-72%, 14/27 = 34-69%; CodeRabbit 4/27 = 6-32%; precision Standard 43/60 = 59-82%, CodeRabbit 9/60 = 8-26%.

### How the findings were judged
Two AI judges, each from a company other than the one behind the reviewer being judged, marked a random sample of findings as real or not real. No model judged its own output.
Judged by two AI judges; a human check on JavaScript and TypeScript findings is in progress. That human check covers JavaScript and TypeScript only; findings in other languages have only the AI judges.

### Limits (please read)
- Small sample: 27 bugs.
- One set of pull requests.
- AI judges, not people, for everything except the JS/TS check above.
- Results vary from run to run, which is why two runs are shown.
- We built the set ourselves, so this is a second look at a set we know, not a blind test.
- These are bugs that were merged despite a CodeRabbit review, so this is not a measure of everything CodeRabbit catches. It says nothing about how often CodeRabbit is right on other changes.
- Use MegaLens alongside your current reviewer. This entry does not claim MegaLens is better.

### Pre-registration and files (sha256)
The plan was written down before the runs. Pre-registration: local commit ff2b1e6, with addendum 8c8e7b2.
| File | sha256 |
|---|---|
| Pre-registration (ff2b1e6) | sha256 ba0c2bc597f973586dfc79d2ff4343c6a6c7f8e023fa38a8b36aae1ed303e277 (PREREG.md at ff2b1e6) |
| Addendum (8c8e7b2) | sha256 cf0636acc6aada678d961d031cf9370eac800387d62e1785c2edb6c2f041c3e0 (PREREG.md at 8c8e7b2) |
| Labels file (which comments count as a found bug) | sha256 bb8a6ded86ef2572ee33c23b2eafb2005101e50ba467f982a548ce8dfb578d76 |
| Truth file (the 27 known bugs and their pull requests) | sha256 d1afb899c737966e2009792ca6e02a64fb020e9f2c0330cf4d7f6ae1680c3819 (concatenation of the 43 truth files in name order) |
| Results file, run 1 | sha256 e623cd44f9c448747aaa2bcb769a5c3a55987a70ecfa50d87b78fbba06b87770 |
| Results file, run 2 | sha256 edd31147b4f17a6e3f614f37bc9c441cac8f9dd7cf2fb2da591f270482804d35 |
| Judging file | sha256 de62f8598242fd37b1fe378cc5664a1ce020fa6a249305ac771584bb1b06c955 (concatenation: run 1, run 2, CodeRabbit recall scores, then precision judgments, both judges each) |

### Reproduce
The files above are fixed by their sha256 hashes. We publish counts and hashes only; CodeRabbit's comment text is never published. The set itself is described in README.md in this folder.
