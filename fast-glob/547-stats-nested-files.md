# #547 — stats: true silently drops nested files in async and stream

**Status:** Awaiting decision. No PR. Three paths were offered to Kumar on 2026-11-04, and there is no answer yet — do not proceed with any of them without his explicit choice.

## The issue

With `stats: true`, fast-glob silently drops nested files in async and stream mode, while sync mode works fine. Silent data loss is the worst kind of bug: nothing errors, the results are just wrong.

## What was done

The bug was reproduced exactly, and the root cause is not in fast-glob at all. It lives in published `@nodelib/fs.scandir` 4.0.1: the async provider replaces every dirent with a broken `DirentFromStats` that skips all `fs.Dirent` methods, so `isDirectory()` is always false and `fs.walk` never descends into subdirectories. The fix already exists in the nodelib source, commit `00472f3e4186` from 2025-01-08, with a test — but it was never released, so the published package still carries the bug. The three paths offered were to build a fast-glob workaround, to switch to cJSON #1122 instead, or to comment the analysis on #547 only.

## What we learned

- The bug can live in a published dependency, not the repo itself. Reproducing in the repo and stopping there would have blamed the wrong code.
- An unreleased upstream fix changes the shape of the work entirely. There is no bug left to fix upstream — only a release to wait for, a workaround to write, or an analysis to share — and those are three different contributions.
- When the work forks into paths that only the contributor can choose between, stop and ask. Building the wrong one is worse than waiting.

## Links

- Issue: https://github.com/mrmlnc/fast-glob/issues/547
