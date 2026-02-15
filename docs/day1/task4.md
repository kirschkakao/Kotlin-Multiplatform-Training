# Task 4: Adding game Tile assets
Now that everything is set up, we can start building the game itself. Before we touch any code, it helps to understand how Carcassonne works and what a tile represents.

!!! example "Task"
    Read and understand the game rules for the Carcassonne base game, pages 8-16 (you can find the PDF in the common resources).

This course focuses on implementing the base game without expansions. Still, we'll structure the app in a way that makes it easy to add expansions later, and we absolutely encourage you to explore that after the course if you feel like it.

### On assets and persistent data
There are two kinds of data that can outlive both closing the app and shutting down the machine it runs on: assets and persistent data.
**Persistent data** in software development refers to dynamic data that changes while using the app, such as emails, messages, or contacts in communication apps. This kind of persistence is usually handled through local databases stored on disk.
On the other hand, non-dynamic (static) data that ships with the app is referred to as **assets**. These can be texts, fonts (like those used in the menu bar), or textures.
In our Carcassonne game, the tile data is a typical example of an asset, while a leaderboard would be a typical example of persistent data.

### Tile assets
For Carcassonne we need two types of assets for the game tiles. One of them, the textures, were already shipped with your project files (see `CarcassonneTiles.jpg` in the common resources). The other is tile metadata and game-relevant information that describe what is actually shown on a tile, for example streets or castles and how they are arranged. This is needed to implement the logic that represents the game rules. For example, we need to know algorithmically where and in which orientation we can place a tile on the current board, where we can place followers, and how large our cities are or how long the streets are. With that in mind, we can now define the data structures that represent a tile in code.

## Add Tile data structures and assets
In this task, you will add all the objects needed to represent the game tiles of the base game in code.

### Tile data structure
First we need to add some data structures that define what a tile is and what the elements on a tile are. These types of objects are static data definitions and belong in the data layer. Since the data layer is currently not present in the tile package, we have to add it.

!!! example "Task"
    Add a package `data` to the `tile` package

You might notice that the data package appears inside the `presentation` package. This happens when only one sub package exists and the IDE collapses the complete package path into one. In that case, just rename the `data` package you have created and remove the "presentation" part from it, i.e. `tile.presentation.data -> tile.data`.

We begin with the tile data class. Right click on the `data` package and choose New -> Kotlin Class/File. Then choose "data class" from the list and add the name of the class, i.e. `Tile`. The IDE then adds the file with an empty data class.

Now let's think about what information we need to store for a single game tile. We need a texture connected to it, so add the property `texture` as a mutable variable of type `Any?` and initialize it to `null`. We don't have code that extracts the textures yet, so we use an unspecific type that can be null. We might want to specify it further later.

``` Kotlin
data class Tile(
    var texture: Any? = null
)
```

We also need some metadata like which expansion the tile belongs to, so that we can later choose expansions, and whether the tile is the starting tile, i.e. the one that sits on the board from the beginning.

!!! example "Task"
    Add meta data attributes to Tile for the expansion type and the starting tile flag. What data types should be used here?

While it is quite obvious how the starting tile flag should be modeled (boolean), there is more than one valid approach for the expansion type. In general, we want the value to allow only specific strings that are determined by the existing tile decks or expansions. So, just using a String as a data type would not suffice, since it allows all kinds of character sequences, which can lead to mistakes like different wordings for the same tile decks. The most straightforward solution to define a type that can only have specific string values in Kotlin is to use enums.

!!! example "Task"
    Add an enum class `Expansion` to the `tile.data` package and use it as data type for the expansion attribute of `Tile`. Initialize the expansion attribute as "basegame" and the starting tile attribute with `false`.

#### What is on the tile?

The most interesting and challenging part of designing the data structure for game tiles is answering: what can we see on a tile? Take a look at pages 16 and 55–66 of the game rules. One way to classify tiles is to simply look at their edges, since only the edges matter when deciding whether a tile can be placed in a given orientation.<br>
However, edge information alone doesn't tell us how the *areas* on those edges are organized. For example, the **CCFF** tiles (page 57 right) exist in three different versions in the base game — cities can be connected (one city on the tile) or disconnected (two separate cities). Plus, a city might have a pennant, which changes how scoring works.<br>
With farms it gets trickier still. Look at the **CCCR** tiles (page 57 left). The edge information we need for placement rules tells us nothing about the two distinct farm areas on the west side of the tile. So edge information is enough to enforce placement rules, but it's not enough to understand the actual area layout.

In the solution provided with this course, we model tiles using two different approaches at the same time: one to track actual areas, and one to quickly check placement rules. You might want to model tiles differently — you're absolutely encouraged to think about your own approach.

!!! example "Task"
    Model the `Tile` data structure such that all game rules can be applied. (Or follow the approach provided below if you prefer.) 

#### Hybrid tile modeling

Our hybrid approach uses two representations working together. First, we track the *connections* at each edge of the tile — this gives us everything we need to check placement rules quickly. Then, we separately track the *actual areas* inside the tile with their detailed topology — this lets us handle scoring and follower placement correctly.

Let's start with connections. Each edge of a tile (north, east, south, west) connects three possible terrain types: cities (C), roads (R), or farms (F).

!!! example "Task"
    Add an enum class `Connection` to the `tile.data` package with values `C`, `R`, and `F`. Then add a `connections` attribute to `Tile` of type `List<Connection>`, storing the four connections in order: north, east, south, west.

