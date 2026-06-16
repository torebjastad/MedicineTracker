# MedicineTracker

A tiny, mobile-optimized web app to track **one or more medicines**, each with
its own schedule (doses per day, course length, start date, and dose times).

## Features

- **Multiple medicines** — switch between them with the chips at the top, or
  add a new one with its own independent schedule.
- **Add / edit schedule** — set name, doses per day, course length (days),
  first-dose date, and the time of each dose. Edit or delete any time.
- **Swipe by day** — a card per day with one button per dose; tap to mark taken,
  tap again to undo. Swipe, use the arrows, or tap a day dot to navigate.
- **Progress at a glance** — overall progress bar, per-day count, and a
  `taken/total` badge on each medicine chip.
- No double-tap zoom, large tap targets, works added to the home screen.

## No backend, no account

Everything runs in a single `index.html`. Your data lives only in this browser's
`localStorage` on your device — there is no server, database, or login. Clearing
your browser data (or private browsing) erases the log.

## Usage

Open `index.html` in a mobile browser (or "Add to Home Screen"). Also works on
any static host such as GitHub Pages — no build step. Live:
https://torebjastad.github.io/MedicineTracker/
