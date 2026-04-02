
 # LightWave AI Skills

**Claude-compatible skill documents for AI-assisted code generation across the LightWave 3D ecosystem.**

Maintained by [tautologos]
LightWave 2024 / 2025 · Last updated 2026-04-02

---

## What this is

This repository contains structured **skill documents** for [Claude](https://claude.ai) Projects.
Each skill teaches Claude the exact APIs, patterns, templates, and gotchas it needs to generate
correct, production-ready LightWave plugin and scripting code — without hallucinating function
signatures or inventing non-existent commands.

Every skill follows the same structure:

```
skill-name/
  SKILL.md              ← instructions + reference for Claude
  templates/
    starter_file.ls     ← literal working skeleton Claude reads first
    another_file.c
```

Claude reads the template before writing a single line, replaces `PLUGIN_NAME` tokens,
fills in `TODO` sections, and returns working code grounded in the actual SDK.

---

## Skills

| Skill | Language | Target | Templates |
|-------|----------|--------|-----------|
| [`lscript-modeler`](#lscript-modeler) | LScript | Modeler | `cs_basic.ls` · `meshedit_basic.ls` · `scan_modify.ls` · `full_tool.ls` |
| [`lscript-layout`](#lscript-layout) | LScript | Layout | `motion_handler.ls` · `channel_filter.ls` · `generic.ls` |
| [`c-plugin-modeler`](#c-plugin-modeler) | C / C++ | Modeler `.p` | `commandseq_plugin.c` · `meshedit_plugin.c` · `build_mac.sh` |
| [`c-plugin-layout`](#c-plugin-layout) | C / C++ | Layout `.p` | `layout_handler.c` |
| [`python-lightwave`](#python-lightwave) | Python 3 | Layout PCore | `generic_plugin.py` · `displacement_plugin.py` · `master_plugin.py` |
| [`nodes`](#nodes) | C / C++ | Node Editor | `node_plugin.c` |
| [`shaders`](#shaders) | C / C++ · LScript | Surface shaders | `shader_classic.c` · `shader_lscript.ls` |

---

## Skill summaries

### lscript-modeler

CS and MD operation modes, point/polygon creation, geometry traversal, VMap access,
requester UI, layer management, undo grouping, `CommandInput`, progress monitor with
cancel detection, booleans, primitives, and the library/include system.
Templates cover everything from a one-shot CS command to a full production tool with
UI, validation, undo grouping, and cancel-safe progress loops.

### lscript-layout

All Layout script types: Item Animation (motion handler), Channel Filter, Generic,
and Master Class. Covers the full lifecycle (`create` / `process` / `options` /
`load` / `save`), Motion Access Object Agent, Channel Object Agent, `effectedby()`
dependency declaration, and scene I/O.

### c-plugin-modeler

Compiled Modeler plugins (`.p` files). CommandSequence for command-string automation,
MeshDataEdit for direct point/polygon access. Full `MeshEditOp` function table,
`polyScan` / `pointScan` callback patterns, XPanels UI, `GlobalFunc` access,
macOS universal binary build script, and Gatekeeper quarantine fix.

### c-plugin-layout

The complete Layout handler pattern in C: instance callbacks (`create` / `destroy` /
`copy` / `load` / `save` / `descln`), item dependency (`useItems` / `changeID`),
render callbacks (`init` / `cleanup` / `newTime`), and class-specific `evaluate` /
`flags`. Covers Displacement, ItemMotion, MeshDeformer, Shader, Environment,
ImageFilter, PixelFilter, Master, and CustomObject handler classes.

### python-lightwave

LightWave's embedded Python 3 (`lwsdk` / PCore). Handler base classes, `ServerRecord`
registration, `lwsdk` globals (`LWItemInfo`, `LWSceneInfo`, `LWMessageFuncs`),
scene item traversal, Layout command execution, `LWPreferenceSystem`, the PCore
Console, and XPanels from Python. Templates for Generic, Displacement, and Master
handlers.

### nodes

NodeHandler plugin for the LightWave Node Editor. Input/output creation,
`ConnectionType` constants (`NOT_RGB`, `NOT_SCALAR`, `NOT_VECTOR`, etc.),
the `evaluate` per-shading-point pattern, `LWShadingGeometry`, `NodeInputFuncs` /
`NodeOutputFuncs`, `setValue` / `getValue`, node display color, and XPanels UI.

### shaders

Classic `LWShaderHandler` (C and LScript) for per-point surface modification.
`LWShaderAccess` field reference, shader channel flags, safe raytracing function
usage, `LWShadingGeometry`, load/save, and XPanels / requester UI.
Also notes on `LWSurfaceShaderHandler` for PBR/BSDF integrators.

---

## How to use with Claude

### Option A — Claude Project (recommended)

1. Clone or download this repository.
2. Open [claude.ai](https://claude.ai) and create or open a **Project**.
3. Go to **Project settings → Add content** and upload the skill folders you need.
4. In conversation, ask Claude to write a plugin or script. It will automatically
   read the relevant SKILL.md and templates before generating code.

### Option B — Paste into context

Copy the contents of a `SKILL.md` and its templates directly into a conversation
as context before making your request.

### Triggering a skill

Claude selects the skill automatically based on your request. You can also be explicit:

```
Using the lscript-modeler skill, write a tool that offsets selected points along their normals.
```

```
Using the c-plugin-modeler skill, write a MeshDataEdit plugin that generates a spring curve.
```

---

## Repository layout

```
/
├── README.md
├── LICENSE
│
├── lscript-modeler/
│   ├── SKILL.md
│   └── templates/
│       ├── cs_basic.ls
│       ├── meshedit_basic.ls
│       ├── scan_modify.ls
│       └── full_tool.ls
│
├── lscript-layout/
│   ├── SKILL.md
│   └── templates/
│       ├── motion_handler.ls
│       ├── channel_filter.ls
│       └── generic.ls
│
├── c-plugin-modeler/
│   ├── SKILL.md
│   └── templates/
│       ├── commandseq_plugin.c
│       ├── meshedit_plugin.c
│       └── build_mac.sh
│
├── c-plugin-layout/
│   ├── SKILL.md
│   └── templates/
│       └── layout_handler.c
│
├── python-lightwave/
│   ├── SKILL.md
│   └── templates/
│       ├── generic_plugin.py
│       ├── displacement_plugin.py
│       └── master_plugin.py
│
├── nodes/
│   ├── SKILL.md
│   └── templates/
│       └── node_plugin.c
│
└── shaders/
    ├── SKILL.md
    └── templates/
        ├── shader_classic.c
        └── shader_lscript.ls
```

---

## Coverage status

| Category | Status |
|----------|--------|
| LScript — Modeler | ✅ Complete |
| LScript — Layout | ✅ Complete |
| C/C++ — Modeler plugins | ✅ Complete |
| C/C++ — Layout handlers | ✅ Complete |
| Python (PCore / lwsdk) | ✅ Complete |
| Node Editor | ✅ Complete |
| Shaders (classic + PBR) | ✅ Complete |
| LScript — Shader | ✅ In shaders skill |
| Modeler Tools (interactive) | 🔲 Planned |
| Geometry Processing | 🔲 Planned |
| Rendering Extensions | 🔲 Planned |
| Data I/O (OBJ, FBX, Alembic) | 🔲 Planned |
| GPU / GLSL viewport | 🔲 Planned |
| Rigging utilities | 🔲 Planned |
| Physics / Dynamics | 🔲 Planned |
| Asset Management | 🔲 Planned |

---

## Environment

- **LightWave**: 2024.2 / 2025 (SDK at `/Applications/Lightwave3d_2024.2.0/sdk/`)
- **macOS**: Apple Silicon + Intel (universal binary `-arch arm64 -arch x86_64`)
- **Windows**: MSVC / MinGW, `.dll` compiled as `.p`
- **Python**: embedded Python 3.x via PCore (`lwsdk` module, not installable via pip)
- **Claude**: tested with Claude Sonnet 4 in Claude Projects

---

## Contributing

Issues and pull requests are welcome. If you add a skill:

- Follow the existing `SKILL.md` structure (frontmatter, Step 0 template instruction, reference sections, gotchas).
- Include at least one working template with `PLUGIN_NAME` tokens and `TODO` markers.
- Keep the SKILL.md description field under 350 characters.
- Test that Claude generates working code from your template before submitting.

---

## License

MIT — see [LICENSE](LICENSE).

SDK headers and LightWave documentation are property of their respective owners
(NewTek / Vizrt). This repository contains only original skill documents and
template skeletons; no SDK headers or proprietary LightWave files are included.

---

## Author

**tautologos**

Philosopher, Artist,  and Researcher
