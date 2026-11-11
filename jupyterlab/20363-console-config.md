# #20363 — Console setting change scrolls consoles and moves focus

**Status:** Merged — [PR #20431](https://github.com/jupyterlab/jupyterlab/pull/20431), merged 2026-11-11 by krassowski, merge commit `bd6f2521`, approved with a "Thank you!". Kumar's third merged PR.

## The issue

Changing a console setting scrolled every console to the top, moved the focus, and reordered the toolbars. The issue was filed by krassowski himself, and it was a plain bug: a setting change should not disturb the console you are working in.

## What was done

The fix touches three files. In `console/src/widget.ts`, `setConfig` now only calls `_updateLayout` when the prompt cell position actually changed or the panes have never been laid out, and `_updateLayout` only re-focuses the prompt if the focus was already inside that console. In `apputils/src/toolbar/factory.ts`, `transferSettings` returns early when the toolbar definition is unchanged. Two new tests cover the behaviour, and the laptop Jest run came back console 77/77 with apputils at 354 passed plus one pre-existing skip. A video of the fix was recorded, frame-verified, and attached to the PR.

## What we learned

- The fix proposed in the triage comment was exactly the fix that got built. A good triage comment is half the PR: describing the change precisely, before touching the code, makes the implementation straightforward and the review easy.
- When the maintainer files the issue, the scope is usually already settled. There is no design question to resolve first, which is why this track moved fast.

## Links

- Issue: https://github.com/jupyterlab/jupyterlab/issues/20363
- PR: https://github.com/jupyterlab/jupyterlab/pull/20431
