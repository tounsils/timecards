# Timecards

A local time tracker for working on multiple projects at once. Click Start on a project, work, click Stop. Add comments per time slot. Review your day by date.

## Features

- Multiple concurrent timers (one project at a time, or all at once)
- Per-slot comments (set while running, on stop, or edit later)
- **Daily project comments** — one summary per (project, day) for the official timesheet/Slice-Pie style report
- Daily timesheet: totals by project (with daily comment) + the individual time slots
- Project metadata: color, emoji icon, notes (info shown on the row + the timesheet)
- **Archive projects** — hide unused projects from the active list/tray; restore or permanently delete from `File → Archived projects…`
- **Hourly rate per project + income estimation** — set a rate per project; totals show earned money (day total, totals tree, day report, CSVs all include it). Configurable currency symbol.
- **Stats dialog** — `File → Stats…` (or `Ctrl+T`): hours + earnings broken down by project across **Today / This week / This month / All time**.
- **Export** — copy a day's report to clipboard (`📋 Copy day` / `Ctrl+Shift+C`) or export day / week to CSV (`File → Export day / Export week`)
- Reorder projects with ▲ / ▼
- **System tray icon** (right side of the taskbar): always visible while the app runs. Shows green when any timer is active. Right-click for running timers + quick-stop, Show/Hide window, Quit.
- **Close-to-tray**: the X button minimizes to the tray instead of quitting. Quit via the tray menu or `Ctrl+Q`.
- Crash-safe: closing mid-timer leaves the entry open; relaunch and Stop normally
- Local SQLite storage — handles years of records without effort

## Download & install

Grab the latest `Timecards.exe` from the [Releases page](../../releases/latest).

Single file, no installer. Drop it anywhere you have write access (e.g. your Desktop or `Documents`) and double-click to run.

### First-launch warning on Windows 11

Windows SmartScreen / Smart App Control may block the executable with **"Windows protected your PC — couldn't verify the publisher."** Timecards is an indie release without a code-signing certificate, so Windows doesn't recognize the publisher yet.

To run it anyway:

1. Click **More info** on the warning dialog.
2. Click **Run anyway**.

Or, before launching: right-click `Timecards.exe` → **Properties** → check **Unblock** → OK.

## Where data lives

By default `timecards.db` is created **next to the executable** (or next to `timecards.py` when running from source). You can point it elsewhere — see *Choosing a database location* below.

> **Caveat (default location)**: if you put `Timecards.exe` somewhere admin-only like `C:\Program Files\...`, it won't be able to write the DB there. Either keep the exe under your user profile, or use the menu to put the DB on a writable path.

## Choosing a database location

The DB path is resolved in this order:

1. `TIMECARDS_DB` environment variable (absolute or relative path) — power-user override.
2. The `db_path` value in the config file.
3. Default: next to the executable / script.

The config file lives at a stable, install-location-independent path:

- **Windows**: `%APPDATA%\Timecards\config.json` → `C:\Users\<you>\AppData\Roaming\Timecards\config.json`
- **Linux/macOS**: `$XDG_CONFIG_HOME/timecards/config.json` (default `~/.config/timecards/config.json`)

### From the app

**File → Database location…** opens a small dialog showing the current path, the config file path, and a `Change…` button. When you pick a new path:

- If the chosen file **already exists**, you're asked to confirm using it. Your current DB is left untouched (use it as a backup).
- If the chosen file **doesn't exist yet**, you're asked what to do with your current data:
  - **Copy** — duplicate the current DB to the new location.
  - **Move** — relocate the current DB to the new location.
  - **Start fresh** — leave the current DB as a backup and begin with an empty one.

