# 🐉 Dragon's Hollow

A browser-based fantasy co-op action RPG built as a single self-contained HTML file — no build step, no dependencies to install, no server required. Open it in a browser and play.

Built iteratively for local 1–2 player co-op play, with a focus on kid-friendly readability and pick-up-and-play controls.

## Play it

Just open **`index.html`** in a modern desktop or mobile browser. That's it — everything (game logic, art, audio, UI) lives in that one file.

Hosted for free on GitHub Pages once this repo is public: enable Pages in the repo settings (Settings → Pages → Deploy from branch → `main` / root), and the game will be live at `https://<your-username>.github.io/<repo-name>/`.

## Features

- **Local 1–2 player co-op** on one keyboard, or touch controls on mobile/tablet
- **Five hero classes**, each with a unique primary attack and special move:
  - **Warrior** — sword swings, Shield Bash (melee stun)
  - **Mage** — magic bolts, Arcane Nova (AoE burst)
  - **Ranger** — fast arrows, Volley (3-shot spread)
  - **Necromancer** — dark bolts, Raise Skeleton (summons an ally); also has a passive 35% chance for any fallen enemy to rise as a temporary ally
  - **Beastmaster** — whip-crack melee, Call Companion (summons a wolf ally)
- **Five explorable areas**: Millbrook Farm (peaceful hub, Stardew-style), Whispering Woods, Dungeon Depths, Castle Ruins, and Emberoth's Lair (boss fight)
- **Mode-aware co-op puzzle**: a two-plate pressure switch that requires simultaneous teamwork in 2-player mode, or sequential "sticky" plates solo in 1-player mode
- **XP, leveling, and a skill tree** — 6 upgradeable nodes per hero (Vitality, Might, Swiftness, Focus, Spirit/Vampirism, Overcharge)
- **Loot & equipment** — enemies drop gear across 3 rarity tiers (weapon/armor/trinket slots), auto-equipped on pickup
- **Boss fight** with telegraphed attacks (fire breath cone, tail swipe, add summons) that read clearly enough for kids to learn and dodge
- Hand-rolled pixel-art-style rendering pipeline (vector art rendered through an offscreen mosaic pass for a retro sprite look), procedurally decorated room backgrounds, and a lightweight WebAudio SFX synth — all with zero external asset files

## Controls

| Player | Move | Attack | Special |
|---|---|---|---|
| P1 | WASD | F | G |
| P2 | Arrow keys | K | L |

Touch controls (on-screen joystick + buttons) appear automatically on touch devices.

## Project structure

```
.
├── index.html                                # the game — open this to play
├── prototypes/
│   └── 3d-first-person-prototype.html         # standalone WebGL 3D experiment (see below)
└── README.md
```

## About the `prototypes/` folder

`3d-first-person-prototype.html` is an **experimental, standalone** first-person 3D tech test built with [Three.js](https://threejs.org/) (loaded from a CDN, so it needs an internet connection to run). It is **not** connected to the main game in any way — it exists to explore whether a real 3D perspective (real geometry, dynamic lighting, mouse-look) is a direction worth pursuing for the project, without risking the working 2D game. It's a maze with one enemy (a wolf) to fight, nothing more.

If a full 3D conversion is ever pursued for real, it would be a from-scratch redesign of movement, combat, and puzzle systems — not a port — since the main game's mechanics (top-down hitboxes, room-wide puzzle visibility, boss telegraphs) are built specifically around a bird's-eye 2D view.

## Development notes

- **Single-file architecture is intentional**: everything (HTML/CSS/JS, all art, all audio) lives in one file per game/prototype, with no external asset files and no build tooling. Google Fonts and the Three.js prototype's CDN script are the only external network dependencies, and both are loaded directly in the browser (not bundled).
- **No package.json / no npm install needed** — there is no build step. Just open the HTML file.
- Development so far has leaned heavily on **headless logic testing** (extracting pure game-logic functions and unit-testing them with Node, independent of rendering) to catch real bugs — this caught several genuine issues during development, including collision/movement math errors and an off-by-one-frame bug in ally lifetime tracking.

## License

No license file is currently included — add one (MIT is a common, permissive choice for a hobby project like this) if you plan to share or accept contributions.
