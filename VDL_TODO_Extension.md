# VDL-EXT • OBSYDE TASKS  
### Muted Indigo Personality Palette  
_An official extension module for VDL-2.1_

This document defines the **To-Do app color extension** for the Vicky Design Language (VDL-2.1).  
The core VDL spec remains unchanged; this file adds a **personality color system** specific to the Obsyde Tasks application.

---

# 1. PERSONALITY HUE — “Muted Indigo”

### Emotional Profile
- thoughtful  
- intelligent  
- organized  
- calm depth  
- introspective but structured  

Muted Indigo enhances VDL’s clarity and technical character without adding noise or saturation.  
It serves as the **Tier-3 app identity hue** for the To-Do app.

---

# 2. COLOR PALETTE (TOKENS)

All colors are desaturated (40–60%) and tuned to VDL’s atmospheric tone.

## 2.1 Primary Indigo Scale

| Token | Hex |
|-------|------|
| `indigo-950` | `#0E121F` |
| `indigo-900` | `#131728` |
| `indigo-800` | `#1C2134` |
| `indigo-700` | `#253045` |
| `indigo-600` | `#2F3C59` |
| `indigo-500` | `#3B4A72` |
| `indigo-400` | `#50608A` |
| `indigo-300` | `#6C7AA4` |
| `indigo-200` | `#8994BE` |
| `indigo-100` | `#AEB8D4` |
| `indigo-50`  | `#D8DDF0` |

**Primary personality color:** `indigo-500`  
**Primary interaction color:** `indigo-700`

---

## 2.2 Semantic Colors (Indigo-Adapted)

### Success  
- `success-500`: `#3C6F57`  
- `success-600`: `#315A47`

### Warning  
- `warning-500`: `#A1644C`

### Error  
- `error-500`: `#8C3A4A`

These remain muted and aligned with the VDL tone.

---

# 3. USAGE GUIDELINES

## 3.1 Indigo vs Amber

### Indigo (Personality Hue)
Used for:
- primary actions  
- icons  
- tabs  
- backgrounds for interactive components  
- empty state visuals  
- secondary highlights  

### Amber (VDL Core Accent)
Used for:
- metadata  
- outlines  
- subtle hover cues  
- focus rings  
- system-level emphasis  

**Rule:**  
Indigo defines the *identity*.  
Amber defines the *system accent*.  
They must never compete in saturation or intensity.

---

## 3.2 Rules & Restrictions
1. Indigo must always be lower saturation than typical UI blues.  
2. Never mix indigo → bright blue gradients.  
3. Indigo props should remain < 7% opacity.  
4. Use one personality hue only (Muted Indigo).  
5. Amber cannot be used as a substitute for primary actions.  
6. Avoid neon, glow-heavy, or high-chroma blues entirely.

---

# 4. COMPONENT APPLICATION

## 4.1 Buttons

### Primary Button
- Background: `indigo-700`  
- Hover: `indigo-600`  
- Text: `#FFFFFF`  
- Radius: 4–6px  
- Shadow: ultra-soft depth only  

### Secondary Button
- Border: `indigo-600`  
- Hover Fill: `indigo-800`  
- Text: `indigo-200`  

### Minimal Button
- Text: `indigo-300`  
- Hover: amber underline  

---

## 4.2 Inputs
- Background: VDL neutral (`#111315`)  
- Border (default): `indigo-900`  
- Hover: `indigo-800`  
- Focus Ring: subtle amber glow  
- Text: soft gray (`#C7C7C7`)  

---

## 4.3 Cards
- Background: `#111315`  
- Border/Elevation: `indigo-900`  
- Header Underline: `indigo-800`  
- Props: low opacity, atmospheric only  

---

# 5. PROP EXTENSION RULES

## 5.1 Structural Props
Allowed:
- faint indigo radial falloff  
- indigo mesh textures (2–5% opacity)  
- micro-grid using `indigo-950`  

Purpose: fill void without visual noise.

---

## 5.2 Data Props
- timestamps (`indigo-400`)  
- hash-like IDs (monospace)  
- capsule labels (`status: active`)  

---

## 5.3 Character Props
- appear only in empty states  
- tone: dry, observational, intelligent  
- max 1–2 lines  

---

# 6. EMPTY STATE SPEC

### Required Elements:
- subtle radial (indigo-950 → transparent)  
- micro-grid at 3% opacity  
- capsule metadata  
- calm microcopy  

**Example:**

Capsule: `status: clear`  
Heading: “No tasks pending.”  
Subtext: “Add a task to begin organizing your flow.”  

---

# 7. JSON TOKEN EXPORT (FOR AGENTS)

```json
{
  "palette": {
    "indigo": {
      "50": "#D8DDF0",
      "100": "#AEB8D4",
      "200": "#8994BE",
      "300": "#6C7AA4",
      "400": "#50608A",
      "500": "#3B4A72",
      "600": "#2F3C59",
      "700": "#253045",
      "800": "#1C2134",
      "900": "#131728",
      "950": "#0E121F"
    },
    "success": "#3C6F57",
    "warning": "#A1644C",
    "error": "#8C3A4A"
  },
  "personalityHue": "muted-indigo",
  "rules": {
    "primary": "indigo-700",
    "hover": "indigo-600",
    "textOnPrimary": "#FFFFFF",
    "accent": "amber-A2-only",
    "propsOpacityMax": 0.07,
    "personalityHueUsage": "primary actions, icons, tabs, cards, empty states",
    "noSaturation": true,
    "noNeonBlue": true
  }
}
