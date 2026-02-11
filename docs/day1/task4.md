# Task 4: Tile assets and parser

### On assets and persistent data
There are two kinds of data that can outlive both closing the app and shutting down the machine it runs on: assets and persistent data.
**Persistent data** in software development refers to dynamic data that changes while using the app, such as emails, messages, or contacts in communication apps. This kind of persistence is usually handled through local databases stored on disk.
On the other hand, non-dynamic (static) data that ships with the app is referred to as **assets**. These can be texts, fonts (like those used in the menu bar), or textures.
In our Carcassonne game, the tile data is a typical example of an asset, while a leaderboard would be a typical example of persistent data.

### Tile assets
For Carcassonne we need two types of assets for the game tiles. One of them, the textures, were already shipped with your project files (see `CarcassonneTiles.jpg` in the common resources). The other is tile metadata that describes what is actually shown on a tile, for example streets or castles and how they are arranged. This is needed to implement the logic that represents the game rules. For example, we need to know algorithmically where and in which orientation we can place a tile on the current board, where we can place followers, and how large our cities are or how long the streets are.

## Add Tile data structures and metadata assets
In this task, you will add all necessary objects needed to represent the game tiles of the base game in the code. 

### Tile data structure
First we need to add some data structures that define what a tile is and what the elements on a tile are. These type of objects are static data type definitions and belong into the data layer. Since the data layer is currently not present in the tile package, we have to add it.
!!! example "Task":
    add a package `data` to the `tile`package


### Tile metadata assets

#### Json parser



---

[Previous: Task 3](task3.md) | [Next: Day 2](../day2/index.md)
