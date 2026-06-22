---
name: verify-game
description: Verify Tetris game changes are correct — checks canvas/constant sync and confirms the user should test via Live Server
disable-model-invocation: false
---

When asked to verify the game or before reporting a task complete:

1. **Check canvas/constant sync** — Read `game.js` to find the current values of `COLS`, `ROWS`, and `BLOCK`. Then read `index.html` and confirm the board canvas has `width="{{COLS*BLOCK}}" height="{{ROWS*BLOCK}}"`. If they don't match, flag it immediately and show the correct values.

2. **Check next-piece preview** — Confirm the preview canvas in `index.html` is large enough for the `NB` block size used in `drawNext()` in `game.js`.

3. **Report sync status** — State clearly whether the canvas dimensions are in sync or out of sync. If out of sync, provide the exact HTML attribute fix.

4. **Prompt browser test** — Remind the user: "Please open the game in Live Server (right-click index.html → Open with Live Server) and test the change manually — there are no automated tests."

Keep the report concise: sync status, any fixes needed, and the browser test reminder.
