# #20373 — Notebook snapback in full windowing mode

**Status:** Merged — [PR #20387](https://github.com/jupyterlab/jupyterlab/pull/20387), merged 2026-11-06 by krassowski, merge commit `d8e2d7e`, 9 commits. Kumar's first merged JupyterLab PR.

## The issue

In full windowing mode, the notebook snapped back to a newly inserted cell whenever the user scrolled away from it. The scroll position was not really the user's to keep: the windowing list kept remembering the new cell as its target and pulling the view back to it on every resize.

## What was done

The root cause sat in `packages/ui-components/src/components/windowedlist.ts`. `scrollToItem` remembered its target for three seconds, `_scrollBackToItemOnResize` re-fired on every resize, and the old `onScroll` handler had no way to cancel the snapback once the user scrolled. The fix combined the maintainer's own approach — a separate block before the Firefox branch plus a clamped offset — with our guard on `event.type === 'scroll'`, in fifteen lines. A Galata regression test covers the behaviour. The PR carries the labels bug, pkg:ui-components, tag:Testing, and Still Needs Manual Backport, with milestone 4.6.x; Lumberbot flagged a backport conflict for 4.6.x, which sits with the maintainers.

## What we learned

- Read the repo's AGENTS.md and the TypeScript style guide it links before writing any code. They are short, and they answer most of the questions a reviewer would otherwise have to ask.
- A small, testable fix is the fastest way into a new codebase. Fifteen lines and one regression test were enough for a first merged PR.
- Maintainer collaboration improves the fix rather than competing with it. The merged version is a blend of the maintainer's block and our guard, and it is better than either alone.

## Links

- Issue: https://github.com/jupyterlab/jupyterlab/issues/20373
- PR: https://github.com/jupyterlab/jupyterlab/pull/20387
