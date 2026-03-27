# Stitch MCP Tool Schemas

Use these examples to format your tool calls to the Stitch MCP server correctly.

---

## 🏗️ Project Management

### `list_projects`
Lists all Stitch projects accessible to you.
```json
// No parameters needed
{}
```

### `get_project`
Retrieves details of a specific project.
```json
{
  "name": "projects/4044680601076201931"
}
```

### `create_project`
Creates a new Stitch project.
```json
{
  "title": "My New App"
}
```

### `create_design_system`
Creates a new design system for a project.

> [!IMPORTANT]
> You MUST call `update_design_system` immediately after `create_design_system`
  to apply the design tokens to the project and display the design system in
  the UI. Use the asset ID returned from `create_design_system` as the `name`
  field (format: `assets/{asset_id}`).

```json
{
  "projectId": "4044680601076201931",
  "designSystem": {
    "displayName": "My Design System",               // OPTIONAL. Display name of the design system
    "theme": {                                       // REQUIRED. The design theme object
      "colorMode": "LIGHT",                          // REQUIRED. Options: LIGHT, DARK
      "headlineFont": "INTER",                       // REQUIRED. Options: INTER, ROBOTO, OPEN_SANS, LATO, MONTSERRAT, NOTO_SANS, NOTO_SERIF, etc.
      "bodyFont": "INTER",                           // REQUIRED. Same font options as headlineFont
      "labelFont": "INTER",                          // OPTIONAL. Same font options as headlineFont
      "roundness": "ROUND_EIGHT",                    // REQUIRED. Options: ROUND_FOUR, ROUND_EIGHT, ROUND_TWELVE, ROUND_FULL
      "customColor": "#0EA5E9",                    // REQUIRED. Primary brand color / seed color for dynamic color system (hex)
      "colorVariant": "FIDELITY",                    // OPTIONAL. Options: FIDELITY, TONAL, VIBRANT, EXPRESSIVE, CONTENT, MONOCHROME, FRUIT_SALAD, RAINBOW
      "overridePrimaryColor": "#996e47",           // OPTIONAL. Override primary color (hex)
      "overrideSecondaryColor": "#0EA5E9",         // OPTIONAL. Override secondary color (hex)
      "overrideTertiaryColor": "#c4956a",          // OPTIONAL. Override tertiary color (hex)
      "overrideNeutralColor": "#0D0D0D",           // OPTIONAL. Override neutral color (hex)
      "designMd": "# Design System..."               // OPTIONAL. Markdown string with detailed design system spec
    }
  }
}
```

### `update_design_system`
Updates an existing design system for a project, this is needed in order to
display the design system in the UI as well.
```json
{
  "name": "assets/15996705518239280238",
  "projectId": "4044680601076201931",
  "designSystem": {
    "displayName": "My Design System",              // OPTIONAL. Display name of the design system
    "theme": {                                      // REQUIRED. The design theme object
      "colorMode": "LIGHT",                         // REQUIRED. Options: LIGHT, DARK
      "headlineFont": "INTER",                      // REQUIRED. Options: INTER, ROBOTO, OPEN_SANS, LATO, MONTSERRAT, NOTO_SANS, NOTO_SERIF, etc.
      "bodyFont": "INTER",                          // REQUIRED. Same font options as headlineFont
      "labelFont": "INTER",                         // OPTIONAL. Same font options as headlineFont
      "roundness": "ROUND_EIGHT",                   // REQUIRED. Options: ROUND_FOUR, ROUND_EIGHT, ROUND_TWELVE, ROUND_FULL
      "customColor": "#0EA5E9",                   // REQUIRED. Primary brand color / seed color for dynamic color system (hex)
      "colorVariant": "FIDELITY",                   // OPTIONAL. Options: FIDELITY, TONAL, VIBRANT, EXPRESSIVE, CONTENT, MONOCHROME, FRUIT_SALAD, RAINBOW
      "overridePrimaryColor": "#996e47",          // OPTIONAL. Override primary color (hex)
      "overrideSecondaryColor": "#0EA5E9",        // OPTIONAL. Override secondary color (hex)
      "overrideTertiaryColor": "#c4956a",         // OPTIONAL. Override tertiary color (hex)
      "overrideNeutralColor": "#0D0D0D",          // OPTIONAL. Override neutral color (hex)
      "designMd": "# Design System..."              // OPTIONAL. Markdown string with detailed design system spec
    }
  }
}
```

---

## 🎨 Design Generation

### `generate_screen_from_text`
Generates a new screen from a text description.
```json
{
  "projectId": "4044680601076201931",
  "prompt": "A modern landing page for a coffee shop with a hero section, menu, and contact form. Use warm brown tones (#4b2c20) and a clean sans-serif font.",
  "deviceType": "DESKTOP" // Options: MOBILE, DESKTOP, TABLET
}
```

### `edit_screens`
Edits existing screens with a text prompt.
```json
{
  "projectId": "4044680601076201931",
  "selectedScreenIds": ["98b50e2ddc9943efb387052637738f61"],
  "prompt": "Change the background color to white (#ffffff) and make the call-to-action button larger."
}
```

---

## 🖼️ Screen Management

### `list_screens`
Lists all screens within a project.
```json
{
  "projectId": "4044680601076201931"
}
```

### `get_screen`
Retrieves details of a specific screen.
```json
{
  "projectId": "4044680601076201931",
  "screenId": "98b50e2ddc9943efb387052637738f61",
  "name": "projects/4044680601076201931/screens/98b50e2ddc9943efb387052637738f61"
}
```

### `apply_design_system`
Applies a design system to one or more screens in a project.

> [!IMPORTANT]
> `selectedScreenInstances` must contain **only** `id` and `sourceScreen` — do NOT
> include position/dimension fields (`x`, `y`, `width`, `height`) or the request
> will fail with "invalid argument". Get the screen instance IDs from
> `get_project`.

```json
{
  "projectId": "4044680601076201931",
  "assetId": "c277fcdfc1e04baf91b92d975ff4c54a",
  "selectedScreenInstances": [
    {
      "id": "98b50e2ddc9943efb387052637738f61",
      "sourceScreen": "projects/4044680601076201931/screens/98b50e2ddc9943efb387052637738f61"
    },
    {
      "id": "ab12cd34ef56789012345678abcdef01",
      "sourceScreen": "projects/4044680601076201931/screens/ab12cd34ef56789012345678abcdef01"
    }
  ]
}
```

**How to get the required IDs:**
1. Call `get_project` to retrieve `screenInstances` — each has an `id` and `sourceScreen`.
2. Call `list_design_systems` to retrieve the design system `name` (format:
   `assets/{assetId}`) — use the part after `assets/` as the `assetId`.
3. Filter out any instances with `type: "DESIGN_SYSTEM_INSTANCE"` — only pass real screens.