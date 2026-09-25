# Evidence: the counts in "AI coding mistakes that pass every test"

Page: https://megalens.ai/blog/ai-coding-mistakes-that-pass-tests

Every count on the page, with the date it was true and the record it comes from. The session was on
20 and 21 September 2026. The counts describe our own production records and code on those dates;
they will not match a query run today, because runs and code have moved on since.

| On the page | Count | True on | Where it comes from | Re-checked 25 Sep 2026 |
|---|---|---|---|---|
| Tests passed at the end of the session | 640 tests across 40 files | 21 Sep 2026, commit `38ab710` | One run of the full test suite on the code at that commit | Yes. Re-run on an exact copy of `38ab710`: `Test Files 40 passed (40)`, `Tests 640 passed (640)` |
| Mistake 1: runs said to lack a field | reported 0 of 131; 10 did carry it (121 of 131 did not) | 21 Sep 2026 | Our report of 21 Sep 2026 and the session record | Wording matched against both records |
| Mistake 2: records with the field vs records with content | 67 of 69 had the field; 18 of the 67 were empty; 49 usable | 21 Sep 2026 | Our report of 21 Sep 2026 (all 69 production records inspected) | Wording matched against the report |
| Mistake 3: runs that arrived without the mechanism | 75 of 94 | 14 Sep 2026, commit `2c4cc63` | A code comment written at that commit recording the measurement | Comment present at `2c4cc63` |
| Mistake 8: new fields silently left out of saved records | two | 20 Sep 2026, commits `1363f40` and `8d53f7a` | The two commits that added the fields, and a code comment added on 21 Sep (`b2a852b`) recording that it happened twice | Comment present at `b2a852b` |

**What is private, and how you can still check it.** The records above are in our private code
repository and our own notes. `sources.sha256` fingerprints a frozen copy of each one: the session
record, the 21 September report, the saved test run output, the two code files at the named commits,
and the list of files the two commits changed. We can give those copies to an independent auditor, who
can confirm they match these fingerprints and say whether the counts on the page follow from them.

## What is not here, and why

- **How MegaLens runs a review.** Which model does which job, in what order, with what settings and
  instructions is our own work and is deliberately left out. The code files and reports behind these
  counts describe that work in places, which is why they are fingerprinted rather than published.
  Nothing left out is needed to check a count on the page.
