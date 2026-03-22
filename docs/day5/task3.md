# Task 3: Scoring Areas and Logic

Now comes one of the most rewarding parts of the game: the scoring logic.

You have already built all the core gameplay flow, from tile placement to follower placement. In this final task, you will connect everything by implementing scoring areas according to the Carcassonne rules.

This is a more algorithmic challenge than the previous tasks, so treat it as a capstone. Take it step by step, validate often, and do not worry if it takes a few iterations.

## Using the Jump Start

If you want a substantial jump start, check out the `day5-task3-jumpstart` branch. It provides the required scoring area objects in the `scoringarea` package and extension functions for the `Edge` and `Corner` types that you need for the scoring logic.

You will find a common data type, `ScoringArea`, from which all specific area types (city, road, farm, cloister) inherit. These objects handle:

- extracting areas from tiles
- combining areas when tiles are added
- follower occupation logic
- scoring logic for completed areas

You will also find an `AreaRef` data class, which acts as a position reference for scoring areas by combining a coordinate with an `AreaSlot`.

The `ScoringAreaRepository` is the main interface for this part of the game and manages an internal area grid, represented as a mutable map from `AreaRef` to `ScoringArea`. The repository includes functions to:

- add tiles to the area grid
- retrieve takeable slots at a given coordinate
- take possession of an area at a specific slot
- calculate points for completed areas

This repository is ready to be injected into the `GameViewModel` so you can wire scoring areas into your gameplay flow.

Make the necessary frontend updates and see if you can bring the app to a fully playable Carcassonne version.

You are very close to the finish line. Enjoy the final stretch!



---

[Previous: Task 2](task2.md) | [Next: Summary](summary.md)