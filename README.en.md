# NEURO // LIVE

A floating 3D brain for **DSH Desktop** that maps **real agent runtime events** onto an anatomical cortical field.

[中文 README](./README.md) · [Live demo](https://slianhamess.github.io/dsh-neuro-live/) · [v0.2.0 release](https://github.com/Slianhamess/dsh-neuro-live/releases/tag/v0.2.0)

![DSH floating brain plugin](docs/panel-final.png)

This repository contains two deliverables:

| Directory | What it is | Reads real data? |
| --- | --- | --- |
| [`plugin/`](./plugin/) | DSH Desktop floating-brain client plugin: live reasoning stream, tool calls, subagents, steps and token usage | ✅ Real DSH runtime events |
| [`standalone/`](./standalone/) | Standalone Three.js demo with tunable brain parameters and a scripted thinking animation | ❌ Scripted demo, no model connection |

![Standalone Three.js demo](docs/standalone.png)

---

## Features

### DSH plugin

- **Real runtime event graph**: reasoning segments, tool calls, subagent delegations, steps and token usage become event nodes inside the brain.
- **Whole-brain dense neural field**: about 374 base neurons (160 interior + 190 cortical surface + 24 cerebellar), with 500+ links.
- **Meteor-like signals**: each signal is a 12-sample fading trail travelling along a link, not a single moving dot.
- **48% cortex opacity**: the cortex stays readable while the internal field remains visible.
- **Functional mapping**: read tools tend toward temporal regions, writes toward prefrontal, terminal commands toward motor, search toward parietal, and real subagent calls cross hemispheres.
- **Live telemetry**: current action, reasoning tail, tool chain, step, tool count, real subagent calls, tokens, character count and elapsed time.
- **Verifiable parent/subagent display**: the parent is the current DSH session (short `sessionId` in the footer); subagent nodes only come from real `subagent` / `subagent_fork` tool calls. No call, no node.
- **Floating window controls**: drag, collapse into an orb, close until the next turn. The panel stays visible by default and keeps rotating at 60fps while idle; it does not auto-collapse.
- **Local assets**: Three.js and the brain mesh are bundled; the plugin needs no network at runtime.

### Standalone demo

- Anatomical brain mesh, translucent cortex, cerebellum and brain stem
- 140–520 tunable neurons, local connections, long-range links and moving pulses
- Auto-rotation, zoom, hover inspection and click-to-inject signals
- Scripted agent-thinking sequence: parse → recall → associate → simulate → verify → answer
- Parameter panel: rotation speed, density, pulse frequency, cortex opacity, scan speed, layer toggles
- Responsive HUD for desktop, tablet and mobile

---

## Honest data boundary

A language model's internal **weights, attention matrices and individual neurons** are not exposed by DSH's public interfaces.

Therefore:

- The plugin visualises **DSH runtime events**: reasoning stream, tool calls, subagents, steps and tokens.
- It does **not** fabricate internal model activations and does not claim to show literal neuron firing.
- The dim base neural field is a visual substrate, not live data; real event nodes light up as traces arrive.
- The parent agent is simply the current DSH session; subagents must correspond to real `callId`s or they are not drawn.
- The standalone demo is not connected to any model; its thinking chain is scripted.

The `i` overlay inside the plugin states the same boundary.

---

## Quick start

### 1. DSH Desktop plugin

Tested with **DSH Desktop 2.0.x**; client-plugin support must be enabled.

```powershell
$src = ".\plugin"
$dst = "$env:USERPROFILE\.dsh\profiles\desktop\node_modules\dsh-neuro-live"

if (Test-Path $dst) { Remove-Item $dst -Recurse -Force }
New-Item -ItemType Directory -Force $dst | Out-Null
Copy-Item "$src\*" $dst -Recurse -Force
```

Append to `~/.dsh/profiles/desktop/cordis.patch.yml`:

```yaml
- insert:
    - id: dsh-neuro-live
      name: 'dsh-neuro-live'
```

**Fully quit and reopen DSH Desktop** (including the tray). The client bundle is scanned at application startup; a running window will not hot-load a new plugin bundle. Your conversation is preserved.

After restart, send a message in any conversation. The brain appears in the bottom-right; when idle it collapses into an orb.

See [`plugin/README.md`](./plugin/README.md) for build, uninstall and troubleshooting details.

### 2. Standalone demo

```powershell
cd standalone
python -m http.server 8766
# open http://127.0.0.1:8766/
```

You can also open `standalone/index.html` directly; it has no server or network dependency.

Or try the hosted demo: **https://slianhamess.github.io/dsh-neuro-live/**

---

## Build

```powershell
cd plugin
node build.mjs
node --check lib/client.js
```

`lib/client.js` bundles:

- Three.js r155
- Quantized cortical mesh
- `src/engine.js` 3D brain engine
- `src/client.js` React floating-window client

Preview (React comes from a CDN; the production plugin receives React from DSH):

```powershell
python -m http.server 8766
# http://127.0.0.1:8766/plugin/preview/panel.html
```

---

## Controls

| Action | Plugin | Standalone |
| --- | --- | --- |
| Drag | Move floating window | Rotate camera |
| Wheel / pinch | — | Zoom |
| Hover node | Inspect real event | Inspect brain region |
| Click | Collapse / close | Inject a focus signal |
| `Space` | — | Run a scripted thinking demo |
| `A` / `R` / `H` | — | Auto-rotate / reset / help |

The plugin window position is stored in `localStorage` under `dsh-neuro-live.settings.v1`.

---

## Repository layout

```text
dsh-neuro-live/
├── plugin/                  # DSH Desktop plugin
│   ├── src/
│   │   ├── client.js        # slot registration, trace extraction, React UI
│   │   ├── engine.js        # Three.js brain field and event graph
│   │   └── brain-cortex.js  # quantized cortical mesh (CC BY-SA 4.0)
│   ├── lib/client.js        # generated bundle
│   ├── preview/             # standalone preview + mock data
│   ├── vendor/three.min.js  # Three.js r155
│   ├── build.mjs
│   ├── cordis.patch.yml
│   └── README.md
├── standalone/              # pure Three.js demo page
│   ├── index.html
│   ├── styles.css
│   ├── main.js
│   ├── brain-asset.js       # quantized brain mesh (CC BY-SA 4.0)
│   ├── vendor/three.min.js
│   └── README.md
├── docs/                    # README screenshots
├── THIRD_PARTY_NOTICES.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── LICENSE                  # MIT (code)
└── README.md
```

---

## Limitations

- Requires WebGL; very old GPUs or disabled hardware acceleration may not render.
- The plugin bundle is about 1.5 MB; first load has a parsing cost.
- The plugin is scoped to the current conversation view; it does not read global state from other sessions.
- Only the latest 28 real event nodes are retained.
- The standalone demo is not connected to any model; all thinking content is scripted.
- The brain mesh is for visual demonstration only and is **not suitable for clinical diagnosis, surgical planning or any medical use**.

---

## Contributing

Issues and PRs are welcome. See [`CONTRIBUTING.md`](./CONTRIBUTING.md).

Especially welcome:

- Better runtime event mappings (without inventing data)
- Performance and low-end WebGL improvements
- Accessibility and keyboard controls
- Brain mesh / material / lighting improvements
- Documentation, screenshots and translations

---

## License and credits

- **Project code** (HTML/CSS/JS/React client): MIT, see [`LICENSE`](./LICENSE).
- **3D anatomical brain derivatives** (`brain-asset.js`, `src/brain-cortex.js` and the mesh embedded in the bundle): **CC BY-SA 4.0**. Sources and modifications are listed in [`THIRD_PARTY_NOTICES.md`](./THIRD_PARTY_NOTICES.md).
- **Three.js r155**: MIT.
- The preview page uses React 18.3.1 from a CDN (MIT); the production plugin gets React from DSH.

> `plugin/lib/client.js` combines MIT code, Three.js and a CC BY-SA 4.0 brain mesh. Redistribution must comply with all applicable licenses and preserve attribution.

This project is not affiliated with DeepSeek. DSH Desktop is a separate product; this repository contains only plugin code and visual assets.

---

## Status

- Version: **v0.2.0**
- Plugin UI and 3D engine validated in the standalone preview
- DSH profile composition and host/client module resolution validated
- A DSH restart is required for the plugin to mount; slot/API details may differ between DSH versions

If this project is useful, star the repo or open an issue. You can also connect it to your own agent runtime.
