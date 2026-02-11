# Task 4: Tile assets and parser

### On assets and persistent data
There are two types of data that lives through both, the closing of the app and the shutdown of the machine on which the app runs as well, assets and persistent data.
**Persistent data** in software development relates to dynamic data which changes while using the app, e.g. emails, messages and contacts in communication apps. This type of persistence is usually achieved through local databases which are stored on the hard disk. 
On the other hand, non-dynamical i.e. static data that is shipped with the app is referred to as **assets**. Those can be texts and fonts of the app itself (like of the menu bar) or textures.<br>
In our Carcassonne Game the tile-data is an typical example of assets, while a leaderboard for example would be an typical example of persistent data.

### Tile assets
For Carcassonne we need two types of assets for the game tiles. One of which, the textures, were already shipped with your project files (see `CarcassonneTiles.jpg` in the common resources). The other one is tile metadata which describes what's actually shown on a tile, e.g. streets or castles and how they are arranged. This is needed in order to implement the logic that represent the game rules. For example, we need to now algorithmicly where and in which oriontation we can place a tile on the current board, were we can place followers and how big our cities or how long the streets are.

## Add Tile data structures and metadata assets

### Tile data structure

### Tile metadata assets

#### Json parser



---

[Previous: Task 3](task3.md) | [Next: Day 2](../day2/index.md)
