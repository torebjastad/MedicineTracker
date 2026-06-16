# MedicineTracker — Project Notes

A single-file, mobile-first web app to track **one or more medicines**, each
with its own dosing schedule (doses per day, course length, start date, and
per-dose times). No backend, no database, no account — all data lives in the
browser's `localStorage` on the user's device.

Originally built for a single course of **Dicloxacillin Orion** (4 doses/day
for 9 days); generalized to arbitrary medicines while migrating that data in.

## Owner / context
- Personal tool for the repo owner. The first medicine was an antibiotic course
  the owner started **Sunday** (course Day 1 = that Sunday); dev "today" was
  around 2026-06-16 (Tuesday → Day 3).

## What the app does (current UI)
- **Medicine switcher** (top): horizontal row of chips, one per medicine, each
  showing a mini `taken/total`. Tap to switch; **＋ Add** chip to add one.
- For the selected medicine: **overall progress bar** (`N / total doses`), a
  **day navigator** (`‹ Day X of D ›` + date + Today badge), and a **swipeable
  carousel** of day cards.
- Each day card shows **one button per dose** (Dose 1…perDay) in a grid. Tap to
  mark taken (green + check), tap again to undo. Every dose displays the day's
  **scheduled time** for that slot (consistent across all days, including today).
- **Day dots** under the card (shown only when course ≤ 14 days). Tap to jump.
- **Jump to today** and **✎ Edit schedule** controls. Edit opens a form to
  change name, doses/day, course length, start date, and per-dose times — and
  to **Delete** the medicine. **＋ Add** opens the same form blank.
- **Double-tap zoom is disabled** via `touch-action: manipulation` on the body
  (plus the no-scale viewport), so quick double taps don't zoom the page.

## Data model & storage
- Key: `medtracker.v3` → `{ meds: Medicine[], activeId }`.
  - `Medicine = { id, name, perDay, days, start:"YYYY-MM-DD", times:["HH:MM"…perDay], grid: (number|null)[days][perDay] }`.
  - `grid[dayIndex][slot]` = timestamp the dose was marked, or `null`.
  - Marking a dose stores that slot's scheduled time; the UI always *displays*
    the scheduled time, so changing `times` updates every day at once.
- **Migration on load** (when `medtracker.v3` is absent):
  - from `dicloxacillin.course.v2` (`{start, grid, times}`) → one medicine
    "Dicloxacillin Orion" (perDay 4, days = grid length).
  - else from `dicloxacillin.doses.v1` (timestamp array) → same, grouping
    timestamps per day.
- Data is per-browser/per-device. Clearing site data or private browsing erases
  it. Do day-to-day logging/backfilling on the device actually used.

## Implementation notes
- Single static `index.html` (HTML + inline CSS + inline JS). No build, deps, or
  tests. All logic is one IIFE operating on the **active** medicine via `cur()`.
- The carousel `track` is rebuilt only when the active medicine's structure
  (`id:perDay:days`) changes (`trackSig`); `renderStates()` updates content.
- Tap vs swipe: taps are handled in `touchend` using the touch's start element
  (small movement = tap → `toggle`); the `click` handler is a desktop fallback.
  This was necessary because treating any >~6px move as a swipe swallowed taps.
- Limits: `MAX_PER_DAY = 8`, `MAX_DAYS = 60`.

## Repo / workflow conventions
- **Commit directly to `main`** (owner's request; the old `claude/...` branch was
  merged in). **Do NOT open pull requests** unless explicitly asked.
- **GitHub Pages** enabled (Source: **GitHub Actions**; owner enabled it once via
  Settings → Pages because the Actions token can't create the site on first run).
- Deploy workflow `.github/workflows/pages.yml` runs on every push to `main`
  (`configure-pages` + `upload-pages-artifact` path `'.'` + `deploy-pages`).
- **Live URL**: https://torebjastad.github.io/MedicineTracker/

## Working on this project
- Quick JS syntax check before committing:
  ```
  node -e "const fs=require('fs');const h=fs.readFileSync('index.html','utf8');const m=h.match(/<script>([\s\S]*)<\/script>/);new Function(m[1]);console.log('OK');"
  ```
- After pushing to `main`, confirm the "Deploy to GitHub Pages" workflow run
  succeeds.
