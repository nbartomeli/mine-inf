# Project: No-Guess Minesweeper

## Overview
A web-based (HTML5/JavaScript) Minesweeper prototype designed for mobile web. The core differentiator is the **"No-Guess" guarantee**: every generated board is mathematically solvable using logic alone, eliminating 50/50 guessing scenarios.

## Technical Architecture
*   **Format**: Single-file solution (`index.html`) containing HTML, CSS, and JavaScript.
*   **Rendering**: HTML5 `<canvas>` for high-performance rendering of the grid.
*   **Input System**: Custom pointer handling system supporting:
    *   Mouse and Touch events unified.
    *   **Pinch-to-Zoom** and **Drag-to-Pan**.
    *   **Long-Press** (500ms) for flagging (with vibration feedback).
    *   **Chording** (tapping revealed numbers to clear neighbors).

## Key Components

### 1. Board Generator & Solver
*   **Location**: `generateSolvableBoard` and `checkSolvability` functions.
*   **Logic**:
    1.  Place mines randomly (respecting a safe start zone around the first click).
    2.  Run a deterministic solver on the generated board.
    3.  **Solver Logic**:
        *   **Basic Heuristics**: Trivial flags/reveals (e.g., if `flags == number`, reveal rest).
        *   **Tank Solver (Backtracking)**: Analyzes boundary components (hidden cells adjacent to numbers). It uses recursive backtracking to check all valid mine configurations for a component. If a cell is safe in *all* valid configurations, it is revealed. If it is a mine in *all* valid configurations, it is flagged.
    4.  If the solver gets stuck before the board is solved, the board is discarded and regenerated.

### 2. Game Loop
*   **State**: `grid` 2D array storing cell state (`isMine`, `isRevealed`, `isFlagged`, `neighborMines`).
*   **Rendering**: `render()` loop using `requestAnimationFrame`. Draws only the visible area based on `camera` transform (x, y, scale).

### 3. Input Handling
*   **Coordinate System**: 
    *   Input events (`clientX/Y`) are in CSS pixels.
    *   Camera position (`camera.x/y`) is in CSS pixels.
    *   Canvas drawing context is scaled by `devicePixelRatio` for sharpness, but logic coordinates remain in CSS pixels.
*   **Long Press**: Implemented via `setTimeout`. The flag action triggers *immediately* upon timeout (not on release), providing better tactile feedback.

## Implementation History (Recent Changes)

1.  **Coordinate System Fix**: Removed incorrect division by `devicePixelRatio` in input handlers (`handlePointerMove`, `handlePointerUp`). Inputs and Camera now consistently use CSS pixels, resolving click accuracy issues.
2.  **Long Press UX**: Modified `handlePointerDown` to execute the flag toggle immediately when the 500ms timer expires, rather than waiting for the user to lift their finger. `handlePointerUp` was updated to ignore the lift event if a long press occurred.
3.  **Chording Fix**: Refactored `tryChording` to delegate directly to the main `reveal()` function. This ensures that opening a '0' via chording triggers the correct recursive flood-fill (opening adjacent empty squares), which was previously failing.

## Future Work / To-Do
*   **Performance Optimization**: The Tank solver treats the entire boundary as one component. For larger boards (beyond 10x14), segmentation (finding connected components) is necessary to avoid exponential complexity.
*   **Visual Polish**: Add animations for revealing cells and flagging.
*   **Settings**: Allow users to configure difficulty (grid size, mine count).
