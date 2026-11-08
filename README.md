# FM27 Player Editor — Cheat Engine Table

A player editor for **Football Manager 27**, built as a Cheat Engine table because the DODI repack of FM27 ships without an in-game editor.

Browse every player in your save, then edit current ability, potential ability, all 54 attributes, personality, height, birth year, reputation and position ratings.

Created by **Rajovan** — Discord: `Rajovan` · GitHub: [github.com/quepast](https://github.com/quepast)

---

## Requirements

- Football Manager 27 (tested on the DODI repack)
- [Cheat Engine](https://www.cheatengine.org/) 7.5 or newer

## Install

1. Download `FM27-Player-Editor.CT`.
2. Launch FM27 and **load a save**.
3. Open the table in Cheat Engine and attach to `fm.exe` (the table does this for you — see below).
4. Run **Step 1: Scan players** and click through the confirmation dialog.
5. Open **Step 2 → Player Browser**.

## How it works

The table scans memory for the class vtable pointer of FM27's player object type, then reads each player's fields directly. There are no pointer paths to walk, so it keeps working after a restart or a save reload — just run Step 1 again. (One module offset for the vtable is baked into the scan; if a game patch moves it, Step 1 will report "No player objects found".)

The scan finds roughly 24,000 players in a full database. Names, clubs and nationalities are read from the game's own name database, so players you have never viewed in-game still resolve.

## What you can edit

| Step | Field |
| --- | --- |
| 3 | Current Ability (CA), Potential Ability (PA) |
| 4 | All 54 attributes (1–100) |
| 5 | Personality traits: Adaptability, Ambition, Loyalty, Pressure, Professionalism, Sportsmanship, Temperament, Controversy (1–20) |
| 6 | Height (cm), Birth year |
| 7 | Three reputation values (labels unverified) |
| 8 | Position ratings for 15 positions (1 = not natural, 20 = natural) |

Every step group is collapsed by default — click the `+` to expand.

## Player Browser

The browser lists every indexed player with Name, Club, Position, Age, CA and PA.

- **Filter** by club or player name.
- **My club only** narrows the list to your own squad.
- **Click a column header** to sort by it; click again to reverse.
- **Double-click a row** (or use *Select player*) to target that player. All the Step 3–8 records then read and write that player.
- **Bulk: all attributes** / **Bulk: CA & PA** apply to the row selected in the list, so you can edit a batch of players without re-running Step 2.
- **Copy list** copies the current filtered view to the clipboard.
- The window is resizable and maximisable.

## CA / PA and attribute limits

- CA and PA accept values up to 200.
- Attributes are 1–100.
- Position ratings are 1–20, where 20 is natural.

Edits are written straight to memory. **Save your game in FM27 afterwards** to persist them — the game does not write these values to disk on its own.

Known limitation: the game may recompute some attributes from CA when it re-evaluates a player. If a bulk attribute edit appears to be ignored after advancing a day, that is the cause, not a table bug.

## Safety

- **Back up your save before editing.** Memory edits are not reversible through the game.
- The table only reads and writes player memory. It does not inject code, hook functions or patch the executable.
- Do not run it on a network/online save you care about.

## Notes from the author

FM27 does not expose a per-player "role" (Channel Midfielder, Playmaking Midfielder, etc.) as a readable field the way positions are exposed, so roles are not editable in this table. The position ratings in Step 8 are what the game actually stores.

The Step 7 reputation labels are unverified — the three values exist at fixed offsets but I could not confirm which is current, potential and world reputation.
