# BlenderMCP Project — Claude Instructions

This file gives Claude Code context for working in this repo. It supplements the user's global instructions in `~/.claude/CLAUDE.md`.

## Context

This repo is the **BlenderMCP server** — an MCP (Model Context Protocol) server that lets Claude drive Blender directly. Two main components:

- `addon.py` — Blender addon; runs a socket server inside Blender on port 9876
- `src/blender_mcp/server.py` — MCP server that Claude connects to; forwards commands over the socket to the addon

The user is new to Blender and to MCP. The primary use case right now is **using** BlenderMCP to work on 3D models together, not modifying the MCP source.

## User Profile

- New to Blender, learning the basics while we work
- Technical-creative (see global CLAUDE.md) — wants to ship, not ceremony
- Currently has a character model open in Blender (`assets/psycho-character.blend` is untracked in git — likely related)

## JV Working Folders: `input/` and `output/`

This fork adds a simple workflow pattern:

- **`input/`** — JV drops raw assets here (reference images, source meshes, FBX/OBJ from elsewhere, screenshots, etc.) for us to process.
- **`output/`** — finished/exported deliverables land here (renders, exported models, baked textures).

Both folders are **tracked as folders** (via `.gitkeep`) but their **contents are gitignored**. JV manages those files manually for now — never commit anything in them, and don't push large binaries through git. If JV asks to "process the input folder" or similar, treat it as the source of truth for whatever raw material we're working with that session.

## How to Work Together in Blender

When the `blender` MCP tools are connected, a typical flow:

1. **Orient first** — call `get_scene_info` and/or `get_viewport_screenshot` before assuming what's in the scene. The user said a model is there, but you haven't seen it yet.
2. **Small, reversible changes** — prefer operations that can be undone with Ctrl+Z. When something risky is needed (merging meshes, applying modifiers, deleting), tell the user first.
3. **Name things** — freshly created objects get default names like "Cube.001". Rename them so the scene stays readable.
4. **Show, don't just tell** — after a visible change, take a viewport screenshot so the user can see the result.
5. **Teach as you go** — the user is learning. When using a Blender concept (modifier, shader node, bone constraint, etc.), briefly explain what it is the first time it comes up. Log non-obvious lessons to `LEARNING.md` (see below).

## The `execute_blender_code` Escape Hatch

The MCP exposes `execute_blender_code`, which runs arbitrary Python in Blender's context. Use it when the dedicated tools don't cover what's needed, but:

- It can crash Blender if the code is wrong. Keep snippets small and focused.
- Prefer `bpy.ops` calls that match what a user would do in the UI — easier for the user to learn from.
- **Always tell the user before running non-trivial code** so they can save their work first.

## Learning Log

Maintain `LEARNING.md` as we go:

- When the user learns something new (a shortcut, a concept, a workflow), append it under **Concepts Learned**.
- When the user expresses a preference ("I like working in X mode", "always orbit with middle mouse"), append it under **Preferences**.
- When we discover a gotcha (addon version mismatch, port conflict, etc.), append it under **Gotchas**.
- Don't log every tiny thing — only what would help a future session pick up where we left off.

## Scope Discipline

The user has not asked us to modify the BlenderMCP source code. Don't propose refactors or improvements to `addon.py` / `server.py` unless the user brings it up or we hit a bug that requires it.
