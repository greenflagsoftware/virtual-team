# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repo holds AI persona definitions, skills, and a HyperFrames video presentation that demonstrate and explain the virtual-team concept — AI agents working as a coordinated team with distinct roles and identities.

## Repository Structure

```
virtual-team/
├── docs/                        # Source materials (read-only reference)
│   ├── ppt/Materials/           # Exported slide PNGs (Slide1–Slide5.PNG)
│   ├── Materials.pptx           # Original PowerPoint source
│   ├── our_team_large.html      # HTML team card layout (2400px wide, for screenshotting)
│   ├── our-team.png / our-team-large.png  # Rendered team card images
│   ├── personas.png             # Persona overview diagram
│   └── Skills-Repo.png          # Skills repo diagram
└── virtual-teams/               # HyperFrames video composition project
    ├── index.html               # Root composition (main video timeline)
    ├── assets/slides/           # Slide PNGs copied here for use in compositions
    ├── compositions/            # Sub-compositions (blocks, components)
    ├── hyperframes.json         # HyperFrames registry + path config
    ├── meta.json                # Project metadata
    ├── CLAUDE.md                # HyperFrames-specific Claude instructions ← read this too
    └── package.json             # HyperFrames@0.6.49 scripts
```

## HyperFrames Video Project

All video composition work happens in `virtual-teams/`. **Always `cd virtual-teams/` before running any HyperFrames command.** The `CLAUDE.md` inside that directory is the authoritative guide for composition authoring — read it before editing any `.html` file.

```bash
# From virtual-teams/
npm run dev       # preview server — run_in_background: true (long-running, never foreground)
npm run check     # lint + validate + inspect — always run after edits
npm run render    # render to MP4
npm run publish   # publish and get shareable link
```

## Source Material Conventions

- `docs/ppt/Materials/` contains the canonical exported slides (Slide1–Slide5.PNG). These are the source of truth for slide content and design.
- When a slide PNG is needed in a composition, **copy it to `virtual-teams/assets/slides/`** rather than referencing it via a `../docs/` relative path — the renderer requires assets to live inside the project directory.
- `docs/our_team_large.html` is a standalone 2400px-wide HTML file intended to be screenshotted, not served. It is not part of the HyperFrames composition.

## Git

- Local git identity: `greenflagsoftware / greenflagsoftware@gmail.com` (set at repo level, overrides global).
- Active branch for video work: `feature-hyperframes`.
- `.memsearch/` is gitignored — do not commit it.
