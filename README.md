# Exams Hall

A minimalistic web-based timer for IB, IGCSE, and custom exams, plus an incident log sheet for invigilators and a seating plan maker. Static HTML/CSS/JS with no dependencies, build step, or backend.

## Features

- **IB exams**: 5-minute reading time, 30-minute warning, 5-minute warning
- **IGCSE exams**: 5-minute warning
- **Custom exams**: no reading time; choose whether to show the 30-minute and/or 5-minute warnings, and pick the card's border/accent color
- **Optional extra time on any board**: tick the box to include an extra-time allowance and set its percentage of the exam duration (defaults to 25%)
- **Up to 16 simultaneous timers** in a grid (4 per row, wraps to new rows)
- Drag-and-drop to reorder cards
- Live countdown with current time display
- Inline editable time field per card (reading time for IB, start time for IGCSE/Custom) — all other milestones recalculate automatically
- Edit any card (board, name, duration, start time, extra time, and — for Custom — warnings/color) via the pencil icon, which reopens the same form used to create it
- Default start time set to the current time when adding a timer
- Save reminder popup shown after adding the first timer
- Save Timer button only appears when there are unsaved changes
- Save/load all timer configurations as JSON
- Remove individual timers with the x button
- Color-coded cards: blue (IB), red (IGCSE), user-chosen (Custom) — the accent drives the border, countdown, status label, and milestone highlights (the exam name stays black on every board)
- Bold, color-matched status label above the countdown (**Reading Time**, **Time Remaining**, **Extra Time**, **EXAM ENDED**)
- Typing `1153` into any time field auto-formats to `11:53`
- High-contrast black text on white background; inverted white text on highlighted warnings
- Exam name and countdown scale to the width of their own card (CSS container queries), so four cards per row stay readable without overflowing
- **Seating Setter**: generate a grid of seats from a row/column count and a list of student names, drag cards to swap seats, and export the plan as a CSV file

## Usage

1. Open `index.html` in a browser — the **Exams Hall** landing page
2. Click **Exam Timer**, **Incident Log Sheet**, or **Seating Setter**
3. On the timer page, select exam board (IB, IGCSE, or Custom)
4. Enter exam name, duration, and the start time — for IB this field is the **reading time**, and the exam start is derived as 5 minutes later
5. Optionally enable extra time and set its percentage (for Custom: also choose which warnings to show and the border color)
6. Click **Add Timer**
7. Click the **+** card to add more timers (up to 16)

### Editing a timer

Two ways, and they stay in sync:

- **Inline, on the card** — the board's primary time is a bordered input you can type straight into: the **reading time** for IB (start stays 5 min later), or the **start time** for IGCSE and Custom. Every other milestone recalculates as soon as you leave the field. Invalid entries revert to the previous value.
- **Pencil icon** (top-right, next to the × remove button) — reopens the setup form pre-filled with that timer's details, for changing anything else: board, name, duration, start time, extra time, or (for Custom) warnings and color. Click **Save Changes**.

### Save / Load

- After the first timer is added, a popup reminds you to use **Save Timer** to download your session in case of a shutdown
- **Save Timer** (bottom center) saves all timer configurations as `timer.json` — it's only shown when there are unsaved changes (adding, removing, editing, or reordering timers), and hides again right after saving or loading a file
- **Load Timer** on the setup screen imports a previously saved `timer.json`

### Reordering

Drag any card and drop it onto another card's position to reorder.

## Incident Log Sheet

`incident-log.html` is a separate tool for logging exam incidents (toilet, sickbay, etc.).

1. On load, enter the **Room**, **Exam**, and **Date**
2. Press **Add Log** to record an incident — **Candidate**, **Incident**, **Left** time, **Back** time (times are 24-hour `HH:MM`)
3. Click any row to edit or delete it
4. Click the pencil icon beside the Room/Exam/Date line to correct those details later — same icon and behaviour as the timer's card pencil
5. **Save Sheet (CSV)** downloads the sheet as a `.csv` file (named after the exam and date), including the Room/Exam/Date header rows

> The CSV is generated entirely in the browser and saved to the device — the site is static, with no backend or upload.

## Seating Setter

`seating-setter.html` generates a grid of seats and lets you drag students between them.

1. Enter the grid size as **Rows** and **Columns** (e.g. 4 and 4 for a 4x4 grid)
2. Enter the **Student Names**, one per line (or comma-separated) — the first name fills seat 1, the second fills seat 2, and so on, row by row
3. Click **Generate Seating Plan**
4. Cards with a student assigned get a light red background; empty seats stay white
5. **Drag** a card onto another to swap the two students' seats (their colors move with them)
6. **Click** a card to type a name directly into that seat (useful for empty seats or quick corrections)
7. The small palette icon on a named card's top-right corner opens a color picker to set that card's own color
8. The pencil icon next to the title reopens the setup form, pre-filled, to change the grid size or the name list
9. **Export XLSX** downloads the seating plan as a real `.xlsx` workbook laid out to match the grid (one spreadsheet row per row of seats), with each named cell filled in its card's color

> Fewer names than seats leaves the remaining seats blank; more names than seats fills the grid and leaves the rest out (with a warning). Like the rest of the site, nothing is saved automatically — reloading the page starts a new plan.
>
> The `.xlsx` file is a genuine OOXML workbook (the same zip-of-XML format Excel itself produces), assembled entirely in the browser: a small hand-written zip packer (uncompressed/"stored" entries, with the CRC-32 the format requires) builds the archive, and the styles/worksheet XML parts are written by hand too — no library or backend involved. Colors are written as plain hex RGB, not indexed or theme colors.

