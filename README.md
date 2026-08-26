# 🐉 Dragon's Hollow — 3D

A browser-based first-person fantasy action RPG built with [Three.js](https://threejs.org/) (WebGL) — real 3D geometry, dynamic torch lighting with shadows, mouse-look, class selection, and a full XP/skill tree/loot progression system.

**This is the primary/active version of the project.** The original 2D top-down co-op game has been moved to [`legacy/`](./legacy) for reference.

## Play it

Open **`index.html`** in a modern browser. Needs an internet connection on first load (to fetch Three.js from a CDN) — everything else is self-contained in the one file.

Hosted for free on GitHub Pages: enable it in Settings → Pages → Deploy from branch → `main` / root.

## Choose your hero

Five classes, each with real stat differences, a distinct attack, and a unique special ability (E / right-click on desktop, ✨ button on mobile):

| Class | Attack | Special |
|---|---|---|
| Warrior | Melee | Shield Bash — radius damage + stuns the target |
| Mage | Ranged bolt | Arcane Nova — bigger radius burst |
| Ranger | Ranged bolt | Volley — fires 3 bolts in a spread |
| Necromancer | Ranged bolt | Raise Ally — summons a skeleton that fights alongside you |
| Beastmaster | Melee | Call Companion — summons a wolf pup ally |

## Weapon progression

Each class has its own named 3-tier weapon line — Starter → Discovered → Legendary — found as loot drops in the world instead of generic gear. Each tier jumps your base damage and unlocks a new effect:

| Class | Discovered (unlocks) | Legendary (unlocks) |
|---|---|---|
| Warrior | Sunforged Greatsword — *Sunstrike*: every 4th hit triggers a bonus shockwave | Dragonfang Cleaver — *Cleaving Roar*: special gets a bigger radius + knockback |
| Mage | Frostfire Staff — *Elemental Alternation*: bolts alternate fire (DoT) / ice (slow) | Staff of the Ancients — *Arcane Nova* becomes a ring of homing star-bolts |
| Ranger | Windrunner Bow — *Gale Shot*: every 5th arrow splits into 3 | Phoenix Bow — *Phoenix Volley*: special fires 5 arrows that splash on impact |
| Necromancer | Soul Reaper Scythe — *Soul Drain*: bonus lifesteal on every hit | Grimoire of the Fallen King — *Legion of Bone*: special summons 2 allies |
| Beastmaster | Beastcaller Horn — *Rally Call*: re-pressing special with a companion out triggers an instant bonus attack instead of re-summoning | Primal Bond Gauntlet — *Primal Surge*: summoning also buffs your own speed & damage |

Weapon tiers persist visually too — Legendary weapons read larger with a faint glowing aura around the tip/blade.

## Controls

**Desktop:** WASD to move, mouse to look (click to lock your cursor), click or Space to attack, E or right-click for your special, Esc to release the cursor, ⭐ to open the skill tree.

**Mobile / touch:** left analog joystick (vertically centered on the left edge) to move, drag anywhere else to look, ⚔/✨ buttons (vertically centered on the right edge) for attack/special, ⭐ button for the skill tree. Controls are dual-touch aware (hold the joystick with one thumb, look/attack with the other) and use safe-area-aware, viewport-relative sizing so they scale properly on small phone screens instead of clipping.

## Progression: levels & story

The dungeon isn't the whole game — defeating enough wolves in an area opens the way to the next:

1. **Dungeon Depths** — a dense, dark maze lit by flickering torches
2. **Sunlit Grove** — a bright, open outdoor area with scattered trees, under real sky lighting
3. **Castle Courtyard** — a semi-open colonnaded ruin

After the Castle Courtyard, the loop continues back to the Dungeon with tougher wolves, so there's always a next fight. Each level has its own lighting, fog, wall/floor textures, and layout density — not every level is a cramped corridor maze.

## Progression systems

- **XP & leveling** — defeating wolves grants XP; leveling up grants a skill point
- **Skill tree** (⭐ button, pauses the game) — 5 upgradeable nodes: Vitality (+HP), Might (+damage), Swiftness (+speed), Focus (-cooldown), Vampirism (heal on hit)
- **Loot & equipment** — wolves have a chance to drop gear across 3 rarity tiers (weapon/armor/trinket slots), shown in the world as a glowing floating icon and auto-equipped on pickup

## Features

- Real WebGL 3D scene: procedurally-textured brick walls and stone floor (canvas-generated, no external image files), fog for depth, ceiling beams
- 3 distinct levels with different lighting/mood palettes and progression gated by kill count, with looping difficulty scaling:
  - **Dungeon Depths** — dark, torch-lit corridors with cool blue crystal-vein fill lights for wayfinding
  - **Sunlit Grove** — bright open outdoor level with glowing mushroom clusters and firefly motes
  - **Castle Courtyard** — golden-hour ruins with braziers and green ivy-covered glowglass windows
- Every light source is paired with a visible emitter mesh (no "phantom" lights), ambient is never fully dark in any level, and prop placement runs through a real collision-overlap pass so no two light-bearing props ever land on top of each other
- Environment props for visual variety: barrels, crates, rubble piles, and columns scattered through each level
- A low-poly 3D wolf enemy with a walk cycle that turns to face you and chases based on line-of-sight-aware AI
- Per-class first-person viewmodels with real detail (wrapped grips, gems, coiled wire, fletched arrows, a skull-topped necromancer staff) and ranged bolt projectiles for casters, visually upgrading (larger, glowing aura) as you find better gear
- A shared ally-companion system powering the Necromancer's and Beastmaster's summon specials
- A full class-specific 3-tier weapon progression (Starter → Discovered → Legendary) with unique unlockable combat effects per class
- Minimap overlay showing the maze layout and live positions
- Fully separated architecture: all gameplay rules (movement, collision, line-of-sight, enemy AI, XP/skills/loot/specials/scenery-placement math) are pure functions independent of Three.js — the rendering layer only ever *reads* game state, it never owns game logic

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

