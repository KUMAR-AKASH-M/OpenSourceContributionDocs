# #20384 — Mermaid diagrams keep the previous theme in edit-mode cells

**Status:** Merged — [PR #20406](https://github.com/jupyterlab/jupyterlab/pull/20406), merged 2026-11-06 by krassowski, merge commit `c96fd19`, two approving reviews (Darshan808, krassowski), all 110 checks passed. Kumar's second merged PR.

## The issue

Mermaid diagrams kept the previous theme in Markdown cells that were in edit mode during a theme switch. A related fix had already handled diagrams inside the document; the ones being edited were missed, because the cell's rendered output is not refreshed when its source has not changed.

## What was done

The original fix stamped each rendered figure with the theme it was drawn in, using a `data-*` attribute, and a `MutationObserver` refreshed any diagram whose stamp no longer matched the current theme. During review, Darshan808 found the mechanism flawed: the sanitizer strips `data-*` attributes, and only `data-jupyter-id` is allowed through. He reworked the fix to store the theme as a `jp-mod-mermaidTheme-<theme>` CSS class instead, and krassowski hardened the test and moved the new private methods down in the class. Kumar's tests survived the rework in substance, plus one new round-trip test, and the final suite ran 55/55 across four suites.

## What we learned

- New private methods go after the public methods in a class. It is a small style rule, but maintainers notice it, and it was called out explicitly on this PR.
- Verify your mechanism against the real pipeline, not just your own harness. The `data-*` stamp worked in isolation and failed against the sanitizer — the kind of thing only the full render path reveals.
- A reviewer's rework can replace your mechanism entirely, and that is fine. Accept it, verify it properly, and keep your tests; the PR is better for it.

## Links

- Issue: https://github.com/jupyterlab/jupyterlab/issues/20384
- PR: https://github.com/jupyterlab/jupyterlab/pull/20406
