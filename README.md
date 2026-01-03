# Project Continuum

## 1. Project Overview
*Project Name:* Continuum Minesweeper
*Platform:* Mobile Web (PWA Optimized)
*Objective:* To provide a meditative yet challenging, endless Minesweeper experience where every move is logically deducible, eliminating the "guessing" element of classic versions.

## 2. Core Gameplay Mechanics
### 2.1 The "No-Guess" Generation Algorithm
The game must generate grids that are solvable using logic alone.

Validation: Upon generation, a virtual "solver" must play through the board. If the solver reaches a point where it must guess (e.g., a 50/50 corner), the board is discarded and regenerated.

First Tap Safety: The first tap must always be a "0" (an opening) to provide the player with a starting logic base.

### 2.2 Standard Ruleset
Reveal: Single tap on a hidden square.

Flagging: Long press (300ms) to toggle a flag.

Chording: Tapping a revealed number when the correct number of adjacent flags are placed will "pop" (reveal) all surrounding unflagged squares.

Risk: If the flags are placed incorrectly, chording will reveal a bomb and end the game.

### 2.3 The "Endless" Loop & Expansion
Instead of a "You Win" popup, completing a grid triggers an expansion.

Trigger: All non-bomb squares in the current grid are revealed.

Expansion: A new grid of equal size spawns on a random adjacent side (North, South, East, or West). The camera pans smoothly to center the new area.

Density Progression: * Start: 15% bomb density.

Increment: Each subsequent grid increases bomb density by 10% (multiplicative).

Cap: Maximum density capped at 38% (the theoretical limit where logic-based solvability becomes statistically rare and overly cramped).


