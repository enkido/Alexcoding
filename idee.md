# Game Concept: Procedural Neon Arcade

## 1. Core Overview

A complete, browser-based, client-only arcade game heavily inspired by Pac-Man. The game runs immediately without server dependencies, featuring a procedural neon aesthetic, responsive design, and classic arcade mechanics refined with modern visual polish.

## 2. Technical Foundation

- **Platform:** HTML5 Canvas and vanilla JavaScript (Client-only).
- **Game Loop:** Utilizes `requestAnimationFrame` with delta time to ensure consistent movement speeds across different monitor refresh rates.
- **Rendering:** Procedurally drawn elements (no static image assets or ASCII grids).
- **Viewport:** The canvas dynamically fills the browser window. The maze is centered and scaled automatically based on available space, eliminating fixed tile sizes and reserving edge space for the Heads-Up Display (HUD).
- **Stability:** Strict avoidance of memory leaks or performance drops; particle systems are capped to maintain stable framerates. No entity clipping, getting stuck, or invalid memory states.

## 3. Procedural Maze Generation

- **Dimensions:** Compact grid, approximately 21x21 tiles.
- **Symmetry:** The generated layout must be horizontally symmetrical.
- **Connectivity:** The maze must be fully connected with no isolated sections, unreachable pellets, or dead-ends that break gameplay.
- **Algorithm:** Implement Wave Function Collapse (WFC) to procedurally generate the layout.
- **Tile Types:** Wall, Path, Pellet, Power Pellet, Ghost Spawn (Central House), Player Spawn, Fruit Spawn.

## 4. Game States & Flow

- **States:** Title Screen, Playing, Paused, Life Lost, Level Complete, Game Over.
- **Interface:** Title and Game Over screens must display clear instructions.
- **Controls:** Support for both Keyboard (arrows/WASD) and Mobile (swipe gestures/on-screen controls).
- **Progression:** 3 initial lives. Completing a level regenerates the maze or resets the entities with increased difficulty (faster ghosts, shorter power mode).

## 5. Entity Mechanics

### The Player (Neon-Man)

- **Movement:** Smooth tile-to-tile interpolation.
- **Input:** Queued turns (inputting a direction slightly before reaching an intersection automatically turns the player when valid).
- **Restrictions:** No diagonal movement; strict collision detection to prevent clipping.
- **Wraparound:** Side tunnels allow seamless teleportation from one edge of the screen to the opposite side.
- **Death:** Triggers a reset of entity positions upon life loss.

### The Ghosts (4 Entities)

- **Spawning:** Start in a central "ghost house" and exit sequentially with timed delays.
- **AI Behavior:** Simple grid-based pathfinding. Each ghost possesses a distinct personality/target logic (e.g., direct pursuit, interception, random wandering).
- **Movement Rules:** Ghosts must only move on valid paths and can never freeze or get stuck in loops.

## 6. Gameplay & Scoring System

- **Base Scoring:** Pellets (+10 points), Power Pellets (+50 points).
- **Bonus Items:** Fruit spawns temporarily near the center (+500 points).
- **Combo System:** Rapid, uninterrupted collection of pellets builds a score multiplier.
- **Power Mode:** - Triggered by eating a Power Pellet.

- Duration ranges from ~8 seconds to a minimum of 3 seconds (decreases as levels progress).
- Ghosts become vulnerable (edible), moving slower and fleeing.
- Eaten ghosts grant chain scoring (200 -> 400 -> 800 -> 1600 points) and return to the central spawn house to regenerate.
- **Persistence:** High scores are saved locally using browser `localStorage`.

## 7. Visuals & Polish

- **Aesthetic:** Dark background with vibrant, glowing neon lines and shapes.
- **Effects:** Smooth animations for entity movement, particle explosions for eating ghosts/fruit, and screen transitions for level completion.
- **HUD (Heads-Up Display):** Cleanly positioned on the edges, displaying Current Score, High Score, Remaining Lives, Current Level, Combo Multiplier, and a visual Power Mode Timer.

### Glossary of Technical Terms

- **Client-only:** Software that runs entirely on the user's local machine (browser) without needing to communicate with a remote server during gameplay. Example: A JavaScript game that doesn't need to fetch data to function.
- **Delta Time:** The amount of time that has passed since the last frame was rendered. Used to calculate movement independent of frame rate. Example: Moving 50 pixels per second, rather than 1 pixel per frame.
- **Procedural Generation:** Creating data algorithmically rather than manually. Example: Generating a random but valid maze layout using code instead of drawing it by hand.
- **requestAnimationFrame:** A browser API that tells the browser you wish to perform an animation and requests that the browser call a specified function to update an animation before the next repaint.
- **Wave Function Collapse (WFC):** An algorithm utilized in procedural generation that determines the state of a grid cell based on the strict rules and constraints of its neighboring cells. Example: A rule stating a "wall" tile cannot be placed adjacent to a "ghost spawn" tile.