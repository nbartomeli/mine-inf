# Project: Infinite No-Guess Minesweeper

## Overview
A web-based (HTML5/JavaScript) Minesweeper designed for mobile web with an **Infinite Expansion** mechanic. The core differentiator is the **"No-Guess" guarantee**: every generated area is mathematically solvable using logic alone, even when transitioning between sectors.

## Technical Architecture
*   **Format**: Single-file solution (`index.html`) containing HTML, CSS, and JavaScript.
*   **Grid System**: Infinite **Variable-Chunk** architecture.
    *   The world is composed of independent `Chunk` objects with varying dimensions (10x10 to 30x30).
    *   Chunks are stored in a list and accessed via spatial bounds checks.
*   **Rendering**: HTML5 `<canvas>` with viewport-based **culling**. Only chunks currently visible in the camera view are rendered.
*   **Visual Feedback**: Implemented a "Ripple" system to provide visual confirmation of user actions (e.g., flagging), essential for mobile browsers where haptic feedback (vibrate) may be restricted or covered by the user's finger.
*   **UI/HUD**: Overlay stats panel displaying current Level (Sector), Grid Size, and Mine Density.

## Key Components

### 1. Infinite Chunk Manager
*   **Spawning**: When a chunk is solved, the engine identifies empty adjacent space (N, S, E, W) and spawns a new chunk.
*   **Variable Scaling**:
    *   **Dimensions**: Starts at 10x10. Increases by 2 units every 2 levels (e.g., Level 1-2: 10x10, Level 3-4: 12x12). Hard capped at 30x30.
    *   **Mine Density**: Starts at **12%**. Increases linearly by **2%** per level (e.g., Level 1: 12%, Level 2: 14%). Hard capped at **38%**.
    *   **Placement**: New chunks are centered relative to the parent chunk's edge to ensure alignment despite varying sizes.

## Implementation History

1.  **Infinite Expansion & Scaling (Jan 2026)**:
    *   Refactored grid system to support **Variable Chunk Sizes**.
    *   Implemented **Linear Difficulty Scaling** (12% -> 38%) and **Step-wise Size Scaling** (10x10 -> 30x30).
    *   Added HUD overlay for game stats.
    *   Updated chunk placement logic to handle varying sizes.
2.  **Visual Feedback & UX Polish**:
    *   Added **Ripple effect** (expanding ring) for flagging feedback on all devices.
    *   Fixed UI interpolation bugs in HUD labels.
    *   Improved long-press responsiveness (250ms).
3.  **First Click Generation**:
    *   Deferred board generation until the user's first click.
    *   Ensures a "Safe Start" (zero-mine neighbor count) at the initial interaction point.
4.  **Coordinate System Fix**: Unified CSS pixels for inputs and camera.
5.  **Chording Fix**: Enabled recursive flood-fill for chording-triggered reveals.

## Future Work / To-Do
*   **Performance Optimization**: Segment the Tank solver's boundary into connected components to handle larger chunks or complex seams more efficiently.
*   **Visual Polish**: Add animations for revealing cells and flagging.
*   **Settings**: Allow users to configure difficulty scaling and base density.
