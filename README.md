# 🐉 Dragon's Hollow — 3D

A browser-based first-person fantasy action game built with [Three.js](https://threejs.org/) (WebGL) — real 3D geometry, dynamic torch lighting with shadows, mouse-look, and a first-person viewmodel weapon. Explore a dungeon maze and fight off wolves.

**This is the primary/active version of the project.** The original 2D top-down co-op game (the earlier iteration of Dragon's Hollow) has been moved to [`legacy/`](./legacy) for reference — see below.

## Play it

Open **`index.html`** in a modern browser. It needs an internet connection on first load (to fetch Three.js from a CDN) — everything else is self-contained in the one file.

Hosted for free on GitHub Pages: enable it in Settings → Pages → Deploy from branch → `main` / root. The game will be live at `https://<your-username>.github.io/<repo-name>/`.

## Controls

**Desktop:**

| Action | Input |
|---|---|
| Move | W / A / S / D |
| Look | Mouse (click the game to lock your cursor) |
| Attack | Click, or Space |
| Release cursor | Esc |

**Mobile / touch:**

| Action | Input |
|---|---|
| Move | Left analog joystick (bottom-left) |
| Look | Drag anywhere else on screen |
| Attack | ⚔ button (bottom-right) |

Controls are dual-touch aware — you can hold the joystick with one thumb and drag to look with the other at the same time. A rotate-to-landscape prompt appears automatically if a phone is held in portrait.

## Features

- Real WebGL 3D scene: box-built maze walls, floor, ceiling, and fog for depth
- Dynamic point-light torches that flicker and cast real shadows
- A low-poly 3D wolf enemy with a walk cycle that turns to face you and chases based on line-of-sight-aware AI
- First-person viewmodel dagger that swings on attack
- Minimap overlay showing the maze layout and live positions
- Fully separated architecture: all gameplay rules (movement, collision, line-of-sight, enemy AI) are pure functions independent of Three.js, so the rendering layer only ever *reads* game state — it never owns game logic

## Project structure

```
.
├── index.html                     # the 3D game - open this to play
├── legacy/
│   └── dragons-hollow-2d.html     # the original 2D top-down co-op game
└── README.md
```

## About legacy/dragons-hollow-2d.html

This is the original version of Dragon's Hollow: a 2D top-down local co-op action RPG (1-2 players, 5 hero classes, a skill tree, loot/equipment, 5 explorable rooms, and a boss fight). It's fully playable and kept here for reference / in case development ever branches back to it, but **it is no longer the actively developed version** - all new work is going into the 3D game at the repo root.

## Development notes

- **Single-file architecture is intentional** for both games: everything (HTML/CSS/JS, all art, all audio) lives in one file, no build tooling, no external asset files. Google Fonts and the Three.js CDN script are the only external network dependencies, loaded directly by the browser.
- **No package.json / no npm install** - there is no build step for either file.
- The 3D game's gameplay logic (collision, line-of-sight, enemy movement, and the camera-direction math) was developed and unit-tested in isolation with Node before being wired into the Three.js rendering layer, specifically because WebGL rendering can't be executed/verified in a headless environment the way plain game logic can. If you're extending the 3D game, keeping that separation (pure logic functions vs. Three.js scene code) is strongly recommended - it's what made it possible to catch a real camera-direction bug (movement not matching look direction) before it ever shipped.

## License

No license file is currently included - add one (MIT is a common, permissive choice for a hobby project like this) if you plan to share or accept contributions.
