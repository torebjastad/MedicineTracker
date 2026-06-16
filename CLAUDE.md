# MedicineTracker — Project Notes

A single-file, mobile-first web app to track a course of **Dicloxacillin Orion**:
**4 doses per day for 9 days (36 doses total)**. No backend, no database, no
account — all data lives in the browser's `localStorage` on the user's device.

## Owner / context
- Personal tool for the repo owner's own antibiotic course.
- The owner started the course **Sunday morning** (course Day 1 = that Sunday).
- "Today" examples in development were around 2026-06-16 (a Tuesday → Day 3).

## What the app does (current UI)
Swipeable day-by-day interface optimized for minimum taps:
- **Overall progress bar** at the top: `N / 36 doses`.
- **Day navigator**: `‹ Day X of 9 ›` with the calendar date and a **Today** badge.
- **One card per day** in a horizontal carousel. Swipe left/right (or use the
  arrows / day dots) to move between the 9 days.
- Each day card has **four large dose buttons** (Dose 1–4) in a 2×2 grid.
  Tap to mark a dose taken (fills green + check + time); tap again to undo.
  - Untaken buttons show the **suggested** time (08:00 / 13:00 / 18:00 / 22:00).
  - Taken buttons show the **actual** recorded time. On "today" that's the tap
    time (`Date.now()`); on other days it defaults to the suggested slot time.
- **Day dots** (9) below the card: empty / partial / full, current day enlarged.
  Tap a dot to jump to that day.
- **Jump to today**, **Day 1 starts <date>** (editable start date), and **Reset**
  controls. Changing the start date only relabels day dates; dose data stays on
  the same day indices.

## Data model & storage
- Key: `dicloxacillin.course.v2` →
  `{ start: "YYYY-MM-DD", grid: number|null[9][4] }`.
  - `start` = calendar date of Day 1.
  - `grid[dayIndex][slot]` = timestamp the dose was taken, or `null`.
- **Migration**: older builds stored `dicloxacillin.doses.v1` = array of
  timestamps. On load, if v2 is absent, the app migrates v1 by grouping
  timestamps per calendar day (start = earliest dose's date) and filling slots.
- Data is per-browser/per-device. Clearing site data or using private browsing
  erases it. Backfill on the device used day-to-day.

## History of the build (important decisions)
1. Started as a single `index.html` with a circular progress ring + one big
   "log a dose now" button (timestamps array, v1 storage).
2. Added a "log earlier dose" datetime picker for backfilling past days.
3. Reported issue: after adding an earlier dose, the UI still showed the
   "log first dose" state. Resolved by the redesign (no "first dose" flow now).
4. Redesigned to the **swipeable per-day, 4-button** interface above (v2 storage,
   with v1 → v2 migration). Suggested slot times added for context.

## Repo / workflow conventions
- **Commit directly to `main`** (the owner requested this; earlier work was on
  branch `claude/dicloxacillin-tracker-4gsaro`, now merged into `main`).
- **Do NOT open pull requests** unless explicitly asked.
- **GitHub Pages** is enabled (Source: **GitHub Actions**). The owner enabled it
  manually once via Settings → Pages, because the Actions token cannot create a
  Pages site on first run.
- Deploy workflow: `.github/workflows/pages.yml` deploys the static site on
  every push to `main` (uses `configure-pages` + `upload-pages-artifact` +
  `deploy-pages`). It uploads the repo root (`path: '.'`).
- **Live URL**: https://torebjastad.github.io/MedicineTracker/

## Working on this project
- It's a single static `index.html` (HTML + inline CSS + inline JS). No build
  step, no dependencies, no tests.
- Quick JS syntax check before committing:
  ```
  node -e "const fs=require('fs');const h=fs.readFileSync('index.html','utf8');const m=h.match(/<script>([\s\S]*)<\/script>/);new Function(m[1]);console.log('OK');"
  ```
- After pushing to `main`, confirm the "Deploy to GitHub Pages" workflow run
  succeeds.
