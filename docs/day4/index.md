# Day 4: The Game Screen

## Overview
Today you'll build the core game screen for your Carcassonne app. You'll start by implementing reusable grid structures and a `TileGrid` that models valid tile placement on the board. Then you'll add tile rotation and placement logic, connect the board to your `GameViewModel`, and create an interactive game board that highlights valid moves.

Finally, you'll turn this prototype into a proper game screen. You'll make the board draggable, refine the layout, add a top app bar with game actions, and implement player rotation so turns can progress correctly.

## Before You Start
Make sure you have completed all Day 3 tasks and that your project matches the expected baseline.

!!! warning "Important"
    Whenever you need to import objects from external libraries and have more than one option to choose from, prioritize `androidx.compose` and `material3` imports.

## Tasks Overview

### 1. [The Tile Board](task1.md)
Implement a reusable `Grid` and `TileGrid`, build a `StaticSimpleGrid` composable, and create the first version of the `GameBoard` connected to `GridState` in the `GameViewModel`.

### 2. [Rotate and Place Tiles](task2.md)
Implement tile rotation, highlight valid placement positions on the board, and place tiles into the `TileGrid` while updating the game state.

### 3. [Building the Game Screen](task3.md)
Make the board draggable, clean up and restructure the game screen layout, add a main top app bar, and implement end-of-turn player rotation.

---

[Previous: Day 3](../day3/index.md) | [Summary](summary.md) | [Next: Day 5](../day5/index.md)