# #18737 — Bump yjs to 13.6.33

**Status:** Open — [PR #20436](https://github.com/jupyterlab/jupyterlab/pull/20436), head `a9be9ee8fa`, 4 commits, title "Bump yjs to 13.6.33", labels bug and pkg:cells, milestone 4.7.0. CI is fully green (137 runs, zero failures). Blocked on an upstream ydoc fix; nothing for Kumar to do.

## The issue

A garbage-collection bug in yjs crashes `applyUpdate` with an "Unexpected case" error. The fix is a dependency bump: yjs 13.6.30 or later. Alok, who had claimed the issue in July 2026, released it to Kumar in a comment on 2026-11-09, so the issue was unambiguously his. There was a trap to avoid first: a prior PR, #18953, had been closed unmerged because its base was `main` and it let lib0 float to 0.2.117, which krassowski had flagged as problematic.

## What was done

The PR bumps yjs to 13.6.33 and drops the lib0 pin, since main now uses TypeScript ~5.9.3 and the pin's rationale is gone. It also carries a cast in `packages/cells/src/model.ts`, because the bump surfaced a latent type error: `@jupyter/ydoc` types `outputsChange.insert` as a single `Y.Map`, but at runtime the delta carries an array of output maps. The Read the Docs build caught it — Jest does not typecheck, so the test suite had passed happily while the docs build failed. krassowski reviewed with three inline comments: drop the lib0 pin, bump to 13.6.33 because the 13.6.29 regression is only fixed there (yjs#768), and asked whether the cast is a ydoc bug. It is, and he filed [jupyter_ydoc#459](https://github.com/jupyter-server/jupyter_ydoc/issues/459) himself. Kumar addressed the review in two new commits — the package.json change and a lockfile regeneration — and replied to all three threads. krassowski then said "Thank you!" on 2026-11-11 at 19:33 IST, but dismissed that review at 20:40 IST and commented that the PR is blocked on [jupyter_ydoc#428](https://github.com/jupyter-server/jupyter_ydoc/pull/428) ("Avoid Yjs warnings during cell insertion", by MUFFANUJ, open since 2026-07-16): with the bump applied, JupyterLab generates enormous amounts of yjs warnings, so the bump waits until ydoc is fixed.

## What we learned

- Check for a prior closed PR before starting. The #18953 trap — wrong base, floating lib0 — was visible in the history, and building on it would have repeated the same closure.
- A dependency bump can surface a latent type error that tests cannot see. The Read the Docs build is a real check; run it, or at least run a typecheck, after a bump.
- The cast must not be "simplified" back. A repro proved the simpler form does not compile, and the comment on the cast exists to stop the next person from trying.
- An upstream blocker can park a green, approved PR. The code is right, the CI is green, and the merge still waits — that is normal, and there is nothing to do but wait.

## Links

- Issue: https://github.com/jupyterlab/jupyterlab/issues/18737
- PR: https://github.com/jupyterlab/jupyterlab/pull/20436
- Upstream blocker: https://github.com/jupyter-server/jupyter_ydoc/pull/428
- Upstream typing issue: https://github.com/jupyter-server/jupyter_ydoc/issues/459
