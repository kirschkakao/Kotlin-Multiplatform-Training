# Task 2: UI Screens and Tile Display

### Why UI and State Management?
Compose enables declarative UIs, and StateFlows handle reactive updates. This task builds screens and a tile rotator, demonstrating how UI reacts to state changes.

### Task: Create Screens and Tile Display
1. **Build Selection Screens**: Use Compose to create player and tile selection screens with lists and buttons.
2. **Implement Tile Display**: Show a random tile with rotation buttons. Use StateFlows in a ViewModel to manage rotation state.
3. **Integrate State**: Ensure UI updates when state changes (e.g., tile rotates).

### What It Should Look Like Now
- Selection screens display options and allow interaction.
- Tile window shows a rotatable tile; buttons change its orientation.
- State persists across interactions.

!!! tip
    Test on Windows; use collectAsState() for StateFlow in Compose.

---

[Previous: Task 1](task1.md) | [Next: Day 3](../day3/index.md)