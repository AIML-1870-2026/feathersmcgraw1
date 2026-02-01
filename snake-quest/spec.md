# Snake Quest: Dungeon Roguelike - Technical Specification

## Project Overview
A roguelike dungeon crawler reimagining of Snake with level-based progression, strategic upgrades, and environmental hazards. Built as a single-file HTML/CSS/JavaScript application for GitHub Pages deployment.

## Code Optimization Requirements

### Token Efficiency Directives
- **Single-file architecture**: All HTML, CSS, and JavaScript in one file for minimal overhead
- **Modular functions**: Small, reusable functions with clear single responsibilities
- **Efficient data structures**: Use arrays and objects appropriately, avoid redundant storage
- **Minimal dependencies**: Pure vanilla JavaScript, no external libraries
- **Clean code standards**: 
  - Descriptive but concise variable names
  - Comments only for complex logic, not obvious code
  - DRY principle (Don't Repeat Yourself)
  - Remove console.logs in production code

### Performance Optimization
- **Canvas rendering**: Use HTML5 Canvas for efficient 2D graphics
- **RequestAnimationFrame**: For smooth 60fps game loop
- **Object pooling**: Reuse particle objects for effects rather than creating new ones
- **Lazy loading**: Only calculate what's needed per frame
- **Efficient collision detection**: Grid-based spatial hashing, not nested loops

## Core Game Mechanics

### Level Structure
- **20 hand-crafted levels** with intentional difficulty curve
- **Variable board sizes**: 
  - Small (12x12 to 15x15): Claustrophobic, precision-focused
  - Medium (18x18 to 22x22): Balanced challenge
  - Large (25x25 to 30x30): Intricate mazes, longer distances
- **Non-linear difficulty**: Mix easy and hard levels to maintain engagement
- **Snake length resets** at the start of each level
- **Starting length varies** (3-6 segments) based on level design needs

### Level Progression Philosophy
- **Levels 1-5**: Tutorial zone - introduce one mechanic at a time
  - Level 1: Basic movement, no hazards (3 apples)
  - Level 2: Simple barriers (4 apples)
  - Level 3: Introduce lava squares (4 apples)
  - Level 4: First projectile turret (5 apples)
  - Level 5: Combination challenge (5 apples)
  
- **Levels 6-10**: Skill building - combine 2-3 mechanics
  - Multiple turrets with crossing patterns
  - Barrier mazes with lava
  - Tighter spaces with more apples
  
- **Levels 11-15**: Complexity - require upgrade strategy
  - Dense hazard combinations
  - Timing-based challenges (turret patterns)
  - Large boards with intricate layouts
  
- **Levels 16-20**: Mastery - test player skill ceiling
  - Multi-turret coordination required
  - Minimal safe zones
  - High apple counts on dangerous boards

### Apple Collection System
- **Apple count scales with board size**:
  - Small boards: 3-5 apples
  - Medium boards: 6-9 apples
  - Large boards: 10-15 apples
- **All apples must be collected** to complete level
- **Snake grows +1 segment** per apple (classic Snake rule)
- **Apple spawn**: Hand-placed in level design data, not random

### Victory Condition
- Complete level 20 → transition to treasure room scene
- **Treasure room visual**: Gold coins, chests, dungeon aesthetic
- **Victory message**: "You have conquered the dungeon! Well done, brave serpent."
- **Option to restart** and play again

## Hazard System

### Static Barriers
- **Impassable walls** within the grid
- **Visual**: Dark stone/brick texture fitting dungeon theme
- Collision = instant death

### Lava Squares
- **Instant death** on contact
- **Visual**: Animated orange/red glow effect, subtle pulsing
- Multiple lava squares can create dangerous zones

### Projectile Turrets
- **Two types**:
  1. **Wall-mounted**: Attached to grid edges, shoot inward
  2. **Standalone**: Placed within grid like obstacles
  
- **Predictable patterns** (essential for puzzle solving):
  - Cardinal direction shots (up/down/left/right)
  - Rotating turrets (sweep in arcs)
  - Timed volleys (shoot every N seconds)
  - Continuous streams vs burst fire
  
- **Projectile properties**:
  - Move at consistent speed (slower than max snake speed)
  - Visible trajectory
  - Pass through food and non-solid objects
  - Despawn at grid boundaries
  
- **Visual**: Glowing energy balls or arrows, trail effect
- **Collision**: Instant death unless player has shield upgrade

### Self-Collision
- **Classic Snake rule**: Hitting your own tail = death
- **Exception**: Ghost Tail upgrades make segments passable

## Upgrade System

### Upgrade Presentation
- **After each level clear**: Modal overlay with 3 random upgrade cards
- **Choice required**: Must select 1 before proceeding
- **Visual design**: Card-based UI with icon, name, description, stack indicator
- **No duplicates** in the same choice set (all 3 options unique)

### Upgrade Pool (20+ Unique Upgrades)

#### Defensive Upgrades (Stackable Quantities)
1. **Ghost Tail (3)**: Last 3 tail segments are passable
   - Stack: +2 segments per additional selection
   - Max stacks: 5 (last 13 segments passable)

2. **Projectile Shield (3)**: Absorb 3 projectile hits before death
   - Stack: +2 charges
   - Max stacks: Unlimited
   - Visual: Shield glow effect when active

3. **Lava Resistance (2)**: Survive 2 lava square contacts
   - Stack: +1 charge
   - Max stacks: Unlimited
   - Visual: Orange protective aura

4. **Wall Phase Charges (1)**: Pass through 1 barrier per level
   - Stack: +1 charge per level
   - Max stacks: Unlimited
   - Resets each level
   - Visual: Ghostly transparency when phasing

#### Mobility Upgrades
5. **Time Dilation (3s)**: Activate to slow game to 30% speed for 3 seconds
   - Stack: +2 seconds duration
   - Cooldown: 15 seconds
   - Visual: Blue time-warp effect

6. **Speed Boost (Passive)**: Increase base movement speed by 15%
   - Stack: +10% per additional selection
   - Max stacks: 3 (45% total increase)
   - Risk/reward: Faster = harder to control

7. **Dash Charge (1)**: Instantly move 3 squares in current direction
   - Stack: +1 charge per level
   - Resets each level
   - Passes through projectiles (invincibility frames)
   - Visual: Motion blur trail

#### Tactical Upgrades
8. **Shrink Ray (1 use/level)**: Remove last 3 tail segments
   - Stack: +1 use per level
   - Minimum length: 3 segments (can't shrink below)
   - Strategic: Create space in tight situations

9. **Apple Magnet (Passive)**: Apples auto-collect from 2 squares away
   - Stack: +1 square range
   - Max stacks: 3 (5 square range)

10. **Turret Hack (2 turrets)**: Disable 2 random turrets per level
    - Stack: +1 turret
    - Max stacks: Unlimited
    - Visual: Sparking/broken turret effect

11. **Temporary Invincibility (2s)**: Activate for 2 seconds of invulnerability
    - Stack: +1 second
    - Cooldown: 20 seconds
    - Visual: Golden shimmer effect

#### Growth Management
12. **Growth Lock (Toggle)**: Apples give points but don't increase length
    - Not stackable (toggle on/off)
    - Score multiplier: 2x points while active
    - Strategic: Keep snake small for difficult levels

13. **Selective Growth**: Every 2nd apple doesn't increase length
    - Stack: Every 3rd apple → Every 4th → etc.
    - Max stacks: 5 (every 6th apple no growth)

#### Offensive/Utility
14. **Projectile Deflector (Passive)**: 25% chance to deflect projectiles
    - Stack: +15% chance
    - Max stacks: 4 (85% chance)
    - Deflected projectiles destroy on wall contact

15. **Venomous Trail (3s)**: Leave temporary lava trail behind snake for 3 seconds
    - Stack: +2 seconds
    - Cooldown: 25 seconds
    - Damages player too (synergizes with lava resistance)

16. **Portal Pair (1 use/level)**: Place 2 portals, teleport between them
    - Stack: +1 pair per level
    - Portals persist until used once
    - Visual: Swirling purple vortex

17. **Second Chance (1)**: Revive at level start on death (lose all level progress)
    - Stack: +1 revive
    - Max stacks: 3
    - Visual: Phoenix resurrection effect

#### Vision/Information
18. **Future Sight (2s)**: See projectile trajectories 2 seconds ahead
    - Stack: +1 second
    - Cooldown: 12 seconds
    - Visual: Ghostly projectile paths

19. **Detector (Passive)**: Highlight nearest apple through walls
    - Stack: Show 2 nearest → 3 nearest → etc.
    - Visual: Glowing outline through obstacles

20. **Safe Path (1 use/level)**: Briefly show safe route to nearest apple
    - Stack: +1 use
    - Duration: 3 seconds
    - Visual: Glowing green path overlay

#### Score/Meta
21. **Score Multiplier (Passive)**: 1.5x points for everything
    - Stack: +0.25x multiplier
    - Max stacks: Unlimited
    - No gameplay effect, just bragging rights

22. **Apple Value (Passive)**: Each apple worth 2x points
    - Stack: +1x value (3x, 4x, etc.)
    - No gameplay effect

### Upgrade Balance Notes
- **Prevent broken combinations**: Test Ghost Tail + Wall Phase synergy
- **Encourage variety**: Weight system to avoid seeing same upgrades every run
- **Early game**: Higher chance of defensive/forgiving upgrades
- **Late game**: Offer more powerful but risky options

## Visual Design

### Theme: Dark Dungeon Crawler
- **Color palette**: 
  - Background: Dark grays (#1a1a1a, #2d2d2d)
  - Stone walls: Charcoal with subtle texture (#3a3a3a, #4a4a4a)
  - Snake: Emerald green with scales (#2ecc71, #27ae60)
  - Apples: Deep red with shine (#c0392b, #e74c3c)
  - Lava: Orange-red glow (#e67e22, #d35400)
  - Projectiles: Icy blue or fiery orange (#3498db, #e67e22)
  - UI: Gold accents (#f39c12)

### Visual Effects
- **Snake**:
  - Gradient from head (bright) to tail (darker)
  - Subtle scale texture or pattern
  - Head distinct from body (eyes, direction indicator)
  
- **Movement**:
  - Smooth interpolation between grid squares
  - Head rotation to face direction
  
- **Eating animation**:
  - Apple shrink/pop effect
  - Brief flash on snake
  - +1 score popup
  
- **Death**:
  - Explosion particle effect at collision point
  - Screen shake
  - Fade to black → respawn menu
  
- **Upgrade selection**:
  - Cards slide in from top
  - Hover effect (glow, lift)
  - Click feedback (card flip or pulse)
  
- **Level transition**:
  - Fade out → new level fade in (1 second)
  - Level number display in center
  
- **Victory**:
  - Confetti/particle burst
  - Treasure room scene (gold coins, chests)
  - Triumphant message

### UI Elements
- **HUD (always visible)**:
  - Current level: Top left
  - Apples collected / Total: Top center
  - Active upgrades with cooldowns: Top right (icons)
  - Score: Bottom left
  
- **Upgrade cards**:
  - Icon (symbolic representation)
  - Name (bold, 16-18px)
  - Description (12-14px, 2-3 lines max)
  - Stack indicator if applicable "(+2)" or "x3"
  
- **Pause menu**:
  - Resume
  - View upgrades (current run)
  - Restart run
  - Controls help

## Controls

### Keyboard (Primary)
- **Arrow Keys** or **WASD**: Snake direction
- **Spacebar**: Activate selected ability (time dilation, dash, etc.)
- **1, 2, 3**: Quick-select active abilities
- **P** or **ESC**: Pause
- **R**: Restart (from pause menu only)

### Input Handling
- **Queue 1 input**: Buffer next direction for precise turns
- **No reverse**: Can't turn 180° into yourself
- **Ability targeting**: Some abilities auto-activate (passive), others require spacebar

## Technical Implementation

### File Structure (Single HTML File)
```html
<!DOCTYPE html>
<html>
<head>
    <style>/* All CSS here */</style>
</head>
<body>
    <canvas id="gameCanvas"></canvas>
    <!-- UI overlays here -->
    <script>/* All JavaScript here */</script>
</body>
</html>
```

### JavaScript Architecture

#### Core Game Loop
```javascript
// Pseudocode structure
class Game {
    constructor() {
        this.state = 'MENU' // MENU, PLAYING, PAUSED, UPGRADE, VICTORY, GAMEOVER
        this.currentLevel = 0
        this.upgrades = []
        this.score = 0
    }
    
    init() { /* Setup canvas, load levels, bind controls */ }
    update(deltaTime) { /* Game logic */ }
    render() { /* Draw everything */ }
    gameLoop() { /* requestAnimationFrame loop */ }
}
```

#### Key Classes/Objects
- **Snake**: position, direction, segments[], grow(), move(), checkCollision()
- **Level**: boardSize, apples[], barriers[], lava[], turrets[], startPos
- **Turret**: position, pattern, timer, shoot()
- **Projectile**: position, velocity, update()
- **Upgrade**: id, name, description, stacks, apply(), isActive()
- **Particle**: For visual effects, pooled for efficiency

#### Level Data Structure
```javascript
const LEVELS = [
    {
        id: 1,
        boardSize: { width: 15, height: 15 },
        startPos: { x: 7, y: 7 },
        startLength: 3,
        apples: [
            { x: 5, y: 5 },
            { x: 10, y: 10 },
            // ... positions
        ],
        barriers: [ /* {x, y} coordinates */ ],
        lava: [ /* {x, y} coordinates */ ],
        turrets: [
            { x: 2, y: 2, type: 'cardinal', direction: 'right', fireRate: 2000 }
            // type: 'cardinal', 'rotating', 'burst'
        ]
    },
    // ... levels 2-20
]
```

#### Upgrade Data Structure
```javascript
const UPGRADE_POOL = [
    {
        id: 'ghost_tail',
        name: 'Ghost Tail',
        description: 'Last {value} tail segments are passable',
        baseValue: 3,
        stackValue: 2,
        maxStacks: 5,
        type: 'passive',
        apply: function(game) { /* implementation */ }
    },
    // ... all upgrades
]
```

### Collision Detection
```javascript
// Grid-based, O(1) lookup
function checkCollision(x, y) {
    // Check barriers
    if (level.barriers.some(b => b.x === x && b.y === y)) return 'barrier'
    
    // Check lava
    if (level.lava.some(l => l.x === x && l.y === y)) return 'lava'
    
    // Check self with ghost tail consideration
    // Check projectiles
    // etc.
}
```

### Rendering Strategy
- **Layered rendering**: Background → Grid → Hazards → Snake → Projectiles → UI
- **Cell-based drawing**: Each grid cell is N pixels (e.g., 20px)
- **Responsive canvas**: Scale based on board size and window size
- **Double buffering**: Canvas automatically handles this

## Deployment

### GitHub Pages Setup
1. Create repository: `snake-quest-roguelike`
2. Single file: `index.html` in root
3. Enable GitHub Pages from repository settings
4. URL: `https://[username].github.io/snake-quest-roguelike/`

### Code Checklist Before Deployment
- [ ] Remove all console.log statements
- [ ] Test all 20 levels are beatable
- [ ] Verify all upgrades function correctly
- [ ] Check mobile responsiveness (if implementing touch)
- [ ] Ensure no hardcoded file paths (all in one file)
- [ ] Test victory condition triggers properly
- [ ] Validate restart/reset functions clear state

## Development Priorities

### Phase 1: Core Mechanics (MVP)
1. Canvas setup and grid rendering
2. Snake movement and controls
3. Apple collection and growth
4. Self-collision detection
5. Level loading and transitions
6. Basic UI (level, apple count)

### Phase 2: Hazards
1. Static barriers
2. Lava squares with animation
3. Turret implementation
4. Projectile system
5. All collision types

### Phase 3: Upgrade System
1. Upgrade data structure
2. Random selection logic (3 unique choices)
3. Upgrade modal UI
4. Implement 5-6 core upgrades (test stackability)
5. Full pool of 20+ upgrades

### Phase 4: Polish
1. Visual effects (particles, animations)
2. Level design (all 20 levels)
3. Treasure room victory scene
4. Pause menu and restart
5. Sound effects (optional, low priority)

### Phase 5: Testing & Optimization
1. Playtest all levels for difficulty curve
2. Balance upgrades (no broken combos)
3. Performance optimization
4. Code cleanup and minification
5. Deployment to GitHub Pages

## Testing Criteria

### Functional Tests
- [ ] All 20 levels completable without upgrades (difficult but possible)
- [ ] Each upgrade functions as described
- [ ] Stacking upgrades correctly
- [ ] Death resets to level 1
- [ ] Victory screen triggers after level 20
- [ ] All hazard types cause death appropriately
- [ ] Upgrades with charges reset per level

### Balance Tests
- [ ] Levels 1-5 completable by beginners
- [ ] Levels 15-20 challenging even with upgrades
- [ ] No single upgrade makes game trivial
- [ ] Defensive upgrades feel valuable
- [ ] Offensive upgrades add fun, not just power

### Performance Tests
- [ ] 60fps on modern browsers
- [ ] No memory leaks over long sessions
- [ ] Smooth animations and transitions
- [ ] Quick level loading (<100ms)

## Success Metrics
- **Gameplay**: Each run takes 15-30 minutes for skilled player
- **Replayability**: Upgrade variety creates different strategies
- **Learning curve**: Fair difficulty progression
- **Visual clarity**: All hazards clearly distinguishable
- **Code quality**: Clean, efficient, well-commented single file under 1500 lines

---

## Implementation Notes for Claude Code

When implementing this specification:

1. **Start with MVP**: Get core Snake working before adding complexity
2. **Modular functions**: Each upgrade should be self-contained
3. **Test incrementally**: Don't build all 20 levels at once, start with 5
4. **Data-driven design**: Levels and upgrades in JSON-like structures
5. **Comment complex logic**: Especially upgrade interactions and collision checks
6. **Efficient rendering**: Only redraw what changed if possible
7. **Readable code**: Favor clarity over cleverness
8. **Single file**: Everything in one HTML file for easy deployment

### Token Optimization Reminders
- Reuse code patterns (don't reinvent collision checks each time)
- Template similar upgrades (create base classes/functions)
- Use array methods efficiently (map, filter, reduce)
- Avoid deep nesting (early returns, guard clauses)
- Combine related functionality (don't scatter upgrade logic)

**End of Specification**
