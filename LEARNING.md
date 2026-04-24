# Blender Learning Log

A running record of what I'm learning about Blender, my preferences, and gotchas we run into. Claude reads and updates this across sessions.

---

## Preferences

*How I like to work. Claude should follow these without being re-asked.*

- _(nothing logged yet)_

---

## Concepts Learned

*New-to-me Blender concepts with a brief definition in my own words. Dated so I can see progress.*

- **2026-04-24** — Session 1 start: fresh to Blender + BlenderMCP setup.
- **2026-04-24** — **Armature = skeleton**. A collection of **bones**. Bones have a `head` (start) and `tail` (end); the bone points from head to tail.
- **2026-04-24** — **Parent with Automatic Weights** = Blender estimates which bone influences each vertex based on proximity. Creates one **vertex group** per deform bone + an **Armature modifier** on the mesh. Fast, decent for simple rigs.
- **2026-04-24** — **Pose Mode** is where you rotate/move bones to animate. **Edit Mode** on an armature is where you add/move bones in their rest position.
- **2026-04-24** — **Action** = a single animation clip (a set of keyframed curves). Only one action is "active" on an object at a time — to ship multiple clips in one file you **push them down to the NLA** (Non-Linear Animation editor) as strips.
- **2026-04-24** — **GLB export with `export_animation_mode='NLA_TRACKS'`** makes each NLA track become its own named animation clip in the GLB. This is how you get multiple idle anims in one file.

---

## Shortcuts & Muscle Memory

*Keyboard shortcuts I want to remember. Kept tight — not a full cheat sheet.*

| Shortcut | Does what | Mode |
|----------|-----------|------|
| `N` | Toggle sidebar (where BlenderMCP tab lives) | 3D Viewport |

---

## Gotchas

*Things that bit us. One-line fix notes.*

- **2026-04-24** — Bones must have non-zero length (head != tail) or Blender drops them. Root bone at origin needs a small offset tail (e.g. 0.1 on Y) to exist.
- **2026-04-24** — GLB export warning "Mesh is not valid" on `PsychoCharacter_Mesh` — usually harmless, often from non-manifold geometry or loose verts. Runtime playback still works.
- **2026-04-24** — Character mesh is **158k verts / 273k tris** — fine for desktop, heavy for mobile/web. If perf matters later, decimate.
- **2026-04-24** — **NLA strips created via `strips.new()` in Python default to influence=0.0** in some Blender builds — they'll silently do nothing in the viewport. Fix: set `strip.influence = 1.0` explicitly after creating. (GLB export still reads the keyframes correctly either way.)
- **2026-04-24** — Viewport screenshots via MCP can show a **stale frame** after changing pose data. Force `bpy.context.view_layer.update()` + `area.tag_redraw()` and take a second shot if unsure.
- **2026-04-24** — **GLB export mode matters big time for multi-clip rigs.** `export_animation_mode='NLA_TRACKS'` bakes each clip's *timeline position* into its internal timing — so a strip at frames 61-90 exports as `2.5s - 3.75s` inside its own animation, meaning runtime plays 2.5s of nothing first. Use `export_animation_mode='ACTIONS'` instead — every action becomes its own animation starting at t=0. Symptom: "clip plays at the end only, rest is dead air."

---

## Open Questions

*Stuff I've wondered about but we haven't dug into yet. Claude: pick these up when relevant.*

- _(nothing logged yet)_
