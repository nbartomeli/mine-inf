# Project: Infinite No-Guess Minesweeper

## Overview
A web-based (HTML5/JavaScript) Minesweeper designed for mobile web with an **Infinite Expansion** mechanic. The core differentiator is the **"No-Guess" guarantee**: every generated area is mathematically solvable using logic alone, even when transitioning between sectors.

## Technical Architecture
*   **Format**: Single-file solution (`index.html`) containing HTML, CSS, and JavaScript.
*   **Grid System**: Infinite **Variable-Chunk** architecture.
    *   The world is composed of independent `Chunk` objects with varying dimensions (10x10 to 30x30).
    *   Chunks are stored in a list and accessed via spatial bounds checks.
*   **Rendering**: HTML5 `<canvas>` with viewport-based **culling**. Only chunks currently visible in the camera view are rendered.
*   **UI/HUD**: Overlay stats panel displaying current Level (Sector), Grid Size, and Mine Density.
*   **Input System**: Custom unified pointer handling:
    *   Supports mouse and touch.
    *   **Pinch-to-Zoom** and **Drag-to-Pan** in an infinite world space.
    *   **Long-Press** (250ms) for flagging.
    *   **Chording** (tapping revealed numbers to clear neighbors).

## Key Components

### 1. Infinite Chunk Manager
*   **Spawning**: When a chunk is solved, the engine identifies empty adjacent space (N, S, E, W) and spawns a new chunk.
*   **Variable Scaling**:
    *   **Dimensions**: Starts at 10x10. Increases by 2 units every 2 levels (e.g., 10x10 -> 10x10 -> 12x12). Hard capped at 30x30.
    *   **Placement**: New chunks are centered relative to the parent chunk's edge to ensure alignment despite varying sizes.
*   **Stitched Generation**: 
    1.  **Boundary Consistency**: New mines are placed such that they do not contradict clues already revealed on the edges of neighboring chunks.
    2.  **Seam-Aware Solvability**: The solver includes the revealed numbers of adjacent chunks when validating the new area.

### 2. No-Guess Solver (Tank Solver)
*   **Basic Heuristics**: Trivial flags/reveals (e.g., if `flags == number`, reveal rest).
*   **Tank Solver (Backtracking)**: Analyzes the boundary of hidden cells. It checks all valid mine permutations; if a cell is a mine (or safe) in *every* valid configuration, it is flagged (or revealed).
*   **Boundary Awareness**: Adapted to work across chunk seams by treating adjacent chunk cells as "known" constants in the backtracking logic.

### 3. Dynamic Difficulty
*   **Scaling**: Starts at a base mine density of **12%**.
*   **Growth**: Linear growth. Density increases by **2% (0.02)** for every new sector.
*   **Cap**: Difficulty is hard-capped at **38%** mine density.

## Implementation History

1.  **Infinite Expansion & Scaling (Jan 2026)**:
    *   Refactored grid system to support **Variable Chunk Sizes**.
    *   Implemented **Linear Difficulty Scaling** (12% -> 38%) and **Step-wise Size Scaling** (10x10 -> 30x30).
    *   Added HUD overlay for game stats.
    *   Updated chunk placement logic to handle varying sizes.
2.  **First Click Generation**:
    *   Deferred board generation until the user's first click.
    *   Ensures a "Safe Start" (zero-mine neighbor count) at the initial interaction point.
3.  **Coordinate System Fix**: Unified CSS pixels for inputs and camera.
4.  **Long Press UX**: Immediate flag execution on 250ms timeout with haptic feedback.
5.  **Chording Fix**: Enabled recursive flood-fill for chording-triggered reveals.

## Future Work / To-Do
*   **Performance Optimization**: Segment the Tank solver's boundary into connected components to handle larger chunks or complex seams more efficiently.
*   **Visual Polish**: Add animations for revealing cells and flagging.
*   **Settings**: Allow users to configure difficulty scaling and base density.
