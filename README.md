# Open Source Contribution Docs

This is a running record of what each contribution taught us, so the next one starts smarter. Every folder here is one upstream repository, and every document inside it is one issue we worked on — what the problem was, what we did about it, and what we learned along the way. The lessons are the point: the code lives in the pull requests, but the judgement that made the code better lives here.

| Repo | Contribution | PR | Status | Key lesson |
| --- | --- | --- | --- | --- |
| [JupyterLab](jupyterlab/) | #20373 — Windowing snapback | [#20387](https://github.com/jupyterlab/jupyterlab/pull/20387) | Merged | Read the repo's AGENTS.md and style guide before writing code. |
| [JupyterLab](jupyterlab/) | #20384 — Mermaid theme | [#20406](https://github.com/jupyterlab/jupyterlab/pull/20406) | Merged | Verify your mechanism against the real pipeline — the sanitizer strips `data-*` attributes. |
| [JupyterLab](jupyterlab/) | #20363 — Console config | [#20431](https://github.com/jupyterlab/jupyterlab/pull/20431) | Merged | A good triage comment is half the PR. |
| [JupyterLab](jupyterlab/) | #19486 — Settings search | [#20414](https://github.com/jupyterlab/jupyterlab/pull/20414) | Open | Repo-generated files are tracked — commit them; Jest loads compiled `lib/`, not `src/`. |
| [JupyterLab](jupyterlab/) | #20377 — Markdown search | [#20432](https://github.com/jupyterlab/jupyterlab/pull/20432) | Open | Check the neighbours — a fix for Find can break Replace. |
| [JupyterLab](jupyterlab/) | #18737 — yjs bump | [#20436](https://github.com/jupyterlab/jupyterlab/pull/20436) | Open, blocked upstream | An upstream blocker can park a green, approved PR. |
| [JupyterLab](jupyterlab/) | #20367 — Already fixed | — | Closed, no PR | Always check the bug is still present on current main before writing any code. |
| [JupyterLab](jupyterlab/) | #18830 — Terminal brackets | — | Closed, no PR | A guess on record is not proof — a control test is what settles where a bug lives. |
| [Jitsi Meet](jitsi-meet/) | #6873 — Lobby cancel | [#18189](https://github.com/jitsi/jitsi-meet/pull/18189) | Open | Be honest in the PR body about what was not verified. |
| [Jitsi Meet](jitsi-meet/) | #17772 — Investigation | — | Parked | Investigate before committing to a fix; a parked issue with notes is still progress. |
| [Humanize](humanize/) | #496 — intcomma | [#502](https://github.com/python-humanize/humanize/pull/502) | Closed, not merged | Check for duplicate or existing fixing PRs before building. |
| [SHAP](shap/) | #3066 — TreeSHAP sum | [#5275](https://github.com/shap/shap/pull/5275) | Merged | A one-line fix in the right place is a real contribution. |
| [fast-glob](fast-glob/) | #547 — stats drops nested files | [#550](https://github.com/mrmlnc/fast-glob/pull/550) | Open | The bug can live in a published dependency, not the repo itself. |
