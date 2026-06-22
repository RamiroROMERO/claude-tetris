# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Zero-dependency Tetris implemented in vanilla HTML5/CSS3/JavaScript (ES6+). No build step, no package manager, no transpilation — the game runs directly in any modern browser.

## Running the Game

Use **VS Code Live Server** (right-click `index.html` → "Open with Live Server") to preview changes. Always test in the browser after modifications; there are no automated tests.

## Critical Gotcha: Canvas Dimensions Must Match JS Constants

`index.html` has hardcoded canvas size attributes:
```html
<canvas width="300" height="600">  <!-- game board -->
<canvas width="120" height="120">  <!-- next-piece preview -->
```

These **must** stay in sync with `game.js` constants:
- Board: `COLS * BLOCK` × `ROWS * BLOCK` (currently 10×30=300, 20×30=600)
- Preview: uses `NB = 30` inside `drawNext()`

If you change `COLS`, `ROWS`, or `BLOCK`, update the HTML canvas attributes too or the game will render incorrectly.

## Code Style

- `'use strict'` at the top of JS files
- `const`/`let` only — no `var`
- 2-space indentation
- camelCase for functions and variables (e.g., `lockPiece`, `rotateCW`, `tryRotate`)
- No formatter is configured — follow the existing style manually

## UI Language

Game UI text (labels, overlay, buttons) is in **Spanish** ("GAME OVER", "PAUSA", "Reiniciar"). Keep new UI strings in Spanish.

## Architecture Notes

- Rotation uses a basic wall kick with 5 offset attempts `[0, -1, 1, -2, 2]` — not SRS (Super Rotation System)
- Drop speed is capped at 100ms minimum: `Math.max(100, 1000 - (level - 1) * 90)`
- The entire game is rendered via Canvas 2D API — no DOM manipulation for game state
