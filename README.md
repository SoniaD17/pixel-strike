# Pixel Strike

A retro-style browser-based shooter game built with vanilla HTML5 Canvas and JavaScript.

## Features

- **Female protagonist** with pixel art sprite
- **Arrow key controls** for movement
- **Spacebar shooting** with adjustable fire rate
- **3 progressive levels** with increasing difficulty
- **Enemy variety**: Walkers, Rushers, and Armored enemies
- **Powerup system**: Health, Rapid Fire, and Shield
- **Victory medal** upon completing all levels
- **Hi-score tracking** with localStorage persistence

## How to Play

1. Open `game.html` in your web browser
2. Press **Enter** to start the game
3. Use **Arrow Keys** to move
4. Press **Space** to shoot
5. Defeat enemies to progress through levels
6. Collect powerups for bonuses:
   - 🟥 Health - Restore 1 HP
   - ⚡ Rapid Fire - Double fire rate for 3 seconds
   - 🛡️ Shield - Block damage for 3 seconds

## Game Progression

- **Level 1**: Defeat 15 Walkers
- **Level 2**: Defeat 25 mixed enemies (Walkers + Rushers)
- **Level 3**: Defeat 35 armored enemies (Rushers + Armored)
- **Victory**: Earn a medal and rank based on performance

## Technical Details

- Pure vanilla JavaScript (no dependencies)
- HTML5 Canvas 2D rendering
- Fixed timestep game loop (60 FPS)
- Retro pixel art aesthetic
- Collision detection system
- Particle effects

## Files

- `game.html` - Main game file (all code in one file)
- `tictactoe.html` - Bonus: Tic Tac Toe game
