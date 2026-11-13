# #547 — stats: true silently drops nested files in async and stream

**Status:** PR open: [#550](https://github.com/mrmlnc/fast-glob/pull/550) (branch `fix/stats-547`, head `a19bcdc`), awaiting maintainer review. Kumar chose the workaround path on 2026-11-13 ("lets fix it").

## The issue

With `stats: true`, fast-glob silently drops nested files in async and stream mode, while sync mode works fine. Silent data loss is the worst kind of bug: nothing errors, the results are just wrong.

## What was done

The bug was reproduced exactly, and the root cause is not in fast-glob at all. It lives in published `@nodelib/fs.scandir` 4.0.1: the async provider replaces every dirent with a broken `DirentFromStats` that skips all `fs.Dirent` methods, so `isDirectory()` is always false and `fs.walk` never descends into subdirectories. The fix already exists in the nodelib source, commit `00472f3e4186` from 2025-01-08, with a test — but it was never released, so the published package still carries the bug. Kumar chose the workaround path on 2026-11-13: the entry filter (`src/providers/filters/entry.ts`) repairs a broken dirent from the stats that were already collected, using fast-glob's own working `createDirentFromStats`, and only when the stats and the dirent disagree about the entry being a directory — so it is a no-op once the dependency ships the upstream fix, and it never runs when symbolic links are not followed. Two end-to-end tests were added in `src/tests/e2e/options/stats.e2e.ts` (each runs in sync, async and stream); with the fix, async and stream return all 9 nested files instead of 1, unit tests 288 passing, e2e 1386 passing.

## What we learned

- The bug can live in a published dependency, not the repo itself. Reproducing in the repo and stopping there would have blamed the wrong code.
- An unreleased upstream fix changes the shape of the work entirely. There is no bug left to fix upstream — only a release to wait for, a workaround to write, or an analysis to share — and those are three different contributions.
- When the work forks into paths that only the contributor can choose between, stop and ask. Building the wrong one is worse than waiting.

## Links

- Issue: https://github.com/mrmlnc/fast-glob/issues/547
