# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## What this project is

**CircleSquareTriangle** is a self-contained, mobile-first, two-player logic
game for kids (the two players are named Adele and Alma in the UI). It's a
"shape Sudoku" played on a 3×3 grid with three shapes (circle, square,
triangle) and three colors (red, blue, yellow). One player is the **Thinker**
(reads clues), the other is the **Builder** (places pieces).

There are two daily puzzle modes:

- **🌈 Rainbow Rows (Mode A)** — a full 3×3 double Latin square: every row and
  column must contain one of each shape *and* one of each color.
- **🔍 Clue Hunt (Mode B)** — a partially filled grid solved from clue
  "tickets," including one **secret card** only the Builder may peek at.

Solving a mode unlocks a **👻 Boss replay**, where placed pieces fade out after
3 seconds so players must remember/announce them.

## Repository layout

```
.
├── index.html   # The entire game — HTML, CSS, and JS in one file
├── README.md    # Project title
└── CLAUDE.md    # This file
```

**The whole application lives in `index.html`.** There is no build step, no
package manager, no dependencies, and no framework. Open the file in a browser
(or serve the directory) and it runs. It is intended to be hosted as a static
`index.html`.

## Architecture (all inside `index.html`)

The single `<script>` is deliberately split into two halves:

1. **Logic engine (no DOM).** Everything above the `if (typeof document...)`
   guard is pure and testable in Node — it is exported via
   `module.exports = { genA, genB, countSolutionsB, violationsA, LS, makeDaily, dateSeed }`.
   Key pieces:
   - Pieces are `{s, c}` objects: `s` = shape index 0–2, `c` = color index 0–2.
   - The grid is a flat array of 9 (row-major); `null` means empty.
   - `mulberry32` + `dateSeed` give **seeded daily puzzles** — the same
     calendar day always yields the same two puzzles, and they can't be
     re-rolled. Cosmetic randomness (confetti) uses `Math.random` after
     generation.
   - `genA` builds Mode A from the 12 valid 3×3 Latin squares (`LS`) and
     reveals givens until the solution is unique.
   - `genB` builds Mode B by generating a solution, choosing clues from a pool
     of clue factories (`clueRowColNo`, `clueCount`, `clueCellNot`,
     `clueCornerShape`, `clueNextTo`, `clueSecret`), and using
     `countSolutionsB` (backtracking with early exit) to guarantee exactly one
     solution, then pruning unneeded clues.
   - `makeDaily(mode, seed)` is the daily entry point.

2. **UI layer (touches the DOM).** Everything inside the `document` guard —
   SVG builders for shapes/colors/mini-maps, WebAudio sound effects (fail
   silently), rendering functions (`renderHeader/Clues/Grid/Palette/Secret`),
   interaction (`tapCell`, `afterMove`), the boss "ghost" hide/peek timers, and
   the win/confetti flow. State lives in the `S` object (`S.A`, `S.B`, plus the
   active `S.mode`); puzzles are generated lazily per tab via `ensureP`.

### Conventions to preserve

- **Keep the logic/DOM split.** Don't reach into `document` above the guard —
  that boundary is what makes the engine Node-testable.
- **Vocabulary is child-friendly:** cells are called "spaces," colors are shown
  as teardrops (never the circle shape) so shape and color are never confused.
- **Accessibility matters:** `aria-label`s on cells/buttons, `aria-live` hints,
  visible focus rings, and `prefers-reduced-motion` handling exist — maintain
  them when editing.
- **Single-file, zero-dependency, offline-capable.** Do not introduce a build
  system, external assets, or CDN links without a clear reason; the game is
  meant to be a standalone `index.html`.
- **No page scroll ever** — the app is a fixed-viewport flex column sized to fit
  a phone. Layout changes should respect that.

## Development workflow

- **Run it:** open `index.html` in a browser, or `python3 -m http.server` in the
  repo root and visit the served page. No install step.
- **Testing the engine:** the logic half is `require`-able in Node (it guards on
  `typeof module`). There is no test suite committed yet; if you add tests,
  target the exported functions and document the command here.
- **Branching:** default branch is `main`; do work on feature branches with
  clear, descriptive commit messages.

## Keeping this file honest

Update `CLAUDE.md` in the same change that alters the structure, adds tooling,
or changes a convention above, so it never drifts from the actual code.
