# MegaLens evidence

The records behind the numbers in MegaLens research and case studies, so you can check them
instead of taking our word for it.

> **What this repository does not contain.** It publishes results only. How MegaLens runs a
> review (which model does which job, in what order, with what settings and instructions) is our
> own work and is deliberately left out. Nothing left out is needed to check a number on our pages.

| Folder | Page |
|---|---|
| `research/ai-code-review-false-positives/` | https://megalens.ai/research/ai-code-review-false-positives |
| `research/llm-latency-timeouts-code-review/` | https://megalens.ai/research/llm-latency-timeouts-code-review |
| `blog/cursor-alternatives/` | https://megalens.ai/blog/cursor-alternatives |
| `blog/ai-code-review-tools/` | https://megalens.ai/ai-code-review-tools |

Each folder has a README that maps every number on its page to the file it comes from.

## How these files are made

- **Results, not internals.** Each file shows what came out of a review: the findings, which models
  answered, counts and times. It does not describe how the review works inside.
- **Model lists are sorted A to Z.**
- **Nothing belonging to someone else.** Where a study used other people's public projects, the
  projects are not named and their code and findings are not reproduced.
- **Fingerprints for what we cannot publish.** `*.sha256` files fingerprint the full internal records
  each folder was cut from. We can give those records to an independent auditor, who can confirm
  they match and have not changed since publication.
- **Linked by commit.** Pages link here by commit, so a file a page points to cannot change later.

Questions or a number you cannot reproduce: open an issue.
