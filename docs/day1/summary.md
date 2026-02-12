# Day 1: Summary

## What You've Accomplished Today

You've built the foundational architecture for your Carcassonne game. You started by setting up Android Studio and getting comfortable with Kotlin, learning the basics of the language while exploring a real Kotlin Multiplatform project structure. Then you dove into architecture, understanding Google's three-layer model and how vertical slices keep features organized and independent.

But the real substance of today came when you defined what a tile *is*. You designed a comprehensive `Tile` class that captures what a tile looks like programatically. You learned that static game data belongs in assets, and figured out how to load all that data from JSON using Gson. By the end of the day, you had a complete tile system — enums, data classes, unit tests, and the foundation for 72 game tiles.

---

## What's Coming Tomorrow

You'll start by learning practical patterns like DAOs and repositories that keep your code clean and your data layer separate from everything else. You'll build pseudo-database structures for tile decks and player data so your game can organize game relevant information.<br>
Then comes Jetpack Compose, the modern, intuitive way to build user interfaces in Kotlin. You'll create screens that actually display your tiles, and most importantly, you'll implement rotation — not just spinning the image, but intelligently rotating the connections and areas so the game logic knows what's what. By the time Day 2 ends, you'll have an app that loads your Tiles, lets you draw tiles from the current deck and rotate them in the frontend.

---

[Previous: Task 4](task4.md) | [Next: Day 2](../day2/index.md)
