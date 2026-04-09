# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains two standalone browser-based games:
- **Pixel Strike** (`game.html`) – A retro-style bullet-hell shooter with 3 progressive levels
- **Tic Tac Toe** (`tictactoe.html`) – A classic two-player game with score tracking

Both games are vanilla HTML5 with no external dependencies.

## Running the Games

1. Open `game.html` or `tictactoe.html` directly in any modern web browser
2. No build step, server, or npm install required
3. localStorage is used for persistent hi-score in Pixel Strike

## Git Workflow & Version Control

**Commit regularly and push to GitHub after completing meaningful work.** Each commit should be a logical unit of work with a clean, descriptive message. This preserves project history and ensures no work is lost between sessions.

### Commit Guidelines
- **Frequency**: Create a commit after each feature, bug fix, or meaningful change
- **Message format**: Start with a verb (Add, Fix, Update, Refactor), be specific about what changed
- **Examples**:
  - `Add level 4 with new enemy type`
  - `Fix collision detection for armored enemies`
  - `Refactor particle system for better performance`
  - `Update CLAUDE.md with git workflow guidelines`
- **Push immediately after committing**: Use `git push origin master` to sync with GitHub
- **Avoid**: Large, vague messages like "changes" or "update" without context

### Before Starting Work
- Run `git status` to see current state
- Create a new branch if working on a substantial feature: `git checkout -b feature/name`

### When Work is Complete
1. Review changes: `git status` and `git diff`
2. Stage files: `git add <files>` or `git add .`
3. Commit with message: `git commit -m "Description of changes"`
4. Push to remote: `git push origin master` (or your branch name)

This ensures the repository always reflects the current state of work and provides a clear audit trail.

## Pixel Strike Architecture

### Game Loop & Rendering
- **Fixed timestep**: 60 FPS with accumulator pattern (line 635-642)
- **Canvas size**: 800×400 pixels
- Separation of update (line 293) and render (line 626) logic
- Uses `requestAnimationFrame` for smooth cross-browser animation

### Sprite System
- **Palette-based sprites**: Color palette (PAL) maps single characters to hex colors (line 18-36)
- Sprites stored as 2D character arrays representing pixel grids at 4×4 pixel scale (T=4)
- `drawSprite()` converts palette indices into filled rectangles on canvas (line 38-49)
- All sprites defined inline: PLAYER (2 walk frames), WALKER, RUSHER, ARMORED (shielded variant), ARMORED_S, powerups, hearts

### Text Rendering
- Custom 5×7 bitmap font (line 52-90) for retro aesthetic
- Text scale independent of sprites (line 92-102)

### Game State Management
- Single global `gs` object tracks: screen (MENU/PLAYING/LEVEL_CLEAR/VICTORY/GAME_OVER), level, score, player, enemies, bullets, particles, powerups, spawner state
- Level config in LEVELS array (line 247-251) defines kill requirements, spawn intervals, max enemies, enemy mix per level

### Collision Detection
- **Bullet-enemy**: Rectangle overlap check (line 356-377)
- **Enemy-player**: Rectangle overlap with invincibility frames and shield absorption (line 382-405)
- **Powerup-player**: Simpler overlap, applies effect and removes powerup (line 416-427)
- Collision rectangles sized per entity type (armored enemies wider)

### Enemy Spawning & Difficulty
- Spawner tracks: timer, spawn interval, max on-screen, total spawned limit
- Enemy types: walker (1 hp, slowest), rusher (1 hp, medium), armored (2 hp, slowest with shield until first hit)
- Animation: 10-frame cycles for enemy walk, player walk is 8-frame
- Enemies scored 10/20/30 points per type; powerups drop at 20% chance on kill

### Powerup System
- Three types: health (+1 hp, max 3), rapid fire (2× fire rate for 3 sec), shield (absorb 1 hit for 3 sec)
- Powerups bob and fade as they fall (line 514-525)
- Shield reduces hit damage taken by reducing rapidFireT (line 391)

### Particle Effects
- Spawned at collision points with random velocity spread
- Lifetime decay with alpha fade (line 527-533)
- Capped at 100 particles; oldest removed when limit exceeded (line 289)

### Key Constants
- GROUND = 340 (player/enemy y-plane)
- PLAYER_SPEED = 3 pixels/frame
- BULLET_SPEED = 9 pixels/frame
- SHOOT_CD = 15 frames (base), halved with rapid fire

### Hi-Score Persistence
- Stored in localStorage as `pxhi`
- Updated on level 3 complete or game over (whichever is higher)
- Rank calculation: S (0 deaths), A (1), B (2), C (3+)

## Common Modifications

### Adding a New Enemy Type
1. Define sprite in WALKER/RUSHER format (character palette grid)
2. Add to LEVELS mix arrays
3. Update collision width/height calculation (line 360, 387)
4. Add speed multiplier in spawn logic (line 344)

### Adjusting Difficulty
- Modify LEVELS array: killsReq (enemies to defeat), spawnInt (frames between spawns), maxOnScreen
- Enemy speeds use 0.9-1.2× multiplier (line 344)

### Changing Canvas Size
- Update W, H constants (line 15) and canvas element width/height attributes
- GROUND position may need adjustment
- HUD positioning uses W/2 and W anchors; should scale reasonably

### Adding Particle Effects
- Call `spawnParts(x, y, colorHex, particleCount)` (line 284-290)
- Default spread: random angle, 1-4 pixels/frame velocity, 20-30 frame lifetime
