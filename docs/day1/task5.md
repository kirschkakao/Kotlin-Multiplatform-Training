# Task 5: Creating a pseudo Database

In the last task, we created all the data structures needed to represent tiles in code and familiarized ourselves with the Gson parser before adding the data assets for all 72 base game tiles.<br>
In this task, we'll build the pseudo database that loads the tiles and manages both the complete tile catalog and the active draw pile for the current game—and later on, player data as well. As described in task 3, a pseudo database mimics real database behavior using simple Kotlin collections.

## The database singleton

In production applications, you need a reliable way to manage data throughout the app's lifecycle. The database is responsible for loading, storing, and providing access to all persistent data—and in our case, that includes the tile assets we created in the last task. Since a database typically manages resources like file handles, memory buffers, or network connections, it's crucial that we don't accidentally create multiple instances of it. Having multiple database objects could lead to inconsistent state, resource conflicts, or unnecessary memory overhead. That's why database classes are prime candidates for the singleton pattern—a design pattern that guarantees exactly one instance of a class exists throughout the entire application. This way, every part of our code accesses the same database instance, ensuring consistency and efficient resource usage.

!!! example "Task"
    Create a `Database` class that implements the singleton pattern. Don't add any data or methods yet. If you're not sure how to implement singletons in Kotlin, check chapter 6 of the Kotlin introduction from task 1. Think carefully about where this class belongs in your project structure—recall the layer and package architecture we introduced in task 3 and consider which layer is responsible for data access and management.

### Adding tables

Now we add the "tables" to our pseudo database. For tiles, we need two of them. The first holds all available tiles from the base game and any expansions—think of it as the complete tile catalog. The second represents the draw pile for the current game session, containing only the shuffled tiles that players actually draw from during this particular game.

!!! example "Task"
    Add two private attributes to the `Database` class: one for the tile catalog (all available tiles) and one for the draw pile (tiles in the current game). Choose suitable data types for each.

These attributes are private because, in well-structured architecture, the actual database and table objects aren't directly visible outside the `Database` class. Instead, they're accessed through DAOs (see task 3).

### Table initialization

Next, we need a tile initialization method that loads the tile data (and later on, textures) from our assets.

!!! example "Task"
    Add a private initialization method that loads the tile data from your JSON asset file into the tile catalog.

To call the initialization method once the database object is created, we use an init block.

!!! example "Task"
    Add an init block that calls your initialization method and prints a message confirming that the database was successfully initialized.

### Table access

Finally, we need a data access object (DAO) for our tile "tables". DAOs are initialized alongside the database object and define basic getter and setter methods for specific tables. These methods correspond to basic SQL queries like SELECT, DELETE, and INSERT, and sometimes more complex queries with filters, joins, and so on. For example, a DAO for a contact request table in a messaging app for Android could look like this:

```Kotlin
@Dao
interface ContactRequestDao {
    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(contactRequest: ContactRequest)

    @Update
    suspend fun update(contactRequest: ContactRequest)

    @Delete
    suspend fun delete(contactRequest: ContactRequest)

    @Query(
        """
        SELECT *
        FROM contact_request
        WHERE direction = :direction
        """
    )
    fun getRequestStream(direction: String): Flow<List<ContactRequest>>

    @Query(
        """
        SELECT COUNT(1)
        FROM contact_request
        WHERE direction = :direction
        """
    )
    fun getNumberOfIncomingRequests(
        direction: String = ContactRequestDirection.INCOMING.value()
    ): Flow<Int>

}
```

In our case, where we don't actually connect to a database with real tables, we could in principle skip the DAOs and implement repositories that directly use the database object. However, we'll mimic the usual behavior so you understand what these objects are for and where they're typically located in a real codebase.

#### The tile DAO

While the `Database` class lives in the core package (because it's relevant across the entire codebase), DAOs belong to the data layer of the specific package whose tables they access.

!!! example "Task"
    Add a class `TileDAO` to the `tile.data` package with two private constructor parameters: one for the tile catalog and one for the draw pile.

When building a codebase, we normally can't predict exactly which methods the DAO will need, so we add them as we go. For this course, though, we'll assume we already know we need the following methods:

- `count()` - returns the number of tiles left in the draw pile
- `clear()` - empties the draw pile
- `initialize(expansions: List<String>)` - populates the draw pile with all tiles from the catalog that belong to the specified expansions (e.g., basegame, expansion1, expansion2)
- `popAt(idx: Int): Tile?` - removes and returns the tile at the given index from the draw pile
- `getExpansions(): Set<String?>` - returns a set of all available expansion names in the tile catalog
- `getStart(): Tile?` - returns the starting tile from the draw pile

!!! example "Task"
    Add the methods listed above to your `TileDAO` class.

The tile DAO is now ready and just needs to be connected to the `Database` class.

!!! example "Task"
    Add a `tileDao` attribute of type `TileDAO` to the `Database` class. Make it accessible from outside but not writable. Initialize the DAO in the init block, passing in the tile catalog and draw pile.

Now you're all set. Your database can be instantiated anywhere in the code via the singleton accessor, and the DAO can be retrieved to access the tables through its methods.

In the coming days, you'll learn how DAOs are used in repositories to provide database- and table-independent methods to the domain layer, and how all these objects are wired together using manual dependency injection.

---

[Previous: Task 4](task4.md) | [Next: Summary](summary.md)

