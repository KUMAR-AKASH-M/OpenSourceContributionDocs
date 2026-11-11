# #6873 — Specifying a bad Jitsi server on Android

**Status:** Open — [PR #18189](https://github.com/jitsi/jitsi-meet/pull/18189), head `556bcd39`, "fix(lobby) add cancel button to native lobby screen". CLA cleared 2026-11-04; merging is blocked only on one approving review.

## The issue

Specifying a bad Jitsi server on Android traps the user: the lobby screen offers no way back. Kumar chose the lobby back-button trap as the first thing to fix, and investigated server-URL validation as well, but deliberately did not implement it — one clear fix beats two half-finished ones.

## What was done

The PR adds a cancel button to the native lobby screen, so a user stuck waiting on a bad server can leave. The CLA process was cleared early: DocuSign re-signed on 2026-11-01, an email went to 8x8, and the label flipped to `info:cla-signed` on 2026-11-04. The PR body is honest about what was not done — the fix was never confirmed on a physical Android device, and the body says so plainly.

## What we learned

- Be honest in the PR body about what was not verified. An untested-on-device fix is still a fix, but only if the reviewer knows exactly what was and was not checked.
- The CLA process (DocuSign plus an email to 8x8) is a real step with real waiting time. Clear it early, before the code is ready, so it never becomes the thing holding a PR up.
- Pick one slice of an issue and finish it. The lobby trap was the chosen slice; server-URL validation was investigated and left out, and that was a decision, not an omission.

## Links

- Issue: https://github.com/jitsi/jitsi-meet/issues/6873
- PR: https://github.com/jitsi/jitsi-meet/pull/18189