## Timer milestones

| Milestone    | IB  | IGCSE | Custom |
|-------------|-----|-------|--------|
| Reading     | User-defined | -- | -- |
| Start       | Reading + 5 min | User-defined | User-defined |
| 30 min left | End - 30 min | -- | End - 30 min (optional, checkbox) |
| 5 min left  | End - 5 min | End - 5 min | End - 5 min (optional, checkbox) |
| End         | Start + duration | Start + duration | Start + duration |
| Extra time  | End + chosen % of duration (optional, checkbox) | End + chosen % of duration (optional, checkbox) | End + chosen % of duration (optional, checkbox) |

The row you type into directly on the card is the **user-defined** one — reading for IB, start for IGCSE and Custom.

A warning row is skipped when the exam is too short to reach it: the 30-minute row needs a duration over 30 minutes, the 5-minute row over 5. The extra-time row is labelled with its percentage rather than a generic name — e.g. `EXTRA 25%`, or `EXTRA 50%` if that is what was chosen.

Custom cards omit the board name above the exam title, since "CUSTOM" carries no meaning to a candidate; IB and IGCSE cards still show theirs.

When a milestone is reached, its row is highlighted (blue for IB, red for IGCSE, the chosen color for Custom) for one minute. After that minute the highlight moves to the remaining-time countdown — with inverted white text — and stays there until the exam ends.

The countdown shows **Time Remaining** until the normal end, then switches to **Extra Time** (counting down the chosen extra-time allowance) until the extra time finishes. With extra time switched off, the timer simply ends at **END**.

## File structure

```
exams-hall/
  index.html          # Landing page (links to the tools)
  timer.html          # Exam timer application (HTML + CSS + JS)
  incident-log.html   # Incident log sheet (HTML + CSS + JS)
  seating-setter.html # Seating plan maker (HTML + CSS + JS)
  favicon.ico         # 96x96 icon - the one Google Search reads
  favicon.svg         # Scalable icon for modern browsers
  og-image.png        # 1200x630 preview image for shared links
  robots.txt          # Allows all crawlers, points to the sitemap
  sitemap.xml         # Lists the site's pages for search engines
  CNAME               # Custom domain for GitHub Pages (examshall.com)
  LICENSE             # MIT
  README.md           # This file
```

The favicon must stay a real file at the site root. An inline `data:` URI renders fine in a browser tab but cannot be fetched by Google's favicon crawler, so search results fall back to a generic globe.

### Search metadata

Each page carries a `description`, a `canonical` URL, and Open Graph / Twitter Card tags; `index.html` also carries `WebApplication` JSON-LD. If a page's title or description changes, update its `og:title` / `og:description` to match — search engines treat a mismatch as a quality signal. `sitemap.xml` lists every page's URL and should gain a row whenever a page is added.

Each page is fully self-contained — its own markup, styles, and script in one file, with nothing shared between them.

## Configuration file format

`timer.json` schema (array of timers):

```json
[
  {
    "board": "IB",
    "name": "English B Paper 1 HL",
    "durationMin": 90,
    "startMin": 540,
    "showExtraTime": true,
    "extraPercent": 25
  },
  {
    "board": "IGCSE",
    "name": "Biology Paper 4",
    "durationMin": 75,
    "startMin": 540,
    "showExtraTime": false
  },
  {
    "board": "Custom",
    "name": "Mock Exam",
    "durationMin": 60,
    "startMin": 540,
    "showExtraTime": true,
    "extraPercent": 50,
    "show30Min": true,
    "show5Min": true,
    "color": "#7c3aed"
  }
]
```

Loading also accepts a single object (legacy format).

| Field          | Type    | Description                            |
|---------------|---------|-----------------------------------------|
| `board`       | string  | `"IB"`, `"IGCSE"`, or `"Custom"`        |
| `name`        | string  | Exam name                              |
| `durationMin` | number  | Exam duration in minutes               |
| `startMin`    | number  | Start time as minutes from midnight (e.g. 540 = 09:00) |
| `showExtraTime`| boolean | Whether to include the extra-time row (any board; omitted means `true`) |
| `extraPercent`| number  | Extra time as a percentage of the duration (defaults to `25`) |
| `show30Min`   | boolean | Custom only — show the 30-minute warning |
| `show5Min`    | boolean | Custom only — show the 5-minute warning |
| `color`       | string  | Custom only — border/accent color (CSS hex, e.g. `"#7c3aed"`) |

## Browser support

Any current browser (Chrome, Firefox, Safari, Edge). No build step or server required — open the files directly, or serve them statically. The layout is responsive across desktops, tablets (e.g. iPad), and phones: the card grid steps from 4 columns to 3, 2, then 1 as the screen narrows.

The cards use [CSS container query units](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries) (`cqi`) to size the exam name and countdown against the card rather than the viewport. That needs Chrome/Edge 105+, Safari 16+, or Firefox 110+ (all released in 2022–23). Older browsers drop those `font-size` declarations entirely and render the exam name and countdown at the inherited body size — small, but every timer still keeps correct time.

## License

Released under the [MIT License](LICENSE).
