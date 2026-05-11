# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the games

No build step needed. Open either file directly in a browser:

```
open tictactoe.html
open shooter.html
```

## Project structure

Two standalone, zero-dependency browser games — each is a single self-contained HTML file with all CSS and JS inlined.

- **`tictactoe.html`** — Local two-player Tic Tac Toe with persistent score tracking across rounds (in-memory only, no localStorage).
- **`shooter.html`** — "Neon Assault", a canvas-based top-down shooter.

## Neon Assault architecture (`shooter.html`)

All game code lives in one `<script>` block. Key structures:

**State machine** — a global `STATE` string (`MENU` → `PLAYING` → `LEVEL_COMPLETE` → `GAME_OVER`) drives which update/render path runs each frame. `changeState()` handles transitions and resets entity arrays.

**Entity classes** — `Player`, `Bullet`, `Particle`, and an `Enemy` base class with three subtypes: `BasicEnemy` (Compsognathus raptor), `FastEnemy` (Velociraptor with ghost trail), `TankEnemy` (T-Rex). All sprites are drawn procedurally via Canvas 2D API — no image assets.

**Wave system** — `waveManager` reads from the `LEVELS` array (5 levels × 3 waves), builds a shuffled spawn queue per wave, and trickles enemies in via `spawnTimer`. Level difficulty scales with a `mul` multiplier (`1 + levelIndex * 0.18`).

**Game loop** — delta-time based (`dt = (ts - lastTime) / 1000`, capped at 0.05s). Entities use an `alive` flag and are filtered out at the end of each frame.

**Persistence** — only high score is saved, via `localStorage` key `neonAssaultHS`.

**Color palette** — all colors are defined in the `C` object at the top of the script; reference it rather than hardcoding hex values.

## Git workflow

Single branch (`master`) with a GitHub remote. Commits go directly to `master` — no feature branches or PRs.

**Commit and push frequently throughout any task** — after each logical unit of work, not just at the end. This ensures progress is never lost. A "logical unit" can be as small as completing one feature, fixing one bug, or finishing one meaningful change to a file.

```bash
git add shooter.html        # or tictactoe.html
git commit -m "Description of change"
git push origin master
```

Commit messages use imperative present tense and describe what changed (e.g. `"Add shield power-up to player"`, `"Fix collision radius for TankEnemy"`). Keep them specific enough that the history tells a clear story of what was built.
