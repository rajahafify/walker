# WALKER

A delivery roguelike built with Three.js — endless procedural terrain, a jointed humanoid walker, cargo that slows you down, storms that drain you, and upgrades after every delivery. All UI is drawn into a canvas; there is no HTML UI.

## Run locally

```bash
python3 -m http.server 8137
# open http://localhost:8137/
```

It must be served over http — ES modules and import maps don't load from `file://`.

## Controls

| Input | Action |
| --- | --- |
| WASD / arrows | Move |
| Drag | Look |
| Wheel | Zoom |
| Enter | Start / restart |
| 1–3 | Pick an upgrade after a delivery |
| T | Toggle joint telemetry |

## Deploy to Cloudflare Pages

**Via dashboard**: Workers & Pages → Create → Pages → connect this repo → build command: none, output directory: `/` (static site).

**Via CLI**:

```bash
npx wrangler pages deploy . --project-name walker
```

## Structure

```
index.html          # the whole game
vendor/three.module.js
```

## Notes

- Terrain is an analytic height field (`WALK(x, z)`) sampled into streamed chunks — the walker's feet and the ground always agree.
- Gait constants live in one `GAIT` object in real-world units (stride in metres, cadence in steps/sec, joint angles in radians), modelled on an average adult humanoid platform. Retargeting to real hardware means editing that object.
- Press `T` in-game to record per-frame joint angles to `window.WALKER_TELEMETRY` — a hook for using play sessions as humanoid motion data.
- `?debug` exposes `window.__walker` for automated testing.
