# #3066 — TreeSHAP sum

**Status:** Merged — [PR #5275](https://github.com/shap/shap/pull/5275), merged 2026-11-01.

## The issue

A test in `tests/explainers/test_tree.py` summed predictions with `predictions.sum(1)`, a positional axis argument that newer NumPy no longer accepts in that form. The fix is one character of intent: name the axis.

## What was done

The change is a single line at `tests/explainers/test_tree.py:273`, from `predictions.sum(1)` to `predictions.sum(axis=1)`. The full PySpark test was never run, and that limit is stated plainly — it was never claimed to have passed.

## What we learned

- A one-line fix in the right place is a real contribution. Size is not the measure; correctness is.
- Be honest about which tests you could and could not run. Saying the PySpark suite was not run is part of the PR, not a footnote to hide.

## Links

- Issue: https://github.com/shap/shap/issues/3066
- PR: https://github.com/shap/shap/pull/5275
