# Step 6 pre-registration, amendment A9: refusals by the benchmark account's call quota (17 Nov 2026)

Added to the frozen step 6 pre-registration and amendment A8. It was written and published before any re-run started and before any result was opened (no catch result, head-to-head count, tally output or reviewer decision). It was decided by three independent reviewers of the plan (unanimous on A9.4 and A9.6, 2-1 on A9.2 and A9.3). Unlike A1 to A8, it was written after an operational failure was observed. The original protocol's outcome is therefore reported next to the amended one (A9.5).

## A9.1 What happened
- All runs use one internal benchmark account on the live service, on the frozen build.
- The live service has a per-account runaway-loop guard for paid accounts: 60 review calls an hour and 100 a day. It refuses a call at admission, before any review runs and without any charge, with the error code `rate_limited`.
- The run harness retried refused calls within seconds, so affected PRs used all 5 attempts on refusals:
  - route 1, run 1: 39 of 100 PRs;
  - route 1, run 2: 71 PRs, with none completed;
  - route 2, run 1: 25 of the 59 PRs attempted.
- Route 2, run 2 had not started. Both routes were stopped at about 18:35 UTC.
- No other failure type occurred, except 4 refusals in route 1, run 1 for input over the 500KB limit.

## A9.2 Rule
- An attempt refused at admission by the benchmark account's call quota does not count against the 5-attempt budget of section 3. Each attempt is classified from its own record:
  - error code `rate_limited`, or one of the quota messages;
  - and no review run was created.
- Every other failure counts as before, including the 4 refusals over 500KB, and is not re-run beyond its budget.
- A PR-run that completed is never re-run.
- Each affected PR-run is resumed with its remaining counted attempts. New attempts get new numbers, and every earlier attempt file is kept.
- The harness now waits on a quota refusal:
  - 60 seconds for the per-minute limit;
  - 5 minutes for the hourly limit;
  - a daily refusal, or 3 refusals in a row on one PR, halts the arm under section 3, with no further amendment.

## A9.3 Reviewer decisions made before this amendment
- Recall and audit decisions had been produced, unopened, for route 1 run 1 and for the CodeRabbit side, from packets prepared before A8 and before any re-run.
- They are superseded. They are archived unopened and published as superseded.
- All five arms (four MegaLens runs and CodeRabbit) are prepared fresh, after every run is complete, and assessed by the same two blind reviewers with the frozen prompts.

## A9.4 Service setting for the campaign
- For the rest of the campaign, the paid per-account limits on the live service are raised to 150 an hour and 500 a day. The per-minute limit (10) is unchanged.
- This is a restart of the same frozen build with the same frozen feature switches, verified before and after the restart. These limits are not in the frozen feature-switch set, and no review behaviour depends on them.
- The setting applies to all paid accounts while it is in force.
- Concurrency is unchanged.
- The defaults are restored as soon as the last run completes, and no later than 18 Nov 2026 23:59 UTC.
- Both changes are logged with UTC times.

## A9.5 Reporting
- Refused attempts are published and counted in their own row per arm.
- Results are reported under A8 as amended here.
- They are also reported under the original protocol: a PR-run whose first 5 attempts all failed, including quota refusals, counts as a miss. This row is labelled "original protocol" so the effect of A9 is visible.

## A9.6 Timing
- Nothing is opened until all four runs are complete and all five arms are assessed.
- A status note with no results is published at 05:00 UTC on 18 Nov.
- The hard stop is 18 Nov 2026 23:59 UTC. An arm still incomplete then is reported under section 3 and A8.5.
