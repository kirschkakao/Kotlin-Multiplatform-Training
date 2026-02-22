# Day 3: Connecting the Frontend to the Backend and Building a Tile Card

## Overview
Today you'll connect your frontend to your backend architecture and learn how to properly manage state and dependencies. You'll implement ViewModels and manual dependency injection to manage your application's state, then connect the domain layer to the data layer with repositories and containers. Finally, you'll build a reusable tile card component from a Figma design.

## Before You Start
Make sure you have completed all Day 2 tasks and that your project matches the expected baseline.

!!! warning "Important"
    Whenever you need to import objects from external libraries and have more than one option to choose from, prioritize `androidx.compose` and `material3` imports.

## Tasks Overview

### 1. [ViewModels and manual Dependency Injection](task1.md)
Move state and business logic from the UI layer into ViewModels using StateFlows. Implement `AppViewModelProvider` to externalize ViewModel creation and understand the differences between constructor and field injection.

### 2. [Persist and Manage Game State](task2.md)
Connect the domain layer to the data layer by implementing Repositories and DAOs. Create `AppContainer` to provide repositories as singletons and visualize the complete dependency flow from screens to database.

### 3. [Building a Tile Card](task3.md)
Decompose a Figma design into reusable Jetpack Compose components. Load and map tile textures from assets, implementing `TileCell`, `CurrentPlayerBadge`, `CurrentTileHeader`, and `CurrentTileCard`.

---

[Summary](summary.md) | [Next: Day 4](../day4/index.md)