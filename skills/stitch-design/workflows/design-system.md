---
description: Central workflow for managing a Stitch project's design system.
  Handles creating/updating .stitch/DESIGN.md, analyzing existing designs, and
  syncing the design system with Stitch MCP via create_design_system and
  update_design_system.
---

# Workflow: Design System

Create a "source of truth" for your project's design language to ensure
consistency across all future screens.

## 📥 Retrieval

To analyze a Stitch project, you must retrieve metadata and assets using the
Stitch MCP tools:

1.  **Project lookup**: Use `list_projects` to find the target `projectId`.
2.  **Screen lookup**: Use `list_screens` for that `projectId` to find
representative screens (e.g., "Home", "Main Dashboard").
3.  **Metadata fetch**: Call `get_screen` for the target screen to get
`screenshot.downloadUrl` and `htmlCode.downloadUrl`.
4.  **Asset download**: Use `read_url_content` to fetch the HTML code.

## 🧠 Analysis & Synthesis

### 1. Identify Identity

- Capture Project Title and Project ID.

### 2. Define Atmosphere

- Analyze the HTML and screenshot to capture the "vibe" (e.g., "Airy",
 "Professional", "Vibrant").

### 3. Map Color Palette

- Extract exact hex codes and assign functional roles (e.g., "Primary Action: #2563eb").

### 4. Translate Geometry

- Convert Tailwind/CSS values into descriptive language (e.g., `rounded-full` → "Pill-shaped").

### 5. Document Depth

- Describe shadow styles and layering (e.g., "Soft, diffused elevation").

## 📝 Output Structure

Create a `.stitch/DESIGN.md` file in the project directory with this structure:

```markdown
# Design System: [Project Title]
**Project ID:** [Insert Project ID Here]

## 1. Visual Theme & Atmosphere
(Description of mood and aesthetic philosophy)

## 2. Color Palette & Roles
(Descriptive Name + Hex Code + Role)

## 3. Typography Rules
(Font families, weights, and usage)

## 4. Component Stylings
* **Buttons:** Shape, color, behavior
* **Containers:** Roundness, elevation

## 5. Layout Principles
(Whitespace strategy and grid alignment)
```

## 🚀 Create or Update Design System in Stitch
After generating `.stitch/DESIGN.md`, make sure to also create or update the
design system in Stitch using MCP tools.

**Two-step design system creation:**
1. Call `create_design_system` with an **empty request** — only pass the
`projectId` and an empty `designSystem` object (`{}`). This creates the asset
and returns the asset ID.
2. Immediately call `update_design_system` with the **full theme configuration**
using the returned asset ID (format: `assets/{asset_id}`). Map DESIGN.md content
to the required `theme` fields (`colorMode`, `headlineFont`, `bodyFont`,
`roundness`, `customColor`) and pass the full markdown verbatim in
`theme.designMd`. See [Tool Schemas](/labs/language/aida/product/appcompanion/stitch_skills/stitch-design/references/tool-schemas.md) for the exact format.

> [!IMPORTANT]
> Do NOT put theme data in `create_design_system`. The `update_design_system`
call is what actually applies the design tokens and displays the design system
in the UI.

Once both `create_design_system` and `update_design_system` have been called,
Stitch holds the design tokens at the project level — you do NOT need to repeat
them in generation prompts.

## 📋 Update Project Metadata
After writing `.stitch/DESIGN.md`, also create or update `.stitch/metadata.json`
to track the `projectId`, `title`, all known screens, and design system summary.
 See [examples/metadata.json](/labs/language/aida/product/appcompanion/stitch_skills/stitch-design/examples/metadata.json) for the format.

## 💡 Best Practices

- **Be Precise**: Always include hex codes in parentheses.
- **Be Descriptive**: Use natural language like "Deep Ocean Blue" instead of just "Blue".
- **Be Functional**: Explain *why* an element is used.
