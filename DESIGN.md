# Design System Strategy: The Kinetic Terminal

## 1. Overview & Creative North Star
The Creative North Star for this design system is **"The Kinetic Terminal."** 

Unlike traditional CLI websites that merely mimic a black box, this system treats the web browser as a high-end, editorial canvas where the precision of code meets the breathability of modern minimalism. We are moving away from "retro-tech" and toward "advanced-utility." The layout intentionally breaks the rigid 12-column grid, using extreme whitespace and asymmetrical content blocks to guide the eye. By leveraging high-contrast monochrome and a singular, piercing accent color, we create an environment that feels both authoritative and lightweight-mirroring the speed and efficiency of the underlying Python tool.

---

## 2. Colors & Surface Philosophy
The palette is built on a high-contrast foundation, using tonal shifts rather than structural lines to define information architecture.

### The "No-Line" Rule
**Explicit Instruction:** You are prohibited from using 1px solid borders for sectioning or containment. Boundaries must be defined solely through background color shifts. For example, a `surface-container-low` section should sit directly against a `surface` background. If you feel a border is needed, you have failed the layout’s intentionality. Use whitespace or a tonal shift instead.

### Surface Hierarchy & Nesting
Treat the UI as a series of physical layers-like stacked sheets of fine paper. 
- **Base Layer:** `surface` (#fcf9f8)
- **Primary Content Wells:** `surface-container-low` (#f6f3f2)
- **Nested Utilities (Terminal Windows):** `surface-container-highest` (#e5e2e1)
- **Action Accents:** `primary` (#006e16) and `primary-container` (#00ff41).

### The "Glass & Terminal" Rule
To bridge the gap between a "flat" website and a "dynamic" CLI, use Glassmorphism for floating terminal overlays or sticky navigation headers. 
- Use `surface-container-lowest` (#ffffff) at 80% opacity with a `24px` backdrop-blur. 
- This ensures the "kinetic" movement of the background remains visible as the user scrolls, providing a "liquid" feel to the monochrome aesthetic.

### Signature Textures
Main CTAs should not be flat. Apply a subtle linear gradient from `primary` (#006e16) to `primary-fixed-dim` (#00e639) at a 135-degree angle. This provides a "digital glow" reminiscent of phosphorus terminal screens without resorting to dated skeuomorphism.

---

## 3. Typography
Typography is our primary tool for hierarchy. We pair the structural rigor of Monospace with the editorial clarity of Space Grotesk and Inter.

- **Display & Headlines (Space Grotesk):** These are your "Statement" layers. Use `display-lg` (3.5rem) with tight letter-spacing (-0.02em) to create an impactful, brutalist headline that feels "engineered."
- **Titles & Body (Inter):** Used for readability. `title-lg` and `body-lg` provide a neutral, sophisticated contrast to the high-energy headlines.
- **The CLI Layer (Monospace):** Reserved for code snippets, secondary labels (`label-md`), and metadata. This font should always be treated as "the data," while the sans-serif fonts are "the narrative."

---

## 4. Elevation & Depth
In this system, depth is a function of light and tone, not physical shadows.

### The Layering Principle
Achieve depth by "stacking." Place a `surface-container-lowest` card on a `surface-container-high` background. The slight shift in brightness creates a soft, natural lift.

### Ambient Shadows
If a floating element (like a modal terminal) requires a shadow, use an **Ambient Shadow**:
- Blur: 40px - 60px.
- Opacity: 4% - 6% of the `on-surface` color.
- Shadow Tint: Use a slight green tint (#002203) in the shadow to tie it back to the `primary` accent.

### The "Ghost Border" Fallback
If accessibility requirements demand a border (e.g., in a high-contrast mode), use a **Ghost Border**:
- Token: `outline-variant` (#b9ccb2) at 15% opacity. 
- **Rule:** Never use a 100% opaque border.

---

## 5. Components

### Terminal Windows
The centerpiece of the system.
- **Background:** `inverse-surface` (#313030).
- **Text:** `primary-container` (#00ff41) for commands, `surface` (#fcf9f8) for output.
- **Radius:** `md` (0.375rem).
- **Details:** No window controls (red/yellow/green dots). Use a simple `label-sm` monospace title in the top-left to identify the process.

### Buttons
- **Primary:** `primary` background with `on-primary` text. No shadow. On hover, transition to `primary-fixed-dim`.
- **Secondary:** `surface-container-highest` background. No border. Text in `on-surface`.
- **Tertiary:** Text-only, but with a `primary-container` (terminal green) underline that is 2px thick, offset by 4px.

### Input Fields
- **Styling:** Forgo the four-sided box. Use a `surface-container-low` background with a `2px` bottom-border of `primary-container`.
- **Focus:** The bottom-border transitions to `primary`.

### Cards & Lists
- **Rule:** Forbid divider lines.
- **Separation:** Use `spacing-lg` (vertical white space) or alternate background tints between `surface` and `surface-container-lowest`.

---

## 6. Do’s and Don’ts

### Do:
- **Embrace Asymmetry:** Let text blocks sit off-center to create a modern, editorial feel.
- **Use "Terminal Green" Sparingly:** It should feel like a laser-cutting through the monochrome to point at the most important action.
- **Maximize Whitespace:** If a section feels crowded, double the padding. This system relies on "breathing room" to feel premium.

### Don't:
- **No 1px Borders:** This is the most common way to make this system look "cheap" or "templated." 
- **No Heavy Shadows:** Avoid the "Google Material" look. If it looks like it’s floating 2 inches off the page, it’s too much.
- **No Generic Icons:** Use simple, line-based icons (1.5px stroke weight) that match the `monospace` font’s weight.
