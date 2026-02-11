# Task 4: Adding the game Tiles
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

In the solution provided with this course, we model tiles using two different approaches at the same time: one to track actual areas, and one to quickly check placement rules. You might want to model tiles differently—you're absolutely encouraged to think about your own approach.

!!! example "Task"
    Model the `Tile` data structure such that all game rules can be applied. (Or follow the approach provided below if you prefer.) 

#### Hybrid tile modeling


!!! example "Task"
    Add an enum class `Edge` to the `tile.data` package and add possible values according to the 

### Tile metadata assets

#### Json parser



---

[Previous: Task 3](task3.md) | [Next: Day 2](../day2/index.md)
