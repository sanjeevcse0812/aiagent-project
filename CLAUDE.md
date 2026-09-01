# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains a **Snake Game** built as a single-file HTML5 application (`index.html`). It's a classic arcade game with modern features including touch/swipe support for mobile, high score persistence, pause/resume functionality, and responsive design.

## Project Structure

```
.
├── index.html          # Complete Snake game (HTML + CSS + JS in one file)
├── quiz.html           # Class 1 educational puzzle game (Math/English/Science)
└── CLAUDE.md           # This documentation file
```

## Development Commands

Since this is a static HTML5 game, no build tools or package managers are required:

- **Run locally**: Open `index.html` directly in a browser, or serve with any static server:
  ```bash
  # Python 3
  python -m http.server 8000
  
  # Node.js (npx serve)
  npx serve
  
  # PHP
  php -S localhost:8000
  ```
- **Test**: Open in browser and play - no automated test suite exists
- **Lint/Format**: Not applicable (vanilla HTML/CSS/JS)

## Architecture

### Game Configuration (lines 201-206)
- `GRID_SIZE = 20` - Size of each grid cell in pixels
- `CANVAS_SIZE = 400` - Canvas dimensions (400x400)
- `CELL_COUNT = 20` - Grid is 20x20 cells
- `INITIAL_SPEED = 150` - Initial game loop interval in ms

### Game State (lines 207-217)
- `snake` - Array of segment objects `{x, y}`
- `direction` / `nextDirection` - Current and queued movement vectors
- `food` - Current food position `{x, y}`
- `score` / `highScore` - Current and best scores (high score in localStorage)
- `gameLoop` - setInterval reference
- `isRunning` / `isPaused` - Game state flags
- `currentSpeed` - Dynamic speed that increases with score

### Core Functions
| Function | Purpose |
|----------|---------|
| `init()` | Reset game state, spawn snake at center, spawn first food |
| `spawnFood()` | Place food at random empty cell |
| `update()` | Main game loop: move snake, check collisions, handle food |
| `draw()` | Render grid, food, snake with eyes on head |
| `gameOver()` | Stop loop, update high score, show overlay |
| `startGameLoop()` / `stopGameLoop()` / `restartGameLoop()` | Interval management |
| `startGame()` / `togglePause()` | Game flow control |
| `handleKeydown()` / `handleTouchStart()` / `handleTouchEnd()` | Input handling |

### Key Features
1. **Keyboard controls**: Arrow keys or WASD, Space to pause
2. **Touch/swipe support**: Swipe on canvas for mobile (lines 478-502)
3. **High score persistence**: localStorage (line 213, 386-389)
4. **Dynamic difficulty**: Speeds up every 50 points (lines 291-294)
5. **Visibility handling**: Auto-pauses when tab hidden (lines 527-531)
6. **Responsive design**: CSS clamp() for text, CSS variables for theming
7. **Dark/light mode**: Automatic via `prefers-color-scheme` media query

## Code Style Notes
- Single file, no modules - all code in `<script>` tag
- CSS custom properties for theming
- `ctx.roundRect()` used for snake segments (modern Canvas API)
- Event listeners use passive touch for performance
- 180-degree turn prevention in input handling

## Class 1 Educational Puzzle Game (`quiz.html`)

A mobile-first quiz game for Class 1 students (Math / English / Science).

### Structure
- `SUBJECTS` object (lines ~390-440) — three subjects, each with a question bank
- `loadProgress()` / `saveProgress()` — persist per-subject best score & stars in `localStorage` under `class1QuizProgress`
- `startQuiz()` / `showQuestion()` / `handleAnswer()` / `finishQuiz()` — quiz flow
- 10 random questions per round (shuffled choices, shuffled question order)

### Key Features
1. **Three subjects**: Math (counting & simple sums), English (letters, rhymes, words), Science (animals, nature)
2. **Mobile-first design**: big tappable choice buttons, `user-scalable=no`, `100dvh`, responsive grid
3. **Progress saving**: best score and star rating (1–3 ⭐) persisted per subject in localStorage
4. **Star ratings**: 90%+ = 3 stars, 60%+ = 2 stars, 40%+ = 1 star
5. **Dark/light mode**: automatic via `prefers-color-scheme` media query, matching the Snake game's palette
6. **Feedback**: correct/wrong color flash, encouraging praise messages
7. **Reset progress** link on the home screen (with confirm dialog)

## Future Development