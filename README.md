# Isometric Tower Defense

A real-time strategy tower defense game with isometric graphics, built from scratch in Python using Pygame. Features dynamic AI pathfinding, spatial grid optimization, and round-based survival gameplay.

## ✨ Features

### Core Gameplay
- **Isometric 3D Rendering**: Custom projection system for pseudo-3D perspective
- **Round-Based Survival**: Progressive difficulty with increasing enemy waves
- **Strategic Tower Placement**: Build defensive structures during gameplay
- **Soldier Deployment**: Spawn mobile units to defend your castle
- **Main Castle Defense**: Protect the central fortress with shield mechanics

### AI Systems
- **Zombie AI**: Autonomous pathfinding, target prioritization, collision avoidance
- **Soldier AI**: Automatic enemy tracking, ranged combat, formation behavior
- **Tower AI**: Automatic targeting within attack range
- **Target Prioritization**: Enemies choose between soldiers and towers dynamically

### Technical Features
- **Spatial Grid Optimization**: Efficient collision detection using spatial partitioning
- **Separation Steering**: Flocking behavior prevents entity clustering
- **Dynamic Audio System**: Background music with seamless victory/defeat track integration
- **Zoom & Pan Controls**: Free camera movement with scaling
- **Health & Shield Systems**: Multiple damage layers with visual feedback

## 📋 Requirements

- Python 3.7+
- Pygame
- NumPy (optional, for enhanced math operations)

## 🚀 Installation

```bash
# Clone or download the project
cd tower-defense-game

# Install dependencies
pip install pygame

# Run the game
python main.py
```

## 🎮 Controls

| Input | Action |
|-------|--------|
| **W / A / S / D** | Pan camera up / left / down / right |
| **Mouse Wheel Up/Down** | Zoom in / out |
| **B** | Toggle Build Mode (place towers) |
| **F** | Toggle Spawn Mode (deploy soldiers) |
| **1-6** | Select tower type (in Build Mode) |
| **Mouse Wheel** | Rotate tower (in Build Mode) |
| **Left Click** | Place tower/soldier or select tile |
| **Space** | Restart game (Game Over) / Next round (Victory) |

## 🏗️ Project Structure

```
.
├── main.py              # Entry point and game loop
├── config.py            # Game constants and settings
├── Renderer.py          # Isometric rendering engine
├── AIManager.py         # AI behavior for all entities
├── Audio.py             # Music and sound management
├── Enemy.py             # Zombie entity class
├── Soldier.py           # Soldier entity class
├── Tower.py             # Tower/Castle entity class
├── Spatial.py           # Spatial grid for collision optimization
└── res/                 # Resources (not included)
    ├── Zombie/         # Enemy sprite sheets
    ├── Soldier/        # Soldier animations
    ├── castle/         # Tower textures
    ├── music/          # Background tracks
    ├── sound/          # Sound effects
    └── ...
```

## 🎯 Game Mechanics

### Round System

**Setup Phase:**
- Each round grants you placement tokens
- Build towers before enemies spawn
- Deploy soldiers strategically

**Combat Phase:**
- Zombies spawn at map edges and attack
- Towers auto-target enemies in range
- Soldiers actively hunt zombies
- Survive all waves to advance

**Victory Conditions:**
- Eliminate all zombies → Next round
- Lose main castle → Game Over

### Entity Types

**Main Castle (Spawn: Center)**
- HP: 2000
- Shield: 1000 (regenerates each round)
- Damage: 120
- Special: Loses shield before HP damage

**Towers (Buildable)**
- HP: 500
- Damage: 100
- Range: Configurable radius
- Cost: 1 build token per tower

**Soldiers (Spawnable)**
- HP: 100
- Damage: 100
- Range: Long-range (40 units)
- Behavior: Auto-hunt nearest zombie

**Zombies (Enemy)**
- HP: 100
- Damage: 5
- Speed: 0.5 units/frame
- Behavior: Attack nearest tower or soldier

### AI Behavior

**Zombie AI (Priority System):**
```
1. Check for nearby soldiers
2. Check for nearby towers
3. Attack if in range (≤6 units)
4. Move toward target if farther
5. Collision avoidance with other zombies
6. Smooth rotation toward target
```

**Soldier AI:**
```
1. Find nearest zombie
2. Rotate to face target
3. Attack if in range (≤40 units)
4. Move closer if out of range
5. Avoid tower collisions
6. Separation from other soldiers
```

**Tower AI:**
```
1. Scan for enemies in radius
2. Target nearest enemy
3. Shoot if cooldown ready
4. Deal damage + play sound
```

## 🎨 Rendering Pipeline

```
┌─────────────────────┐
│ Sort by Depth       │ ← (y, x+z) key
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Draw Ground Tiles   │ ← Grass grid
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Draw Entities       │ ← Towers, zombies, soldiers
│ (Depth-sorted)      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Draw Health Bars    │ ← HP/Shield overlays
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Draw Shield Dome    │ ← Main castle effect (if active)
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Draw Attack Lines   │ ← Visual feedback (sub-layer)
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Draw UI Overlay     │ ← Stats, mode indicators
└─────────────────────┘
```

## ⚙️ Configuration

### Game Balance (config.py)

```python
# Map settings
GRID_SIZE = 100              # World grid dimensions
GRASS_SCALE = 3              # Tile scale multiplier
SEPARATION_RADIUS = 2.0      # Entity spacing

# Spawn settings
NUM_ENEMIES = 50             # Starting zombie count
SETUP_DURATION = 5000        # Build phase time (ms)

# Projection settings
PROJECTION_WIDTH = 16        # Isometric tile width ratio
PROJECTION_HEIGHT = 8        # Isometric tile height ratio
```

## 📊 Performance Tips

1. **Reduce enemy count** if FPS drops below 30
2. **Lower `GRID_SIZE`** for smaller maps (faster rendering)
3. **Increase `cell_size`** in `SpatialGrid` for fewer checks
4. **Disable debug rendering** (`DrawDebugInfo()` is commented out)

## 🤝 Contributing

This is an educational project. Feel free to:
- Add new enemy types
- Implement A* pathfinding
- Create new tower types
- Design custom levels
- Improve AI behaviors
