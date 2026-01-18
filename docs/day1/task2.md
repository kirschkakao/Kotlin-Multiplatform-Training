# Task 2: Program the Tile Parser

### Why a Tile Parser?
In game development, parsing data from files (like JSON) into usable objects is crucial. This task introduces you to Kotlin's data handling, immutability, and testing. You'll create a parser that reads tile descriptions from JSON, turning them into immutable data structures – a common pattern in KMP apps for loading game assets.

### Task: Implement the Tile Parser
1. **Understand the Data**: Review the Carcassonne manual and the provided JSON files (e.g., TileDescription.json) for tile metadata (connections, areas, features).
2. **Create Data Classes**: Define immutable data classes in Kotlin for Tile, Areas, etc., using `data class`.
3. **Implement the Parser**: Use Gson (or Kotlin's built-in JSON support) to parse the JSON into your data structures. Ensure immutability (use `val`).
4. **Add Unit Tests**: Write tests to verify parsing works correctly (e.g., check if a tile has the right connections).
5. **Integrate**: Load the tiles into your app's data layer (e.g., a simple list or map).

### What It Should Look Like Now
- Your code compiles without errors.
- Running tests passes (e.g., parsing a sample tile returns the expected object).
- You can access tile data in the app (though no UI yet).

!!! tip
    Use Kotlin's null safety and data classes for clean, error-free code. Check in your solution for review.

---

[Previous: Task 1](task1.md) | [Next: Day 2](../day2/index.md)