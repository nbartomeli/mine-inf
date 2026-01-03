# Project: Infinite No-Guess Minesweeper

## Overview
A web-based (HTML5/JavaScript) Minesweeper designed for mobile web with an **Infinite Expansion** mechanic. The core differentiator is the **"No-Guess" guarantee**: every generated area is mathematically solvable using logic alone, even when transitioning between sectors.

## Technical Architecture
*   **Format**: Single-file solution (`index.html`) containing HTML, CSS, and JavaScript.
*   **Grid System**: Infinite **Chunk-based** architecture.
    *   The world is divided into 10x10 `Chunk` objects indexed by `cx, cy` coordinates in a `Map`.
    *   Coordinate System: Uses global coordinates (`gr, gc`) for game logic and local coordinates (`r, c`) for chunk-internal storage.
*   **Rendering**: HTML5 `<canvas>` with viewport-based **culling**. Only chunks currently visible in the camera view are rendered.
*   **Input System**: Custom unified pointer handling:
    *   Supports mouse and touch.
    *   **Pinch-to-Zoom** and **Drag-to-Pan** in an infinite world space.
    *   **Long-Press** (500ms) for flagging.
    *   **Chording** (tapping revealed numbers to clear neighbors).

## Key Components

### 1. Infinite Chunk Manager
*   **Spawning**: When a chunk is solved, the engine identifies empty adjacent coordinates (N, S, E, W) and spawns a new chunk.
*   **Stitched Generation**: 
    1.  **Boundary Consistency**: New mines are placed such that they do not contradict clues already revealed on the edges of neighboring chunks.
    2.  **Seam-Aware Solvability**: The solver includes the revealed numbers of adjacent chunks when validating the new area. This ensures a logical path exists from the solved "safe" world into the new "hidden" sector.

### 2. No-Guess Solver (Tank Solver)
*   **Basic Heuristics**: Trivial flags/reveals (e.g., if `flags == number`, reveal rest).
*   **Tank Solver (Backtracking)**: Analyzes the boundary of hidden cells. It checks all valid mine permutations; if a cell is a mine (or safe) in *every* valid configuration, it is flagged (or revealed).
*   **Boundary Awareness**: Adapted to work across chunk seams by treating adjacent chunk cells as "known" constants in the backtracking logic.

### 3. Dynamic Difficulty
*   **Scaling**: Starts at a base mine density of 15%.
*   **Growth**: Density increases by 10% for every solved chunk (compounding).
*   **Cap**: Difficulty is hard-capped at 38% mine density to maintain solvability and performance.

## Implementation History

1.  **Infinite Expansion (Jan 2026)**:
    *   Refactored single-grid logic into a `Chunk` system.
    *   Implemented "Stitched" generation to ensure no-guess transitions between chunks.
    *   Added smooth camera panning when new sectors are discovered.
    *   Implemented dynamic difficulty scaling (15% to 38%).
    *   Added viewport culling for performance in a potentially infinite world.
2.  **Coordinate System Fix**: Unified CSS pixels for inputs and camera.
3.  **Long Press UX**: Immediate flag execution on 500ms timeout with haptic feedback.
4.  **Chording Fix**: Enabled recursive flood-fill for chording-triggered reveals.

## Future Work / To-Do
*   **Performance Optimization**: Segment the Tank solver's boundary into connected components to handle larger chunks or complex seams more efficiently.
*   **Visual Polish**: Add animations for revealing cells and flagging.
*   **Settings**: Allow users to configure difficulty scaling and base density.