Now for the actual area distribution within the tile. An area consists of edges and corners, depending on the type. Roads connect only through edges, cities connect only through edges, but farms are trickier — they connect both through edges and corners. That said, when a farm connects over a corner (like the northeast corner), it implicitly includes the adjacent non-city edges. So we only need to track farm corners, not their edge connections.

!!! example "Task"
    Add an enum class `Edge` to the `tile.data` package with the four edges (cardinal directions). Then add an enum class `Corner` with the four corners (diagonal directions like north east). Add a data class `Areas` that holds three attributes: `cities`, `roads`, and `farms`, each as `List<List<...>>` with the appropriate edge or corner type.

Finally, we need to handle special features like cloisters and pennants.

!!! example "Task"
    Add an enum class `Feature` to the `tile.data` package with values for cloisters and pennants. Then add a `features` attribute to `Tile` of type `List<Feature>` (since a tile can have multiple features). Also add the `areas` attribute of type `Areas` to `Tile`.

### Tile data assets

Great! We now have a comprehensive data structure for game tiles. Your `Tile` class should look something like this:

```Kotlin
data class Tile(
    val expansion: Expansion = Expansion.BASEGAME,
    val connections: List<Connection>,
    val areas: Areas,
    val features: List<Feature>,
    var texture: Any? = null,
    val start: Boolean = false
)
```

Each tile in the game will be represented by an instance of this class. Now comes an important question: where does the data for all these tile instances come from? The base game has 72 tiles, each with its own unique combination of connections, areas, and features. We could hard-code all of this in Kotlin—creating 72 tile instances directly in our code—but that would be tedious, error-prone, and hard to maintain or extend later.

Instead, we'll store the tile information in a separate data file that our app reads when it starts. This is where assets come in. Since tiles never change during gameplay, we can define them once in a structured format and load them into our `Tile` instances. For this course, we're using JSON because it's human-readable, easy to edit, and straightforward to extend when you want to add expansions later. Think of the JSON file as a blueprint for all tiles — it describes what each tile looks like, and our code will transform those descriptions into usable objects.

#### JSON parser

To parse the tile asset JSON into instances of `Tile`, we'll use Google's Gson library. Take a look at your `build.gradle.kts` file—you'll see:

```Kotlin
implementation("com.google.code.gson:gson:2.8.9")
```

This means Gson is already available in your project. We don't need to install anything; we just need to understand how it works and structure our JSON so Gson can properly deserialize it into `Tile` objects.

#### Test-driven development

Before we create the full tile asset JSON, let's use test-driven development. We'll write unit tests that verify, step by step, that we can parse Areas, then Tiles, then lists of Tiles, and finally complete tile decks from different expansions.

!!! example "Task"
    Add a `GsonTest` class to the `commonTest/kotlin` package and create a `fluffAreas.json` file in a new `commonTest/resources` folder. (You may need to create the `resources` folder if it doesn't exist.)

Look at the existing `FreeFunctionTest.kt` to understand how Kotlin unit tests are structured and named. We'll follow the same principles for our JSON parser tests.

Let's start by testing `Areas` parsing. Add some sample data to `fluffAreas.json`—it doesn't have to represent real game areas, just valid structures:

```json
{
  "cities": [
    ["N", "E"],
    ["W"]
  ],
  "farms": [
    ["NE"],
    ["NW", "SW"]
  ],
  "roads": [
    ["S"]
  ]
}
```

!!! example "Task"
    Add a JSON-formatted `Areas` definition to `fluffAreas.json`.

Now write the corresponding unit test.

!!! example "Task"
    Add a unit test to `GsonTest` that verifies your JSON `Areas` definition is correctly parsed into an `Areas` instance.

Here's how to use Gson with resource streams:

!!! tip "Parsing with Gson"
    ```Kotlin
    val reader = InputStreamReader(stream)
    data = Gson().fromJson(reader, Areas::class.java)
    ```
    To get the resource stream, use `getResourceAsStream` with `let`:
    ```Kotlin
    this::class.java.getResourceAsStream("/fluffAreas.json")?.let { stream ->
        // parse here
    }
    ```

Your test assertion should match the structure of your JSON. For the example above:

```Kotlin
assert(
    Areas(
        cities = listOf(listOf(Edge.N, Edge.E), listOf(Edge.W)),
        farms = listOf(listOf(Corner.NE), listOf(Corner.NW, Corner.SW)),
        roads = listOf(listOf(Edge.S))
    ) == data
)
```

Run the test by clicking the green triangle next to the test method or test class.

!!! example "Task"
    Add test data and corresponding unit tests for:
    - A single `Tile` instance
    - A list of `Tile` instances
    - Tile decks with different `Expansion` types

!!! tip "Enum serialization with Gson"
    If Gson has trouble serializing enum values like `Expansion`, you may need to add a `@SerializedName("...")` annotation to specify how each enum value appears in JSON:
    ```Kotlin
    enum class Expansion {
        @SerializedName("basegame")
        BASEGAME,
        @SerializedName("expansion1")
        EXPANSION1
    }
    ```

#### Adding the tile assets

Now that you understand how to structure JSON for Gson to parse correctly, you can create the complete tile asset file defining all 72 tiles from the base game.

!!! example "Task"
    Create a `TileDescriptions.json` file in the same location as `CarcassonneTiles.jpg` (in `commonMain/resources`). Add a JSON structure for each of the 72 base game tiles, ordering them to match their position in the texture file. This will make it easier to connect each tile to its texture later.

---

[Previous: Task 3](task3.md) | [Next: Task 5](task5.md)
