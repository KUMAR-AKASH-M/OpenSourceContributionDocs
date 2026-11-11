# #20377 — Marking the current search match in rendered Markdown cells

**Status:** Open — [PR #20432](https://github.com/jupyterlab/jupyterlab/pull/20432), head `dd005b32`, 10 commits, CI green (110 runs, zero failures). No formal review yet; Kumar chose to wait for krassowski's review.

## The issue

When searching inside a rendered Markdown cell, the current match was never marked in the rendered text. The search provider un-rendered the cell whenever there were matches and stepped through the hidden editor instead, so the rendered view — the one the user was actually looking at — showed nothing. The issue was filed by krassowski, and the design was settled as his Option 2, which tonyfast agreed with.

## What was done

The fix lives in `packages/cells/src/searchprovider.ts` only. It steps the editor first, so match counting stays source-based, then marks that match in the rendered text through the rendered provider when the rendered text shows it, and un-renders only for a match the rendered text does not show. The harness ran 29/29 with the fix against 18 passed and 11 failed without it. The first CI round exposed a snag: Replace stayed enabled on rendered matches, so a fix for that went in too — and krassowski then found a real regression in it. Finding "apple" and replacing with "pear" corrupted the heading "Shopping" into "pearpping", because `replaceCurrentMatch` used the rendered match's position, an offset inside a single rendered text node, as if it were a source offset. Kumar let krassowski push the fix himself; his two commits landed, CI went green, and the PR is waiting on his review.

## What we learned

- Check the neighbours. A fix for Find can break Replace, and the regression only showed up because a maintainer actually tried the neighbouring feature.
- Letting the maintainer push the fix is sometimes the right call. He owned the regression, he fixed it in two commits with tests, and the PR came out stronger.
- A Galata PASSED can be a stale build. Re-run after the dev server recompiles, or the result means nothing.
- The Codespace needs its fonts fixed by hand: libfreetype6 2.11.1 (2.8.1 is too old), `FONTCONFIG_FILE` pointed at the local config, and no curl — fetch `.deb` files with python3 urllib. When the VS Code git credential socket dies, a full Codespace restart fixes it.

## Links

- Issue: https://github.com/jupyterlab/jupyterlab/issues/20377
- PR: https://github.com/jupyterlab/jupyterlab/pull/20432
