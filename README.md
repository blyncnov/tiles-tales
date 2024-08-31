# Tile Painter Game

## Overview

Tile Painter Game is a Vue.js project where users can interact with a 10x10 grid of tiles. The objective is to "paint" tiles by selecting them through double-clicking and hovering over adjacent tiles. The game includes a timer that adds an element of urgency, making it both a fun and challenging experience.

## Features

- **Interactive Grid**: The game features a 10x10 grid of tiles (100 tiles in total). Users can select tiles by double-clicking and hovering over adjacent tiles.
- **Selection Feedback**: When a tile is selected, it turns green. Hovering over a tile highlights it, and clicking finalizes the selection.
- **Countdown Timer**: The game has a built-in countdown timer, starting at 10 seconds. The timer adds urgency to the gameplay, encouraging players to act quickly.
- **Dynamic Alerts**: The game includes alerts based on the player's performance:
  - If time runs out, an alert notifies the player that time is up.
  - If the player paints all 100 tiles before time runs out, an alert congratulates them on their speed.

## How to Play

1. **Double-click** on a tile to start painting.
2. **Hover** over adjacent tiles (either horizontally or vertically) to highlight them.
3. **Click** to select the highlighted tiles and finalize the selection.
4. The **timer** starts automatically when the game begins. You have 10 seconds to paint as many tiles as possible.
5. If you paint all 100 tiles before time runs out, you’ll receive a congratulatory message. If the timer runs out, the game resets.

## Project Setup

To run the project locally:

```bash
npm install
npm run serve
```

## Code Explanation

### Components

- **Grid Component**: This component is responsible for rendering the grid of 100 tiles and handling user interactions like double-clicking, hovering, and clicking.

### State Management

- **selectedTiles**: A reactive reference array that stores the indices of the tiles that have been finalized (painted green).
- **hoveredBoxes**: A reactive reference array that temporarily stores the indices of tiles that are being hovered over (highlighted).
- **startTiles**: A reactive reference used to store the index of the tile that the user double-clicks to start painting.

### Timer Logic

- **timeLeft**: A reactive reference that tracks the remaining time in seconds.
- **formattedTime**: A computed property that formats `timeLeft` into MM:SS format.
- **startTimer**: A function that initializes the countdown timer when the page loads.
- **Watchers**: The project includes watchers that trigger alerts when time runs out or when the player paints all 100 tiles before the timer ends.

### User Interaction Methods

- **handleDoubleClick**: Initiates the painting process by storing the index of the tile that was double-clicked.
- **handleMouseOver**: Tracks tiles that the user hovers over, allowing them to be highlighted.
- **handleClick**: Finalizes the selection of highlighted tiles, turning them green.

## License

This project is open-source and available under the MIT License.
