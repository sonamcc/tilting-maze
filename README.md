# Tilting Maze Game

Welcome to the Tilting Maze Game! This interactive game, built with JavaScript, HTML, and CSS, allows players to control balls through a maze using a joystick. This README covers setup, mechanics, customization, game logic, and the math used in the code.

## Table of Contents

- [Overview](#overview)
- [File Structure](#file-structure)
- [Game Elements](#game-elements)
- [How It Works](#how-it-works)
- [Game Logic and Mathematics](#game-logic-and-mathematics)
- [Customization](#customization)
- [Running the Game](#running-the-game)
- [Future Enhancements](#future-enhancements)

## Overview

In this game, players control a joystick to guide small balls through a maze, avoiding walls and obstacles, until they reach the designated end point. Pressing "H" activates a Hard Mode, adding extra obstacles.

## File Structure

- `index.html`: Defines the HTML structure of the game interface.
- `style.css`: Manages styling of game elements.
- `script.js`: Contains JavaScript code for game logic, including animations and interactions.

## Game Elements

### HTML (index.html)

The HTML structure includes the main sections like `#center`, `#game`, `#maze`, `#end`, `#joystick`, and `#note`, which together create the game environment.

### CSS (style.css)

The CSS file styles each element with custom colors, layouts, and animations for a responsive design.

### JavaScript (script.js)

The JavaScript file defines the game’s mechanics, such as joystick controls, ball movement, collision detection, and Hard Mode activation.

## How It Works

1. **Initialization**: When the page loads, the JavaScript initializes game elements, maze layout, and event listeners for joystick and keyboard input.
2. **Joystick Interaction**: Players click and drag the joystick to move the balls in the maze.
3. **Collision Detection**: JavaScript checks if balls hit walls or obstacles, stopping them from passing through.
4. **Target Detection**: When all balls reach the target area, the game registers a win.
5. **Hard Mode**: Pressing "H" adds obstacles for extra difficulty.

## Game Logic and Mathematics

The JavaScript file (`script.js`) uses various mathematical principles to implement joystick movement, collision detection, and win conditions.

### Joystick Movement and Vector Calculations

The joystick controls rely on vector calculations to determine the movement direction and speed of the balls.

**Code Reference**:
In `script.js`, the joystick event listener uses the following code to calculate movement:

```javascript
const deltaX = joystickCurrentX - joystickStartX;
const deltaY = joystickCurrentY - joystickStartY;
const angle = Math.atan2(deltaY, deltaX);
const speed = Math.min(10, Math.sqrt(deltaX ** 2 + deltaY ** 2) / 10);
```

#### Explanation

1. **Direction Calculation**:
   - `angle = Math.atan2(deltaY, deltaX);`
   - This line calculates the angle (in radians) between the initial and current joystick positions, using `Math.atan2` to consider both `x` and `y` deltas. This angle represents the direction the balls will move.

2. **Speed Calculation**:
   - `speed = Math.min(10, Math.sqrt(deltaX ** 2 + deltaY ** 2) / 10);`
   - This line calculates the movement speed based on the Euclidean distance between the joystick's start and current position:
   
     \[
     \text{speed} = \sqrt{(\Delta x)^2 + (\Delta y)^2}
     \]
     
   - `Math.min(10, ...)` caps the speed to a maximum value (here, `10`) for controlled movement, preventing balls from moving too quickly when the joystick is pulled far.

3. **Applying Movement Vector**:
   The movement vector is then applied to the ball positions:
   
   ```javascript
   ball.x += Math.cos(angle) * speed;
   ball.y += Math.sin(angle) * speed;
   ```
   
   Here, `Math.cos(angle)` and `Math.sin(angle)` extract `x` and `y` components of movement, scaling them by `speed`. This allows the ball to move in the correct direction and speed based on joystick input.

### Collision Detection and Resolution

The game uses geometric principles to detect and prevent balls from moving through walls or obstacles.

**Code Reference**:
In `script.js`, collision detection checks the balls' position against each wall:

```javascript
walls.forEach(wall => {
  if (ball.x + ballRadius > wall.x && ball.x - ballRadius < wall.x + wall.width &&
      ball.y + ballRadius > wall.y && ball.y - ballRadius < wall.y + wall.height) {
    // Stop ball movement
    ball.velocityX = 0;
    ball.velocityY = 0;
  }
});
```

#### Explanation

1. **Boundary Detection**:
   - The code checks if any part of the ball overlaps with any part of a wall. For each wall, it verifies if the ball’s boundaries exceed the wall’s boundaries in both `x` and `y` directions.

2. **Stopping on Collision**:
   - When a collision is detected, the ball’s velocity components (`ball.velocityX` and `ball.velocityY`) are set to zero. This prevents the ball from moving in the direction of the collision, simulating a wall impact.

### End Point and Goal Detection

The game checks if each ball is within the `#end` target area, which triggers a win condition.

**Code Reference**:
In `script.js`, this code determines if a ball is inside the end zone:

```javascript
const distanceToEnd = Math.sqrt((endX - ball.x) ** 2 + (endY - ball.y) ** 2);
if (distanceToEnd < endRadius) {
  ball.inEndZone = true;
}
```

#### Explanation

1. **Proximity Check**:
   - The distance from each ball to the center of the `#end` area is calculated using:

     \[
     \text{distanceToEnd} = \sqrt{(\text{endX} - \text{ball.x})^2 + (\text{endY} - \text{ball.y})^2}
     \]

   - If `distanceToEnd` is smaller than `endRadius`, the ball is considered inside the target.

2. **Win Condition**:
   - When all balls have `inEndZone` set to `true`, the game registers a win.

## Customization

You can adjust colors, maze layout, and ball properties to customize the game.

### Colors

In `style.css`, modify the following CSS variables for a new color scheme:

```css
:root {
  --background-color: #ede6e3;
  --wall-color: #36382e;
  --joystick-color: #210124;
  --joystick-head-color: #f06449;
  --ball-color: #f06449;
  --end-color: #7d82b8;
  --text-color: #210124;
}
```

### Maze Layout

Modify the maze layout by adjusting `#maze` in `index.html` and `.wall` elements in `script.js`.

## Running the Game

1. Download and unzip the project files.
2. Open `index.html` in a web browser.
3. Follow on-screen instructions to start the game.

## Future Enhancements

- **Improved Collision Detection**: Smoother, more accurate collisions.
- **Levels**: New maze layouts for progressively harder levels.
- **Sound Effects**: Audio feedback for collisions, wins, or mode changes.
