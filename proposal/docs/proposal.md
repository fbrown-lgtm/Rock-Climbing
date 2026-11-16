# Summit Sequence Project Proposal

## Finished product

**Summit Sequence** is a small single-player 3D rock-climbing game for players who enjoy planning careful moves. A player opens the menu, searches or filters a short list of climbs by difficulty, liked status, or completed ascents, and selects a route. On the climbing screen, the player sees a 3D climbing wall and can rotate the camera, chooses one of four limbs, and moves that limb to a reachable hold. over time the player loses stamina. The player can try to rest if the position alowws for a rest they will recover stamina, then continue toward the top. Reaching the finish hold records an ascent and returns the player to the menu. An account screen shows the number of climbs completed of each difficulty, total ascents, liked routes, and other statistics. The first version will include three small 3D routes, camera rotation, the menu filters, individual-limb movement, stamina and rests, ascent recording, and saved account data. Detailed character animation, large outdoor environments, sound, achievements, community built level, and leaderboards can wait until the core game works.

## Core rule and saved progress

The main challenge is choosing a safe sequence of moves before stamina runs out. A limb may move only to a logical hold that is within its allowed reach. Every second the player will lose 1 stamina point and will have a total of 100 stamina(might change). When trying to rest the player will recover between 1 to 15 stamian depening on the qualty of the rest. When stamina hits zero the climber would fall. The climb is complete when either hand reaches the finish hold.

The game will save the player's liked routes, completed routes, total ascents, ascents by difficulty, and high point for each completed route. This information will be restored when the game closes and reopens.

## Three core user journeys

### W1 Choose a climb

The player opens the menu, selects the "5.12" difficulty filter, and sees only 5.12 routes. They choose a route and press **Start Climb**. The climbing screen loads the selected route with the climber at the starting holds and stamina at 100.

### W2 Complete a climb by managing stamina

The player starts the 5.12 route, selects a limb, and moves it to a reachable hold. They repeat this to make progress, use the rests to regain stamina, and move a hand to the finish hold. The game displays a completion result, records the ascent, and returns the player to the route menu.

### W3 Review account progress

After completing a 5.12 climb, the player opens **Account** from the menu. The screen udate to show one added to the 5.12 ascent and total ascent count and the completed route. After reopening the game, the same statistics are still shown.

## Acceptance checks

| Check | Starting state | Exact actions | Pass result |
|---|---|---|---|
| W1 Filter and start a route | New account with three routes: one 5.6, one 5.9, and one 5.12 | Select the 5.9 filter, choose the visible route, and press Start Climb | Only the 5.9 route is listed; its climbing screen opens with the correct title, starting limb positions, and 100 stamina. |
| W2 Climb, rest, and finish | New account on the 5.6 route | Make the planned valid moves to a rest pasision, confirm the stamina is increase, then move a hand to the finish hold | Each valid move updates the limb position and stamina; the rest increases stamina without going above 100; the finish screen records one successful ascent. |
| W3 Like and review a route | New account with one selected 5.6 route | Press Like, complete the route, return to the menu, and open Account | The route is marked liked; Account reports one 5.6 ascent, and one total ascent. |
| W4 Reject a disallowed move | A climb in progress with a limb on a valid hold and a saved account containing one liked route | Select that limb and click a hold outside its reach or not on a hold| The game explains that the hold is out of reach or not useable. Limb positions, likes, and saved statistics remain unchanged. |
| W5 Restore progress | Account with one liked 5.6 route and one recorded 5.6 ascent | Close the game tab, reopen the local game, and open Account and the route menu | The liked route, ascent totals, difficulty totals, and completed-route marker are still accerate. |

## Technology comparison and choice

| Option | Small runnable test | Main setup risk | Local fallback |
|---|---|---|---|
| HTML, CSS, JavaScript, and Three.js | Render one 3D wall with clickable hold spheres and move a colored limb marker after a click | Selecting the correct 3D hold and setting up the Three.js module | Use a simplified fixed camera and large, spaced holds while keeping the same movement rules |
| Godot with GDScript | Create a 3D scene with clickable holds, a movable camera, and a stamina label | Game-engine installation or project-export issues on the Mac | Build the same first route as a browser game with Three.js |

I will use **Codex** as the AI coding assistant and build the game with **HTML, CSS, JavaScript, and Three.js**. It runs locally in a browser on a Mac, needs no account or paid service, and can store small saved data using `localStorage`. Before building the full game, I will make a small page that renders one 3D wall, lets the camera rotate, lets one limb move between two holds, and changes a displayed stamina number. The riskiest part is selecting the intended 3D hold and keeping the saved statistics in sync; the fallback is a fixed camera with larger, more widely spaced holds. I will demo the app by opening the local `index.html` file in a browser.

## Build plan

| Phase | Target checkpoint | Working behavior to show | Ready check | Dependencies and deferrals |
|---|---|---|---|---|
| Check the biggest risk | 3D movement experiment | One limb can move only to highlighted 3D holds; the camera rotates; stamina changes once per move | Try a valid nearby hold and an invalid distant hold from two camera angles | This must work first. Detailed character animation can wait. |
| Build the prototype | One complete 3D beginner route | Start a route, move all limbs, reach a finish hold, and see a completion screen | Run W1 and W2 on the one route | Depends on route data, camera controls, and movement rules. Filters and account data can wait. |
| Add and test improvements | Menu, filters, likes, rests, and account screen | Search/filter routes, like a route, recover stamina at a rest, and view statistics | Run the related parts of W1-W3 after each improvement | Depends on a reliable completion result. More than three routes can wait. |
| Save and validate | Local saved progress | Likes, completed routes, and statistics survive a restart | Run W4 and W5; have a classmate complete a route without coaching | Depends on stable data fields. Achievements and sound can wait. |

## AI change and test process

For each AI-assisted change, I will describe one behavior and the expected result. I will run the game myself, repeat the related acceptance check, and record the change, result, and next step. If a change fails, I will give AI the exact steps, expected behavior, and actual behavior before asking for a targeted fix. I will not treat generated code as complete until I have run the applicable check.

## Class design review plan

- **2 minutes:** Introduce Summit Sequence, its player, and W2, the climb-rest-finish acceptance check.
- **3 minutes:** Show the user flow and block diagram. Walk through a successful climb and an out-of-reach move with recovery.
- **1 minute:** Explain the browser technology choice, the first movement experiment, and the saved-data risk.
- **4 minutes:** Ask classmates to trace a route with low stamina. I will ask which rule is confusing, which assumption they would test first, and which feature can be deferred. I will add at least one resulting revision or test to the plan.

## Repository and review materials

Repository link: _Add the repository URL before the class review._

The plain-English design is in [design.md](design.md). The two diagrams for the review are in [diagrams](diagrams/).
