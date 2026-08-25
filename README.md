# 🐉 Dragon's Hollow — 3D

A browser-based first-person fantasy action RPG built with [Three.js](https://threejs.org/) (WebGL) — real 3D geometry, dynamic torch lighting with shadows, mouse-look, class selection, and a full XP/skill tree/loot progression system.

**This is the primary/active version of the project.** The original 2D top-down co-op game has been moved to [`legacy/`](./legacy) for reference.

## Play it

Open **`index.html`** in a modern browser. Needs an internet connection on first load (to fetch Three.js from a CDN) — everything else is self-contained in the one file.

Hosted for free on GitHub Pages: enable it in Settings → Pages → Deploy from branch → `main` / root.

## Choose your hero

Five classes, each with real stat and playstyle differences:

| Class | Attack | Style |
|---|---|---|
| Warrior | Melee | Tanky, high HP, short-range heavy blade |
| Mage | Ranged bolt | Glass cannon, strong burst damage |
| Ranger | Ranged bolt | Fastest, longest range, lower damage |
| Necromancer | Ranged bolt | Balanced dark-magic damage dealer |
| Beastmaster | Melee | Tough with the longest melee reach |

## Controls

**Desktop:** WASD to move, mouse to look (click to lock your cursor), click or Space to attack, Esc to release the cursor, ⭐ to open the skill tree.

**Mobile / touch:** left analog joystick to move, drag anywhere else to look, ⚔ button to attack, ⭐ button for the skill tree. Controls are dual-touch aware (hold the joystick with one thumb, look with the other) and use safe-area-aware sizing so they don't get clipped on small phone screens.

## Progression systems

- **XP & leveling** — defeating wolves grants XP; leveling up grants a skill point
- **Skill tree** (⭐ button, pauses the game) — 5 upgradeable nodes: Vitality (+HP), Might (+damage), Swiftness (+speed), Focus (-cooldown), Vampirism (heal on hit)
- **Loot & equipment** — wolves have a chance to drop gear across 3 rarity tiers (weapon/armor/trinket slots), shown in the world as a glowing floating icon and auto-equipped on pickup

## Features

- Real WebGL 3D scene: procedurally-textured brick walls and stone floor (canvas-generated, no external image files), fog for depth, ceiling beams
- Dynamic point-light torches with wall brackets and flickering flame props that cast real shadows
- Environment props for visual variety: barrels, crates, rubble piles, and columns scattered through the maze
- A low-poly 3D wolf enemy with a walk cycle that turns to face you and chases based on line-of-sight-aware AI
- Per-class first-person viewmodels (sword, staff, bow, tome, coiled whip) and ranged spell/arrow projectiles for casters
- Minimap overlay showing the maze layout and live positions
- Fully separated architecture: all gameplay rules (movement, collision, line-of-sight, enemy AI, XP/skills/loot math) are pure functions independent of Three.js — the rendering layer only ever *reads* game state, it never owns game logic

## Project structure

```
.
├── index.html                     # the 3D game - open this to play
├── legacy/
│   └── dragons-hollow-2d.html     # the original 2D top-down co-op game
└── README.md
```

## About legacy/dragons-hollow-2d.html

The original version of Dragon's Hollow: a 2D top-down local co-op action RPG (1-2 players, 5 hero classes, a skill tree, loot/equipment, 5 explorable rooms, and a boss fight). Fully playable, kept for reference, but **no longer the actively developed version**.

## Development notes

- **Single-file architecture is intentional** for both games: everything (HTML/CSS/JS, all art, all audio) lives in one file, no build tooling, no external asset files. Google Fonts and the Three.js CDN script are the only external network dependencies.
- **No package.json / no npm install** — no build step for either file.
- The 3D game's entire gameplay layer (movement, collision, line-of-sight, enemy AI, camera-direction math, class stats, skill math, equipment math, XP curves, and projectile physics) was developed and unit-tested in isolation with Node *before* being wired into the Three.js rendering layer, because WebGL rendering can't be executed/verified in a headless environment the way plain game logic can. Combat was additionally verified end-to-end (aim → attack → hit → kill → XP award, for both melee and ranged classes) using a headless Three.js API stub, and loot pickup and skill-point spending were verified the same way. If you're extending the 3D game, keeping gameplay logic as pure, Three.js-independent functions is strongly recommended — it's what makes this level of testing possible without a browser.

## License

No license file is currently included - add one (MIT is a common, permissive choice for a hobby project like this) if you plan to share or accept contributions.

