# Summit Sequence Design

## Purpose

Summit Sequence is a small 3D browser game about planning individual hand and foot moves on a climbing route. The first playable version is intentionally small: three fixed 3D routes, a four-limb climber, stamina, rest holds, route filtering, likes, and saved account statistics. The game should feel like a strategy puzzle rather than a realistic climbing simulator.

## What the player sees

The menu lists the available routes. Each route card shows a name, difficulty, completion state, liked state, and ascent count. The player can type a route name or filter by difficulty, liked routes, or completed ascents. Choosing **Start Climb** opens the climbing screen.

The climbing screen shows a 3D wall, route holds, a green rest hold when the route has one, the finish hold, four colored limb markers, the selected limb, camera controls, and the stamina bar. The player can drag to rotate the camera around the wall and use a reset-camera button. When a limb is selected, reachable holds are highlighted. A short status message explains every rejected move and every completed climb.

The Account screen summarizes total ascents, ascents at beginner, intermediate, and advanced difficulty, liked routes, completed routes, and the best stamina remaining on each completed route.

## Movement and stamina rules

Each route describes the 3D coordinates and kind of every hold. The climber stores one hold ID for each limb: left hand, right hand, left foot, and right foot. When the player selects a limb and clicks a hold, the game checks these rules in order:

1. The clicked hold belongs to the active route.
2. The hold is within that limb's allowed reach from its current hold.
3. The move has enough remaining stamina.
4. If every check passes, the limb moves and the game subtracts 5 stamina for a normal move or 8 for a longer move.
5. A move to a rest hold adds 15 stamina, capped at 100.
6. If either hand is on the finish hold, the route is complete and the result is saved.

For the first version, reach is calculated from the straight-line distance between the old and new 3D hold coordinates. A normal move is within 1.1 wall units; a longer move is over 1.1 and up to 1.5 wall units. This creates a clear, testable rule and avoids needing physics or full-body animation before the core game is reliable.

## Main parts and their responsibilities

| Part | Job | Data it receives or saves |
|---|---|---|
| Route menu | Lists routes, applies search and filters, starts a selected route | Route data; selected route ID; liked and completion data |
| 3D climbing screen | Renders the wall, manages camera rotation, accepts limb selections and moves, and shows stamina and messages | Selected route; limb positions; camera state; stamina; completion result |
| Rules engine | Validates 3D reach, calculates stamina, applies rests, and decides completion | Route holds; selected limb; old and new 3D positions; stamina |
| Account and save service | Calculates statistics, updates likes and ascent records, restores data at launch | `localStorage` account data; route-completion results |

## Data to save

The built-in route definitions can be plain JavaScript objects because they are included with the game. Player progress is stored under one `localStorage` key, such as `summitSequenceAccount`.

```json
{
  "likedRouteIds": ["granite-start"],
  "completedRoutes": {
    "granite-start": {
      "ascents": 1,
      "bestStaminaRemaining": 32
    }
  },
  "totalAscents": 1,
  "ascentsByDifficulty": {
    "beginner": 1,
    "intermediate": 0,
    "advanced": 0
  }
}
```

The game will read this object when it starts. If the key is missing or malformed, it will use a new empty account instead of crashing. It will write the updated object after a route is liked or successfully completed.

## Error handling

An invalid hold click must not change limb positions, stamina, or saved data. The game will show a brief message such as "That hold is out of reach." If saved data cannot be read, the game will show a message that a fresh local account was started; the built-in routes will still remain playable.

## First prototype boundary

The first prototype only needs one beginner 3D wall, simple block or sphere holds, click-based movement, basic camera rotation, a stamina display, a rest hold, a finish hold, and one completion message. It does not need full-body animation, realistic physics, large outdoor environments, sound, online accounts, leaderboards, or a route editor. Once that single route passes W1 and W2, the menu, saved account data, extra routes, and filters can be added one piece at a time.

## Diagram files

- [User flow diagram](diagrams/climb-user-flow.svg)
- [Connected parts diagram](diagrams/game-block-diagram.svg)
