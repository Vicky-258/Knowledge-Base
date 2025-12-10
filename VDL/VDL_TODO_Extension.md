# **VDL-EXT • OBSYDE TASKS (2.5)**

### Muted Indigo Personality Palette

_An official extension module for VDL-2.5_

This extension defines the **To-Do app personality palette, structural rules, and component behaviors** for VDL-2.5.  
It relies on all VDL-2.5 foundations—**density, structure, scaffolding, props, contrast hierarchy, and foreground depth**—while adding identity-specific rules for the Obsyde Tasks application.

The core VDL specification remains unchanged. This file acts as an **app-level extension module.**

---

# **1. PERSONALITY HUE — “Muted Indigo”**

### **Emotional Profile**

- thoughtful
    
- intelligent
    
- structured
    
- calm depth
    
- technical clarity
    

Muted Indigo reinforces VDL’s quiet, disciplined, engineering tone while giving Obsyde Tasks a **distinct identity**.

This is the **Tier-3 palette** for this application.

---

# **2. COLOR PALETTE (TOKENS)**

All tones remain **desaturated (40–60%)**, engineered, and atmospheric to preserve VDL-2.5 structure.

## **2.1 Primary Indigo Scale**

|Token|Hex|
|---|---|
|`indigo-950`|`#0E121F`|
|`indigo-900`|`#131728`|
|`indigo-800`|`#1C2134`|
|`indigo-700`|`#253045`|
|`indigo-600`|`#2F3C59`|
|`indigo-500`|`#3B4A72`|
|`indigo-400`|`#50608A`|
|`indigo-300`|`#6C7AA4`|
|`indigo-200`|`#8994BE`|
|`indigo-100`|`#AEB8D4`|
|`indigo-50`|`#D8DDF0`|

**Primary personality color:** `indigo-500`  
**Primary interaction color:** `indigo-700`

---

## **2.2 Semantic Colors (Indigo-Adapted)**

### **Success**

- `success-500`: `#3C6F57`
    
- `success-600`: `#315A47`
    

### **Warning**

- `warning-500`: `#A1644C`
    

### **Error**

- `error-500`: `#8C3A4A`
    

These follow VDL-2.5’s contrast hierarchy: **medium contrast, calm tone, never loud.**

---

# **3. USAGE GUIDELINES**

## **3.1 Indigo vs Amber**

**Indigo = Personality** (identity)  
**Amber = System Accent** (VDL core language)

### Indigo is used for:

- primary actions
    
- icons
    
- tabs and navigation cues
    
- foreground section frames
    
- card headers
    
- empty state visuals
    
- high-density UI zones
    

### Amber is used for:

- metadata
    
- subtle outlines
    
- focus rings
    
- micro-interactions
    
- system-level highlights
    

### Hierarchical Rule:

> Indigo shapes identity. Amber shapes clarity.  
> They must **never compete** in brightness, saturation, or frequency.

---

## **3.2 Restrictions**

1. Indigo must always remain muted (no saturation spikes).
    
2. No gradients shifting indigo → bright blue.
    
3. No neon, glowing, or high-chroma blues.
    
4. Personality hue must remain singular: **Muted Indigo only**.
    
5. Indigo props must remain under **7% opacity**.
    
6. Amber must never replace indigo for primary actions.
    

---

# **4. COMPONENT APPLICATION (STRUCTURAL + VISUAL)**

VDL-2.5 introduces **density, frames, and depth**, so components now follow structural rules.

## **4.1 Buttons**

### **Primary Button**

- Background: `indigo-700`
    
- Hover: `indigo-600`
    
- Text: `#FFFFFF`
    
- Radius: 4–6px
    
- Depth: subtle foreground elevation
    
- Contrast Role: highest interactive surface
    

### **Secondary Button**

- Border: `indigo-600`
    
- Hover Fill: `indigo-800`
    
- Text: `indigo-200`
    
- Structure: fits in **Density 2 and 3** zones
    

### **Minimal Button**

- Text: `indigo-300`
    
- Hover: amber underline
    
- Use in low-density or navigation contexts
    

---

## **4.2 Inputs**

- Background: `#111315` (Tier-0 neutral)
    
- Border: `indigo-900`
    
- Hover: `indigo-800`
    
- Focus: amber micro-glow (VDL signature)
    
- Text: neutral gray (`#C7C7C7`)
    
- Structure: uses **Frame Type: Input Cluster**
    

---

## **4.3 Cards (Foreground Depth)**

- Background: `#111315`
    
- Border or edge lift: `indigo-900`
    
- Header Underline: `indigo-800`
    
- Depth: soft elevation (VDL-2.5 foreground depth)
    
- Props: low-opacity structural props only
    
- Must align to **Density Zone 2 or 3**
    

---

# **5. PROP EXTENSION RULES (STRUCTURAL)**

VDL-2.5 props must reinforce **structure**, not decoration.

## **5.1 Structural Props**

Allowed:

- faint indigo radial falloff
    
- micro-grid (`indigo-950`, 2–4% opacity)
    
- subtle mesh textures
    

Purpose: create atmospheric alignment and depth.

---

## **5.2 Data Props**

Used to support intelligence + curiosity:

- timestamps (`indigo-400`)
    
- hash-like IDs
    
- coordinate markers
    
- capsule labels such as `status: active`
    

Must align with density zones.

---

## **5.3 Character Props**

- appear only in empty or minimal-density zones
    
- tone must remain dry, observational, technical
    
- max 1–2 lines
    

---

# **6. EMPTY STATE SPEC (2.5 STRUCTURE)**

Empty states must use **Density 1**, with functional props.

### Required Elements:

- radial falloff (indigo-950 → transparent)
    
- micro-grid at 3% opacity
    
- capsule metadata
    
- calm microcopy
    
- centered or lightly framed structure
    

**Example:**

```
status: clear
No tasks pending.
Add a task to begin organizing your flow.
```

---

# **7. STRUCTURAL RULES FOR OBSYDE TASKS (NEW FOR 2.5)**

### **7.1 Density Zones**

- Task List → Density 2
    
- Settings → Density 3
    
- Empty Dashboard → Density 1
    

### **7.2 Structural Frames**

- Task Group Frame
    
- Priority Frame
    
- Metadata Strip Frame
    

### **7.3 Content Scaffolding**

- consistent vertical rhythm
    
- left-aligned column grid
    
- grouped card stacks
    

These prevent the UI from drifting into a visual void.

---

# **8. JSON TOKEN EXPORT (FOR AGENTS)**

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
    "noNeonBlue": true,
    "densityZones": {
      "dashboard": 2,
      "settings": 3,
      "empty": 1
    },
    "structuralFrames": true
  }
}
```

---

# _End of VDL-EXT • Obsyde Tasks (2.5)_