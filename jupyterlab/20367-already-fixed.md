# #20367 — Shortcut settings table not refreshing (already fixed)

**Status:** Closed, no PR. Kumar's comment was posted 2026-11-04; the issue was closed 2026-11-03 by YuantaiWei.

## The issue

The keyboard shortcut settings table did not refresh after "Restore to Default". The bug was real on 4.6.4, and it looked like a clean starter issue. It was not: merged PR #19121 had already fixed it on main.

## What was done

Kumar posted a comment reporting that the bug was already fixed on main, and the issue was closed shortly after. No code was written, and no PR was opened — correctly.

## What we learned

- Always check the bug is still present on current main before writing any code. An already-fixed bug becomes a duplicate PR, and a duplicate PR costs the maintainers' time and the contributor's credibility.
- A comment that closes an issue is still a contribution. Verifying a fix and saying so, with the PR that fixed it, is useful work.
- This check belongs at the very start of every issue hunt, before the fix design and before any harness. It is the cheapest step in the whole process and the one that saves the most wasted effort.

## Links

- Issue: https://github.com/jupyterlab/jupyterlab/issues/20367
- Fixing PR: https://github.com/jupyterlab/jupyterlab/pull/19121
