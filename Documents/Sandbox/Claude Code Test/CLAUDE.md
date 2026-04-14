# CLAUDE.md — Turn Soccer (3D Turn-Based Soccer Game)

## Project Overview
A 3D turn-based soccer game delivered as a single self-contained HTML file (`soccer.html`).
Each "turn," the match freezes and the player chooses one action; AI players simultaneously
decide their moves; then a short 3D animation resolves the turn before the next input prompt.

## Tech Stack
- **Three.js r128** loaded via CDN (no build tools, no npm, no bundler)
- **Vanilla JavaScript** — no frameworks
- **HTML5 + CSS3** — inline in a single file
- Single file pattern: all `<style>` and `<script>` are inline in `soccer.html`

## File Location
`Documents/Sandbox/Claude Code Test/soccer.html`

To run: open `soccer.html` directly in any modern browser. No server required.

## Game Architecture

### State Machine
```
MENU → LEVEL_SELECT → PLAYING ⇄ RESOLVING → WIN / LOSE
```

- `PLAYING`: waiting for player input, action panel visible
- `RESOLVING`: animation in progress, input locked

### Turn Resolution Sequence (inside `resolveTurn(playerAction)`)
1. Lock input (state → RESOLVING, hide action panel)
2. All AI entities call `aiDecide()` synchronously
3. Compute all destinations (ball path + entity target positions)
4. `animateTurn()` — async, returns a Promise, resolves after ~700ms
5. Post-animation: check goal / tackle / pass completion / turn limit
6. Transition back to PLAYING or to WIN/LOSE

### Key Systems
- **Tweener** — minimal internal tween class, no external dep. All animation goes through it.
- **CameraRig** — third-person camera that follows the ball carrier; lerps each frame
- **Entities** — registry of all player/AI objects (Entity class: position, mesh, role, team)
- **Ball** — single global object with mesh + position
- **UIState** — tracks current action panel selection state

## 3 Levels
| # | Name | Setup | Win Condition |
|---|------|-------|---------------|
| 1 | 1v1 Breakaway | 1 attacker vs 1 GK | Score a goal within 10 turns |
| 2 | 2v2 Build-Up | 2 attackers vs 2 defenders | Score a goal within 15 turns |
| 3 | 5v5 Full Attack | 5 attackers vs 4 defenders + GK | Score a goal within 20 turns |

## Player Actions
Each turn the player picks exactly one:
- **DRIBBLE** — direction (8 compass points N/NE/E/SE/S/SW/W/NW)
- **PASS** — direction (8 compass) + potency (low/medium/high)
- **SHOOT** — direction (8 compass) + type (curled/power/lob)

## AI Behavior
- **Goalkeeper**: Positions between ball and goal center; dives on SHOOT
- **Defender (L2)**: Moves directly toward ball carrier each turn
- **Defender (L3)**: Predicts carrier's next position and cuts off path
- Tackle succeeds if AI is within `TACKLE_RADIUS = 1.2` units after animation → LOSE

## Field Dimensions (Three.js units)
- Width: 30 units (X: -15 to +15)
- Length: 50 units (Z: -25 to +25)
- AI goal at Z = -24, goal width = 7.32 units

## Visual Style
- Low-poly / flat-shaded
- Field: green plane with white line markings and goals
- Players: cylinder body + sphere head, team-colored (blue vs red/orange)
- Ball: white sphere
- Sky: light blue background + fog

## Code Sections (in order within `<script>`)
1. Constants & Config (PHYSICS, FIELD, TEAM_COLORS, DIR_VECTORS)
2. State Machine + `showScreen()`
3. `initThree()` — renderer, scene, camera, lights
4. `buildScene()` — field, goals, markings
5. `Entity` class + `Entities` registry
6. `Ball` object
7. `LEVELS` config object
8. `startLevel(n)`
9. `buildActionPanel()` + `UIState`
10. `resolveTurn(playerAction)` — 4-phase loop
11. Ball physics (`computeBallArc`, `computeDribblePath`, `computeShotArc`)
12. AI logic (`aiDecide`, `aiGoalkeeperDecide`, `aiDefenderDecide`)
13. `CameraRig`
14. `Tweener` class + `Ease` functions
15. `animateTurn()` — Promise-based
16. `checkGoal`, `checkTackle`, `resolvePassComplete`
17. `tick()` render loop
18. Boot — event listeners, `initThree()`, `buildScene()`, `tick()`, show menu

## Development Notes
- **CapsuleGeometry not in r128** — use CylinderGeometry body + two SphereGeometry caps
- **Goal detection** — check per-frame inside tween `onUpdate`, not just at animation end
- **Pass targeting** — direction + potency resolve to nearest teammate along that vector (no explicit target picker in UI)
- **Camera facing** — carrier's `facingDir` drives camera offset; default facing is toward AI goal

## Testing Checklist
- [ ] Menu → Level Select → gameplay renders correctly
- [ ] Action panel shows/hides correctly per state
- [ ] All 3 action types build correct `PlayerAction` objects
- [ ] Dribble moves player + ball together
- [ ] Pass arcs correctly by potency; camera transitions to new carrier
- [ ] Shot arcs toward goal; goal detection triggers WIN screen
- [ ] AI defender intercepts / tackles → LOSE screen
- [ ] GK dives on SHOOT
- [ ] Turn counter increments; max turns → LOSE
- [ ] Level 1 → WIN → Level 2 progression works
- [ ] Level 3 completion → back to menu
