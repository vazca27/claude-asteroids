# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Asteroids clone in pure HTML5 Canvas + vanilla JS (README and UI are in Spanish). No dependencies, no bundler, no build step, no tests, no linter.

## Running

Open `index.html` directly in a browser, or serve locally with `npx serve .` (then http://localhost:3000). Deployed demo is GitHub Pages.

## Architecture

All game logic lives in a single script, `game.js` (loaded by `index.html`, which holds the `#canvas` element).

- Fixed logical canvas of 800x600 (`W`, `H`); the world is toroidal — entities use the `wrap` helper for edges.
- Input: `keys` (held) and `justPressed` (one-frame edge) maps, read via `pressed(code)`.
- Entities are classes (`Bullet`, `Asteroid`, `Ship`, `Particle`) held in module-level arrays (`bullets`, `asteroids`, `particles`) plus `ship`.
- Asteroid size is 1/2/3 (small/medium/large); `RADII`, `SPEEDS`, `POINTS` are indexed by size (index 0 unused). Destroying one splits it into smaller asteroids.
- Game state machine via `state`: `'playing' | 'dead' | 'gameover'`; `deadTimer` handles respawn delay. `initGame()` resets, `nextLevel()` spawns the next wave.
- Main loop: `loop(ts)` computes `dt` and calls `update(dt)` then `draw()` (`drawHUD`, `drawOverlay`) via `requestAnimationFrame`. Movement is dt-based (seconds).
- Power-ups and the shooting-star asteroid were removed in git history; the README description still mentions them but the code does not.
