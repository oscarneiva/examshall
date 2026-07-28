# Exams Hall

A minimalistic web-based timer for IB, IGCSE, and custom exams. Single-page HTML/CSS/JS application with no dependencies.

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
- Color-coded cards: blue (IB), red (IGCSE), user-chosen (Custom)
- High-contrast black text on white background; inverted white text on highlighted warnings
- Large, readable fonts — milestone rows sized close to the countdown for visibility at a distance

## Usage

1. Open `index.html` in a browser — the **Exams Hall** landing page
2. Click **Exam Timer** (or **Incident Log Sheet**)
3. On the timer page, select exam board (IB, IGCSE, or Custom)
4. Enter exam name, duration, and start time; optionally enable extra time and set its percentage (for Custom: also choose warnings and border color)
5. Click **Add Timer**
6. Click the **+** card to add more timers (up to 16)

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
4. **Save Sheet (CSV)** downloads the sheet as a `.csv` file (named after the exam and date), including the Room/Exam/Date header rows

> The CSV is generated entirely in the browser and saved to the device — the site is static, with no backend or upload.

## Timer milestones

| Milestone    | IB  | IGCSE | Custom |
|-------------|-----|-------|--------|
| Reading     | Start - 5 min | -- | -- |
| Start       | Reading + 5 min | User-defined | User-defined |
| 30 min left | End - 30 min | -- | End - 30 min (optional, checkbox) |
| 5 min left  | End - 5 min | End - 5 min | End - 5 min (optional, checkbox) |
| End         | Start + duration | Start + duration | Start + duration |
| Extra time  | End + chosen % of duration (optional, checkbox) | End + chosen % of duration (optional, checkbox) | End + chosen % of duration (optional, checkbox) |

The extra-time row is labelled with its percentage rather than a generic name — e.g. `EXTRA 25%`, or `EXTRA 50%` if that is what was chosen.

When a milestone is reached, its row is highlighted (blue for IB, red for IGCSE, the chosen color for Custom) for one minute. After that minute the highlight moves to the remaining-time countdown — with inverted white text — and stays there until the exam ends.

The countdown shows **Time Remaining** until the normal end, then switches to **Extra Time** (counting down the chosen extra-time allowance) until the extra time finishes. With extra time switched off, the timer simply ends at **END**.

## File structure

```
exams-hall/
  index.html         # Landing page (links to the tools)
  timer.html         # Exam timer application (HTML + CSS + JS)
  incident-log.html  # Incident log sheet (HTML + CSS + JS)
  README.md          # This file
```

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

Any modern browser (Chrome, Firefox, Safari, Edge). No build step or server required. The layout is responsive across desktops, tablets (e.g. iPad), and phones.

## License

Released under the [MIT License](LICENSE).
