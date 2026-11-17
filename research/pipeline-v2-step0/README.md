# Pipeline v2 step 0: "catchable" labels for the 30 development PRs (frozen 17 Nov 2026)

These labels are frozen before any score under them is computed. The labels themselves stay private, because they name third-party repositories. This file commits to them with hashes, so they cannot be changed later without the change showing.

- **Rules (private until the benchmark is published):** sha256 `5da513ff8a54085896e35e82181e466f7e8b5b3d2d68901914d112dfdaa66920`.
- **Frozen label file (private):** sha256 `1a201ddf03cefc3fb48801b10e23656a636af1fe91a147f6536e851bd66e5b69`.
- **Citation archive (private):** 21 archived pages, digest of their sorted sha256 values `994b844ef310f6bfc3ea421c450f5afcad867ec2fcb0dbdeaacf2216ddb58976`.
- **Method:**
  - Two blind labellers from different AI companies labelled every PR on the full repository at review time.
  - They agreed on 27 of 30 (Cohen's kappa 0.77).
  - A third model settled every disagreement without seeing any tool's results.
- **Result:** 22 of 30 bugs are catchable. 18 of them are catchable from the repository alone (the strict figure); 4 also need documented outside behaviour, with a cited official source for the pinned version.
- **Reporting rule:** every catch rate is reported twice, as a share of all 30 bugs and as a share of the catchable bugs, each as k/n with a 95% Wilson interval.
