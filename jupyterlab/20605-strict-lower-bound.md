# #20605 — Strict `>` lower bound treated as inclusive in `_test_overlap`

**Status:** Closed, no merge — but the fix landed. Kumar filed issue #20605 on 2026-11-24, opened PR #20607 on 2026-11-25, and Darshan808 closed the PR (not merged) the same day, carrying the fix into jupyterlab/jupyter-builder#215 with Kumar credited as co-author. The follow-up, #20609 (import the functions from `jupyter_builder` instead of keeping a copy), is now Darshan808's: on 2026-11-26 he said he is doing the lab-side swap himself in draft PR #20613, so Kumar should not start it; jupyter-builder#215 is ready for review.

## The issue

`_test_overlap(">1.0.0", "1.0.0")` returned `True`, so an extension asking for `>1.0.0` looked compatible with a core pinned at `1.0.0`. The same flaw showed up symmetrically (`"1.0.0"` vs `">1.0.0"`) and against a hyphen range ending at the bound (`">2.0.0"` vs `"1.0.0 - 2.0.0"`). A laptop repro against real repo code confirmed all three cases wrongly returned `True`, with controls (`>=1.0.0` vs `1.0.0`, `>1.0.0` vs `1.0.1`) behaving correctly.

## What was done

The fix replaced the endpoint-probing overlap test in `_compare_ranges` with an interval intersection test that tracks whether each bound is inclusive. The obvious one-line fix was wrong: tightening the endpoint comparisons alone breaks overlaps whose overlap is interior-only, like `>1.0.0 <2.0.0` against itself. The rewrite also covered the hyphen upper bound from #20592, so chrikrah's PR #20596 may have been redundant — the PR description said so plainly and let the maintainers choose.

- Branch `fix/test-overlap-gt-bound-20605`, head `3d5ce18`: test commit `c5f5175` (11 new assertions in `test_compatibility`, 3 ordering assertions in `test_compare_ranges`) + fix commit `3d5ce18` (`jupyterlab/commands.py`, `_compare_ranges` only, +55/−24).
- Tests failed before the fix as expected, then passed after: targeted 2 passed, full test file 44 passed, 1 skipped. Ruff check, ruff format and mypy all clean.
- PR #20607 opened 2026-11-25 09:34 IST. Its only failing check was enforce-label — the PR had no triage label. Kumar could not add the label himself (GitHub shows no label control for his account on the repo). Darshan808 added the `bug` label at 11:56 IST and the check passed; he also asked for the PR template to be followed, and the description was rewritten in template format at 12:13 IST.
- Darshan808 filed #20609 at 12:43 IST: fix the overlap logic in jupyter-builder only, then replace jupyterlab's copy with an import, as #18841 did for semver. Kumar commented at 13:08 IST asking if he can take it and whether to start now or wait for #20596 / #20607 to be decided.
- Darshan808 closed #20607 at 13:21 IST: maintaining two copies is a pain, so the fix is kept only in jupyter-builder. His draft PR jupyter-builder#215 ports the fixes (fixes #20592 + #20605, based on #20607's interval-intersection approach), and moves the tests over. Darshan808 replied on #20609 at 13:26 IST that the next step is removing the functions from jupyterlab and depending on builder's, after #215 merges and jupyter-builder releases.
- Co-author credit verified 2026-11-25 13:29 IST: commit `a2621ac9` "Fix version range overlap bounds" in #215 carries `Co-authored-by: Kumar Akash <146612128+KUMAR-AKASH-M@users.noreply.github.com>` alongside chrikrah. Re-checked 14:20 IST: the PR page still shows the commit with both co-authors.

## What we learned

- Small is not trivial. Kumar worried this fix looked too simple, but the one-line version is wrong, which is exactly why it needed the rewrite and the tests. Judge a fix by whether the naive version works, not by its size.
- A closed PR is not wasted work when the fix still lands with credit. The maintainer's call was about where the code should live, not about the fix being wrong — and the co-author credit is the record that it was Kumar's work.
- Check for duplicated copies before fixing in jupyterlab. `_test_overlap` and `_compare_ranges` existed in both jupyterlab and jupyter-builder, already diverged, and the maintainer preferred fixing the single copy. Finding the duplicate first would have pointed the PR at jupyter-builder from the start.
- Always follow the org's PR template from the first draft. Read the repo's template before writing the description and use its sections. #20607 was filed free-form and Darshan808 asked for the template; never submit a non-template description again.
- You cannot label your own PR in this repo. The enforce-label failure is fixed by a maintainer at triage, as Darshan808 did here and on #20414 — do not spend time trying to add it yourself.

## Links

- Issue: https://github.com/jupyterlab/jupyterlab/issues/20605
- PR (closed, not merged): https://github.com/jupyterlab/jupyterlab/pull/20607
- Fix carried over: https://github.com/jupyterlab/jupyter-builder/pull/215 (commit `a2621ac9`, co-authored by Kumar Akash and chrikrah)
- Follow-up: https://github.com/jupyterlab/jupyterlab/issues/20609
- Related: https://github.com/jupyterlab/jupyterlab/issues/20592 and https://github.com/jupyterlab/jupyterlab/pull/20596
