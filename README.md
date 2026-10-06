# Study Sprint

A focus timer and task list for study sessions — one HTML file, no build step, no dependencies, no account.

Run a timed sprint, tick off what you studied, and keep your day streak going. Everything is stored in your own browser.

## Features

- **Focus sprint timer** — presets of 15 / 25 / 45 / 50 minutes, with start, pause, resume and reset.
  The remaining time is mirrored into the browser tab title, so you can see it in a background tab.
- **Sprint completion** — a beep (generated with the Web Audio API, no sound file), a toast, and the
  sprint is logged to your daily total.
- **Task list** — add tasks (up to 140 characters), check them off, delete individually, or clear all
  finished ones at once. Open/total counter at the bottom.
- **Daily stats** — sprints done today, focus minutes today, and a consecutive-day streak.
- **Works offline** — no network requests at all, including the favicon (inline SVG data URI).

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| `Space` | Start / pause the timer |
| `R` | Reset the timer |

Shortcuts are ignored while you are typing in a field or an element like a button is focused, so they
never interfere with normal form use.

## Run it

Open the file directly:

```bash
open index.html          # macOS
```

If your browser blocks `localStorage` on `file://` URLs (tasks disappear when you close the tab),
serve the folder over HTTP instead:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Data & privacy

All state lives in `localStorage` under the key `study-sprint-v1`:

```json
{
  "length": 25,
  "tasks": [{ "text": "Read chapter 4", "done": false }],
  "sessions": [{ "at": 1791284471370, "min": 25 }]
}
```

Nothing is sent anywhere — there is no server, no analytics, no login. Notes:

- Data is **per browser and per device**. Switching browsers or clearing site data starts over.
- Corrupted or unreadable saved data falls back to defaults instead of throwing.
- **Reset all data** at the bottom of the page wipes sprints and tasks after a confirmation prompt.

## Project structure

```
.
├── index.html                    # markup, styles and script in one file
├── README.md
└── .qoder/repowiki/              # generated wiki notes from the editor (not part of the app)
```

`index.html` is organised as `<style>` (design tokens, then per-section rules), the semantic body
markup, and a single IIFE script split into storage, timer, presets, stats and task sections.
Vanilla ES5-compatible DOM code — no framework, bundler, or package manager.

## Known limitations

- A sprint only counts if the page is still open when it finishes; closing the tab mid-sprint
  discards that session (the running timer is not persisted).
- "Today" is based on the device clock, so changing it affects the streak and daily totals.
- No cross-device sync, no dark/light theme switch (the design is dark only).

## Publishing with GitHub Pages

Since `index.html` sits at the repository root, the site can be served as-is: repo **Settings → Pages →
Source: Deploy from a branch → main / (root)**. The URL will be
`https://<your-username>.github.io/study-sprint/`. Remember that Pages data is stored per browser, so
visitors each get their own empty task list.
