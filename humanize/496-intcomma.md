# #496 — intcomma() loses digits from large decimal strings

**Status:** Closed, not merged — [PR #502](https://github.com/python-humanize/humanize/pull/502), closed 2026-11-02 by hugovk as "Duplicate of #498 and #497". The python-humanize organisation blocked Kumar's account at 2026-11-02 20:28:33 UTC, twelve seconds after the close, with no reason stated. Humanize is closed as a contribution track unless the block is lifted.

## The issue

`intcomma()` lost digits from large decimal strings because it routed them through `float()`, and a float cannot hold that many digits. The fix was to parse plain decimal strings with `Decimal` when `ndigits` is None, with three regression tests, and all 244 tests in `tests/test_number.py` passed locally.

## What was done

The fork and PR went through the browser, because the connector fork failed with a 403. The GitHub web text editor twice mangled the indentation of `number.py`, and the reliable fix was to upload the exact local file instead — the final commit was byte-identical to the tested file. The PR was then closed as a duplicate of #498 and #497, and the organisation block followed immediately. In python-humanize repositories Kumar can no longer open issues, send PRs, comment, or fork, watch, or star; the existing fork still exists publicly.

## What we learned

- Check for duplicate or existing fixing PRs before building. The #497 and #498 trap was visible before a line of code was written, and the whole track turned on missing it.
- Never report a browser file edit as fixed without a raw-file compile or diff check. The web editor can silently mangle indentation, and only the raw file tells the truth.
- A closed PR can have consequences beyond the PR itself. There was no warning here, and there is no appeal path in the record — which is why the duplicate check matters so much in a new repo.

## Links

- Issue: https://github.com/python-humanize/humanize/issues/496
- PR: https://github.com/python-humanize/humanize/pull/502
