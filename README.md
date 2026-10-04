# CRANESWEEPER - Iteration Station: Crane Logic

A browser-based programmatic queue execution game designed to teach sequential logic, state management, and loop automation.

## Overview

Iteration Station is a single-file application requiring no external dependencies or build tools. 

The player acts as an automation programmer, constructing an instruction array to navigate an industrial gantry crane across an $8 \times 8$ grid. 

The crane must: 

- choose a path
- grapple cargo units
- drop them to a cargo train destination.

While keeping in mind both the path/move count and cargo collected impact the final score.

Naturally driving toward the goal of iterating to the most optimal path.

## Purpose
Teach iteration and efficiency early with a tangible exercise, has made explaining programming concepts to my kiddos much simpler. Inspired partially by minesweeper scoring mechanics with speed & the robot turtles board game for simple mechanics which have both helped tremendously with teaching iteration, loops, patterns, etc. 
## Architecture & Tech Stack

* **Runtime:** Any modern ECMAScript-compliant web browser.
* **Technology:** Vanilla HTML5, CSS3 (CSS Grid, Flexbox, CSS Variables), and JavaScript (ES6+).
* **Dependencies:** Google Fonts (Fira Code, Inter).
* **State Management:** Deterministic discrete-step simulation based on an in-memory command queue.

## Mechanics & Rules

* **The Grid:** An $8 \times 8$ procedural coordinate space ($r, c \in [0, 7]$).
  * **Empty Cell (0):** Navigable terrain.
  * **Wall (1):** Impassable barrier causing execution failure on collision.
  * **Container (2):** Cargo objective. Must be in the directly adjacent cell facing the crane to be grappled.
  * **Train (3):** Target drop zone. Must be directly adjacent facing the crane to receive cargo and trigger sequence completion.
* **Deterministic Reset:** Running a sequence resets the board and crane to their initial procedural state prior to sequential execution.
* **Failure Conditions:**
  * Gantry boundary breach (moving off the grid).
  * Structural collision (moving into a wall).
  * Invalid grapple vector (executing grab without cargo directly ahead).
  * Premature drop (executing drop with empty payload).
  * Unauthorized drop zone (executing drop when not directly facing the train target).

## Control Mapping

### Crane Commands

| Command | Hotkey | Operation |
| :--- | :--- | :--- |
| `left` | `←` / Left Arrow | Rotate crane $90^\circ$ counter-clockwise. |
| `right` | `→` / Right Arrow | Rotate crane $90^\circ$ clockwise. |
| `forward` | `↑` / Up Arrow | Translate crane 1 cell in current heading. |
| `grab` | `G` | Grapple container from the adjacent front cell. |
| `drop` | `D` | Offload container into adjacent train drop zone. |

### Queue Manipulation

* **Select / Deselect Command:** Left-click an item in `// COMMAND_QUEUE`.
* **Delete Command:** Right-click an item in `// COMMAND_QUEUE`.
* **Loop Selected (x2 / x3):** Duplicates the selected sequence inline to simulate iteration blocks.
* **Purge Queue:** Clears all queued operations and resets the board to baseline state.
* **Trajectory Preview:** When enabled, computes and renders deterministic movement paths ahead of execution.

## Scoring Model

Score computation occurs upon successful drop delivery to the target train:

$$\text{Score} = \max(0, (\text{Loaded Cargo} \times 1000) - (\text{Move Count} \times 10))$$

Efficiency demands minimizing rotational and translational operations while maximizing cargo delivery.

## Output Schema

The runtime visualizes the execution sequence in real time as a structured JSON object:

```json
{
  "sys_id": "CRANE_OP_01",
  "length": 0,
  "sequence": [
    "forward",
    "right",
    "forward",
    "grab",
    "left",
    "forward",
    "drop"
  ]
}
```

## Deployment & Execution

1. Save the source code locally as `index.html`.
2. Open `index.html` directly in any standard desktop web browser:

```bash
open index.html        # macOS
xdg-open index.html   # Linux
start index.html       # Windows
```
