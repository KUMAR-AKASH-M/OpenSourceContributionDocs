# #17772 — Investigation (parked)

**Status:** Parked. Investigation only — no code, no PR, no comment. Kumar chose on 2026-11-05 to leave it for later, and it should not be started now.

## The issue

Something was wrong with insets in a Jitsi Meet view, and the shape of the bug was not clear from the report alone. It needed investigation before anyone could say what the fix should be.

## What was done

The investigation pointed at TileView double-counting insets, and at the embedded `JitsiMeetView` having no native inset handling. That explains the tile view, but not a single-participant stage view, so the picture was incomplete — a caveat worth recording rather than papering over. The notes are kept locally, and the issue was parked with Kumar's agreement.

## What we learned

- Investigate before committing to a fix. A few hours of reading can show that the obvious fix addresses only half the report.
- A parked issue with notes is still progress. The next person to pick it up — including future us — starts from the investigation instead of from zero.
- Say plainly what an investigation does not explain. The stage-view gap in this one matters more than the tile-view answer.

## Links

- Issue: https://github.com/jitsi/jitsi-meet/issues/17772