The app then prompts you to relaunch (config + connection changes don't hot-swap).

### Manual editing

You can also edit `config.json` directly:

```json
{
  "db_path": "D:/Dropbox/timecards.db"
}
```

Forward or back slashes both work on Windows. Restart the app for the change to take effect.

## Usage notes

- **Database location**: `File → Database location…` to view or change. See *Choosing a database location* above.
- **Add project**: top-right `+ Add project`. Opens a dialog for name, icon (emoji), color, **hourly rate**, and notes.
- **Edit project**: `ⓘ` button on the row — change name, icon, color, rate, notes anytime. The same dialog has an **Archive project** button (left side) — archives instead of deleting, keeping all time entries.
- **Reorder**: ▲ / ▼ buttons on each row.
- **Delete project**: `✕` button (confirms — deletes the project and all its time entries). For a non-destructive option, archive instead.
- **Restore an archived project**: `File → Archived projects…` → select → `Restore`. Same dialog has `Delete permanently…`.
- **Daily comment per project**: double-click any row in the **Totals by project** table to open a multi-line editor. Saved per `(project, day)`. Use this for your "official" daily report text. A `📝` marker appears once a comment is set; clearing the text removes it.
- **Archive or delete a day's row**: right-click any row in **Totals by project**:
  - **Archive this row** — hides that (project, day) from totals/slots (e.g. after you've submitted it to Slice Pie). Reversible. Auto-stops a running timer for that project to avoid silently accumulating against a hidden total.
  - **Delete all entries on this day** — permanent: removes every time slot and the daily comment for that (project, day).
  - Toggle **Show archived** (above the totals table) to reveal archived rows again — they appear grayed with an `[ARCHIVED]` prefix and can be unarchived from the same right-click menu.
- **Start/Stop**: click the button. Multiple projects can run simultaneously.
- **Comment**: type in the row's comment field before Start (stored on the entry) or before Stop (appended on stop).
- **Edit a time slot**: double-click any row in the Time slots table (bottom). Opens a full editor — change project, date, start time, end time, comment. If the new date is different from the one you're viewing, the timesheet jumps to it. A **Delete slot** button inside the dialog removes the slot.
- **Right-click a time slot** for the same actions via a quick menu (Edit slot… / Delete slot).
- **Delete a time slot** (keyboard): select it in the bottom table and press `Delete`.
- **Editing a running slot**: the End time field is disabled (it's still ticking). You can still change the project, start time, or comment. Stop the timer first to set an end time.
- **Change date**: ◀ / ▶, or type `YYYY-MM-DD` + Enter, or `Today`.
- **Copy day to clipboard**: `📋 Copy day` button in the date bar, or `Ctrl+Shift+C`. Produces a clean plain-text report (project, h:mm, decimal hours, daily comment) — paste straight into Slice Pie, email, Slack, etc. Honors **Show archived** when deciding what to include.
- **Export day / week to CSV**: `File → Export day to CSV…` or `File → Export week to CSV…`. The week is the ISO week (Mon–Sun) containing the selected date. CSV columns: Date, Project, Hours (h:mm), Hours (decimal), **Rate**, **Earned**, Archived, Daily comment. Suitable for Excel, invoicing, or archival.
- **Hourly rate**: set in the project edit dialog. Used to compute the Earned column on the totals table, the day total, the daily report (clipboard + CSV), and all Stats views. Set to `0` for unpaid projects — they're shown with `—` instead of money.
- **Currency**: `File → Currency…`. Opens a picker with 25 common currencies (USD, EUR, GBP, JPY, CHF, INR, BRL, AED, SGD, etc.) plus a free-text override for anything custom. Default `$`. Stored in `config.json`. A live preview shows how amounts will be formatted.
- **Add a time slot manually**: `+ Add slot` button next to the **Time slots** header. Useful when you forgot to start the timer. Pick project, date, start/end times (HH:MM 24-hour), and an optional comment. If the slot's date is different from the currently viewed date, the timesheet jumps to that day.
- **Stats**: `File → Stats…` or `Ctrl+T`. Tabbed dialog showing per-project breakdowns and totals for Today, This week (ISO Mon–Sun containing the selected date), This month (calendar month), and All time. Each tab shows Hours, Decimal, Rate, Earned, plus a running total at the bottom with the earned sum highlighted in green.
- **System tray** (next to the clock):
  - Icon is green when any timer is running, gray when idle.
  - The tray icon itself is the **primary color signal**: a slate-gray clock when idle, a **vivid green clock with a red recording-dot overlay** when any timer is running.
  - Right-click → grouped menu with `─ RUNNING ─` (⏹ stop glyph + live elapsed) and `─ start a timer ─` (▶ play glyph) sections. Click any item to toggle. With more than one running, a **Stop all running** item appears. A **Today** line shows hours + earnings at a glance.
  - Menu glyphs are monochrome — native Win32 popup menus do not render color emoji, so the icon carries the color cue and the menu glyphs are chosen for shape contrast (▶ vs ⏹).
  - Hover the tray icon for a live tooltip: `Timecards — 2 running · today: 4h 23m · €330.00`.
  - Right-click → `Show window` / `Hide window` / `Quit Timecards`.
  - Closing the main window with X **or** clicking minimize (`_`) hides to the tray — no taskbar button is left behind. Use `Ctrl+Q` or tray → Quit to actually exit.
