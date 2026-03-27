# Stitch Design Skill

A unified orchestrator for Stitch design workflows. This skill wraps the Stitch MCP server to provide a more intuitive interface, ensuring consistent prompt engineering, tool usage, and design system management.

## Install

```bash
npx skills add google-labs-code/stitch-skills --skill stitch-design --global
```

## What It Does

Enables professional-grade UI/UX design workflows through Stitch MCP by extending its core capabilities:

1. **Prompt Enhancement**: Transforms rough intent into structured, high-fidelity prompts with professional terminology and design system context.
2. **Design System Management**: Analyzes existing Stitch projects to create and maintain a `.stitch/DESIGN.md` "source of truth".
3. **UI Generation & Editing**: Intelligently selects the best generation or editing workflow (`generate_screen_from_text`, `edit_screens`, `generate_variants`) based on user intent.
4. **Asset Management & Uploads**: Uploads image assets or mockups directly to Stitch projects, and synchronizes remote designs by downloading HTML and screenshots to the local `.stitch/designs` directory.
5. **Image-to-Screen**: Generates new high-fidelity designs based on user-provided reference images.

## Prerequisites

- Stitch MCP Server access
- A project `projectId` (can be discovered via `list_projects`)

## Example Prompt

```text
Design a premium landing page for a mountain resort with a focus on serene luxury and glassmorphism.
```

## Skill Structure

```
stitch-design/
├── SKILL.md           — Core instructions & Prompt Pipeline
├── README.md          — This file
├── workflows/         — Specialized pipelines (Text-to-UI, Edit, MD)
├── references/        — UI/UX keywords & Technical Mappings
└── examples/          — Gold-standard references (Solace Mindfulness)
```

## Works With

- **`react:components` skill**: Hand-off generated designs for frontend implementation.
- **`stitch-loop` skill**: Provides the `DESIGN.md` context for autonomous building loops.
- **Multi-agent workflows**: Refines prompts before passing design tasks to specialized agents.

## Learn More

See [SKILL.md](./SKILL.md) for complete instructions.
