# JupyterLab

JupyterLab is the main track of the GSoC 2027 preparation: small, testable, policy-clean starter issues in [jupyterlab/jupyterlab](https://github.com/jupyterlab/jupyterlab), worked from Kumar's fork [KUMAR-AKASH-M/jupyterlab](https://github.com/KUMAR-AKASH-M/jupyterlab). There is no CLA, and the repo welcomes AI-assisted contributions held to its highest quality bar. Since 2026-11-07 the work is laptop-first: Kumar's Antigravity agent on his laptop applies patches, runs Jest and lint, pushes, and records videos, while Doremon designs the fix and verifies it with harnesses. The handover rule that keeps that split safe is simple — confirm `git branch` and `git status` in a separate turn before any `git am`, push, or PR step, and never bundle a recovery with the next apply step.

Three of the six pull requests here are merged. The other three are open and waiting on maintainers, which is itself a lesson: a contribution is not finished when the code is right.

## Contributions

| Issue | PR | Status | Doc |
| --- | --- | --- | --- |
| #20373 — Windowing snapback | [#20387](https://github.com/jupyterlab/jupyterlab/pull/20387) | Merged 2026-11-06 | [20373-windowing-snapback.md](20373-windowing-snapback.md) |
| #20384 — Mermaid theme | [#20406](https://github.com/jupyterlab/jupyterlab/pull/20406) | Merged 2026-11-06 | [20384-mermaid-theme.md](20384-mermaid-theme.md) |
| #20363 — Console config | [#20431](https://github.com/jupyterlab/jupyterlab/pull/20431) | Merged 2026-11-11 | [20363-console-config.md](20363-console-config.md) |
| #19486 — Settings search | [#20414](https://github.com/jupyterlab/jupyterlab/pull/20414) | Open, ready for review | [19486-settings-search.md](19486-settings-search.md) |
| #20377 — Markdown search | [#20432](https://github.com/jupyterlab/jupyterlab/pull/20432) | Open, CI green | [20377-markdown-search.md](20377-markdown-search.md) |
| #18737 — yjs bump | [#20436](https://github.com/jupyterlab/jupyterlab/pull/20436) | Open, blocked upstream | [18737-yjs-bump.md](18737-yjs-bump.md) |
| #20367 — Shortcut settings | — | Closed, already fixed | [20367-already-fixed.md](20367-already-fixed.md) |
| #18830 — Terminal brackets | — | Closed, no PR — PowerShell 5.1 root cause | [18830-terminal-brackets.md](18830-terminal-brackets.md) |

## Lessons that apply to every JupyterLab PR

These came out of the work above, mostly from maintainer feedback, and they are worth reading before starting the next issue.

PR descriptions must be short and human-readable — flowing paragraphs, no forced line breaks, no verbosity, and never a "Verification status" section. CI reports tests and reviewers verify; saying so in the description only makes it harder to read. That was krassowski's feedback on 2026-11-09, and it applies to every future PR body from the first draft.

Write the test commit before the fix commit. If the fix is already pushed, add the test in a new commit and do not rebase during a review. A test skip needs a failing run behind it, not a line copied from the neighbouring test.

Do not open a PR for an issue labelled `status:Needs Triage` unless it was filed by a maintainer or by the person you are working with. When triage is unsettled, ask in a comment first.

Run prettier and the ESLint typecheck on every changed file before pushing, and rebuild compiled output (`jlpm build:test`) before running Jest — Jest loads the compiled `lib/`, not `src/`. Never state in the PR description that tests or lint passed. The AI usage section must name every tool and model used, plus the effort level.

Interaction changes need a video or GIF, attached in the web UI and never committed. A PR body must never reference a video or screenshot that does not yet exist, and a video must not be described as verified until its frames have actually been watched. Never claim a test passed that has not run.
