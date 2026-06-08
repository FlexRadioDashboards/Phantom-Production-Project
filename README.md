# Phantom — HF Amplifier NPI Schedule

A self-contained, interactive schedule for the Phantom program. Three parallel
veins (PCBA, Mechanicals, Magnetics) converge at Build & Test @ Flex. The page
computes the projected finish from task durations + dependencies, draws a hard
line at the target date, and tracks actual completion as the program moves.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The schedule app. Self-contained — no build step, no dependencies. |
| `phantom-schedule.json` | The **canonical baseline**. The page loads this on open. This is the source of truth. |

## Hosting on GitHub Pages

1. Put both files in a repo (root, or a `/docs` folder).
2. Repo → **Settings → Pages** → Source: *Deploy from a branch* → pick your
   branch and folder (root or `/docs`).
3. Live in ~1 minute at `https://<owner>.github.io/<repo>/`.

That's it. Anyone with the link can open it and explore scenarios. Their edits
stay in their own browser — nothing they do changes the hosted baseline.

## How the schedule loads (precedence)

1. **Baked-in defaults** in `index.html` (fallback only).
2. **`phantom-schedule.json`** from the repo — overrides the defaults. *This is
   the baseline everyone sees on a fresh open.*
3. **A share link** (`...#p=...`) — if someone opens a link with an encoded
   scenario, that overrides the baseline *for that view only*.

`RESET` always returns to the canonical baseline (#2), not to a share link.

## Weekly scrum workflow

The baseline only changes when `phantom-schedule.json` in the repo changes.
The Import (↑ JSON) button does **not** publish — it only loads a file into the
current browser. Publishing is a commit. So:

1. **Open the page** — it loads the current canonical baseline.
2. **Make the agreed changes** live during scrum (durations, dates, check off
   completed tasks with their actual dates).
3. **Export (↓ JSON)** the agreed schedule → downloads `phantom-schedule.json`.
4. **Commit that file to the repo**, replacing the old one. Any of:
   - GitHub web UI: drag the file onto the repo → *Commit changes*.
   - Drop it in the synced Shared Drive and let the Drive→Pages pipeline push it.
   - `git add phantom-schedule.json && git commit -m "scrum YYYY-MM-DD" && git push`
5. Done — next open of the page shows the new baseline for everyone.

> Tip: commit each week's `phantom-schedule.json` with a dated message. Git
> history then becomes a free audit trail of how the schedule evolved.

## Buttons

- **⧉ COPY LINK** — encode the current on-screen scenario into a shareable URL.
- **↓ JSON** — export the current schedule to a file (use this to publish).
- **↑ JSON** — load a schedule file into *this browser* (preview only; does not publish).
- **RESET** — discard edits, return to the canonical baseline.
- **ZOOM −/FIT/+** — timeline density.

## Editing the model itself (adding/removing tasks, dependencies)

Task structure (ids, names, dependencies, vein) lives in the `DEFAULT_TASKS`
array inside `index.html`. `phantom-schedule.json` only carries the editable
values (durations, dates, completion) keyed by task id. To add or remove tasks
or rewire dependencies, edit `index.html` and re-deploy.
