# Task 4: Implement the Tile Parser

### About This Task
In game development, parsing data from files (like JSON) into usable objects is crucial. This task reinforces Kotlin concepts like data classes and immutability while building a core component of the Carcassonne game engine.

### Why a Tile Parser?
The Tile Parser reads game asset descriptions from JSON files and converts them into immutable Kotlin data structures. This is a fundamental pattern in Kotlin Multiplatform apps for loading and managing game data.

### Task: Implement the Tile Parser
1. **Understand the Data**: Review the Carcassonne manual and the provided JSON files (e.g., TileDescription.json) for tile metadata (connections, areas, features).

2. **Create Data Classes**: Define immutable data classes in Kotlin for Tile, Areas, etc., using `data class`. These should match the structure of your JSON files.

3. **Implement the Parser**: Use Gson (or Kotlin's built-in JSON support) to parse the JSON into your data structures. Ensure immutability (use `val`).

4. **Add Unit Tests**: Write tests to verify parsing works correctly (e.g., check if a tile has the right connections).

5. **Integrate**: Load the tiles into your app's data layer (e.g., a simple list or map) so they can be used by the game engine.

### What It Should Look Like Now
- Your code compiles without errors
- Running tests passes (e.g., parsing a sample tile returns the expected object)
- You can access tile data in the app (though no UI yet)

!!! tip
    Use Kotlin's null safety and data classes for clean, error-free code. Check in your solution for review.

---

[Previous: Task 3](task3.md) | [Next: Day 2](../day2/index.md)
