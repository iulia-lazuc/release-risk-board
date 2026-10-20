# Release Risk Board

**Demo:** https://release-risk-board.vercel.app (opens with example data; nothing you do there is stored on a server)

A planning board for release breakdown sessions: epics (grouped by saga) are split into FE / BE / UX milestones, which are dragged into the remaining cycles. Anything that won't fit lands in **Release Risk**.

## Where the data lives
The board stores **nothing on a server**. It runs in your browser and reads/writes an **Excel file** you choose:

- **Open Excel** loads a board file (`.xlsx`).
- The board opens in **view mode**: nobody can change anything by accident.
- **Edit** turns on editing. Every change (new milestone, drag to a cycle, epic edits) is saved into the open Excel file automatically within a second ("Saved to Excel 10:42"). **Done editing** switches back to view mode.
- **Save** (or Ctrl+S) is there for the first save of a new board, or in browsers that can't write into files (they download a copy instead). Automatic saving needs Edge or Chrome.
- Keep the file in a SharePoint/OneDrive folder that is **synced to your laptop** (so it shows up in File Explorer). The board saves into that local file and OneDrive syncs it to SharePoint.
- Close the file in the Excel app while you edit it on the board, otherwise Windows locks it and saving fails.
- Teammates open the same file (with View rights in SharePoint) to see the plan; they click Open Excel again to see the latest version.

The hosting (Vercel / GitHub Pages) only delivers this page's code. Epic data never leaves your computer.

## Publish for team (no Excel step for viewers)
- In **Edit** mode, click **Publish for team** once and save `board-data.js` in the **same folder as `index.html`** (the folder your team opens the board from).
- From then on, every change you make also updates `board-data.js` automatically. Anyone opening the board sees the published plan ("Published plan · as of 20 Oct, 10:42") without opening Excel.
- The Excel stays the master copy; `board-data.js` is a read-only copy of it for viewers.
- `board-data.js` contains project data. It is listed in `.gitignore` so it is never pushed by accident. If the team opens the board via an internal GitHub Pages site, add it on purpose with `git add -f board-data.js` and push **only to the internal repository**.

## Reconnect
The board remembers the last Excel file in this browser. When you open the board, click **Reconnect to <file>.xlsx** to continue editing in one click.

## Excel layout
| Sheet | Columns |
|---|---|
| Release | Release name |
| Cycles | Cycle, End date (YYYY-MM-DD) |
| Epics | ID (do not change), Priority, Saga, Key, Title, Size, Jira link, Release risk flag, Risk reason, Open questions |
| Milestones | Epic ID, Epic, Discipline (FE/BE/UX), Milestone, Column (Milestones / cycle name / Release Risk) |

You can edit the file in Excel too; the board reads it back.

## Files
- `index.html` – the whole board
- `vendor/` – bundled libraries (SortableJS for drag and drop, SheetJS for Excel), so nothing loads from external sites
