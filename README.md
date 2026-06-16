# MedicineTracker

A tiny, mobile-optimized web app to track a **Dicloxacillin Orion** course:
**4 doses a day for 9 days (36 doses total)**.

## Features

- **One-tap logging** — a single big button records each dose with a timestamp.
- **Progress ring** — shows total doses taken out of 36 and the current day (1–9).
- **Today at a glance** — four dots fill in as you take the day's doses.
- **Smart next-dose hint** — suggests roughly when the next dose is due (~6h spacing),
  and tells you when you're done for the day.
- **Undo / Reset** — remove an accidental tap or start the course over.
- **Dose history** — a scrollable list of every logged dose with time and day.

## No backend, no account

Everything runs in a single `index.html` file. Your data is stored only in this
browser's `localStorage` on your device — there is no server, database, or login.
Clearing your browser data (or using private browsing) will erase the log.

## Usage

Open `index.html` in a mobile browser. For a home-screen app feel, use your
browser's **"Add to Home Screen"** option. It also works hosted on any static
host (e.g. GitHub Pages) — no build step required.
