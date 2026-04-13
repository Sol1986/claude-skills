# examples.md — Generate Image Block Skill

Real prompt examples organized by use case and model. All examples follow the skill's interview-first process and 7-component formula.

---

## Example 1 — Photorealistic Pet Portrait (Luma Photon 1)

**Use case:** Fixed prompt, displayed in a MindStudio UI at 16:9

**Original prompt:**
```
A photorealistic portrait of a golden retriever sitting in a sunlit meadow.
Warm golden hour lighting from the right. Shallow depth of field, 85mm lens.
The dog is looking directly at the camera with a friendly, relaxed expression.
Lush green grass in the soft-focus background. Color palette: warm golds,
greens, and amber. Cinematic, heartwarming atmosphere.
```

**Optimized for Luma Photon 1:**
```
A heartwarming photorealistic portrait of a golden retriever sitting peacefully
in a sun-drenched meadow, gazing directly into the camera with a calm and
friendly expression. Warm golden hour light falls softly from the right,
illuminating individual fur strands with a gentle glow. Shot on an 85mm lens
with shallow depth of field — lush green grass dissolves into soft amber
and gold bokeh behind the subject. Color palette: warm golds, honey amber,
and deep greens. Cinematic, emotionally warm atmosphere. 16:9.
```

**Output Variable:** `dogImage`
**Aspect ratio:** 16:9

**Why it works for Photon 1:**
- "gazing directly" is more emotive than "looking directly" — Photon responds to emotional language
- "illuminating individual fur strands" gives the model fine detail to render
- "dissolves into soft amber bokeh" is more directed than "soft-focus background"
- "emotionally warm atmosphere" positions mood as a directive, not a label

---

## Example 2 — Same Scene, Grok Imagine (xAI)

**Use case:** Same golden retriever prompt, switched to Grok Imagine model

**Optimized for Grok Imagine:**
```
Cinematic wide shot of a golden retriever sitting in a sun-drenched meadow,
gazing directly into the camera with a calm and friendly expression.
Warm golden hour light sweeps in from the right, casting long soft shadows
across the grass. Shallow depth of field, 85mm lens — lush green grass
melts into warm amber bokeh behind the subject. Heartwarming, nostalgic
atmosphere. Photoreal style. Color palette: warm golds, honey amber,
deep greens. 16:9.
```

**Output Variable:** `dogImage`

**Why it works for Grok:**
- Led with shot type ("Cinematic wide shot") — Grok wants camera direction first
- "sweeps in from the right, casting long soft shadows" — atmospheric lighting language
- "melts into warm amber bokeh" — action verb, Grok responds to motion-implied language
- "Heartwarming, nostalgic" — emotion-driven adjectives are Grok's sweet spot

---

## Example 3 — Dynamic Product Image (Universal / No model specified)

**Use case:** Variable-driven product shot, output sent to an HTTP Request block

**Prompt:**
```
A professional product photograph of {{productName}} on a clean white background.
Soft diffused studio lighting from above. Sharp focus throughout. Premium and
modern feel, as if shot for a luxury brand lookbook. Color palette: white,
silver, and subtle grey gradients. Editorial style. 1:1.
```

**Output Variable:** `productImage`

---

## Example 4 — Style Transfer with Source Image (img2img)

**Use case:** User uploads a headshot, workflow restyled it as a watercolor portrait

**Source Image:** `{{userUpload}}`

**Prompt:**
```
Restyle this portrait as a soft watercolor illustration. Warm, muted tones.
Visible brushstrokes with gentle color blending. Keep the subject's likeness,
expression, and composition intact. Do not change the framing or background layout.
Painterly, impressionist feel. Color palette: warm peach, ivory, and soft blue.
```

**Output Variable:** `styledPortrait`

---

## Example 5 — Iterative Refinement (chaining two Generate Image blocks)

**Use case:** First block generates a base image, second block refines it

**Block 1 prompt:**
```
A minimalist flat-lay photograph of a leather journal, a fountain pen, and a
single sprig of eucalyptus on a warm cream linen surface. Soft natural light
from the left. Color palette: cream, tan, and forest green. Editorial style. 1:1.
```

**Block 1 Output Variable:** `baseImage`

**Block 2 — Source Image:** `{{baseImage}}`

**Block 2 prompt:**
```
Same composition and layout. Add a small ceramic espresso cup in the top right
corner. Keep all existing objects, lighting, and color palette unchanged.
Do not alter the camera angle or composition.
```

**Block 2 Output Variable:** `refinedImage`

---

## Example 6 — Cinematic Scene, Seedream v4.5 (ByteDance)

**Use case:** Fixed cinematic scene, displayed fullscreen at 16:9

**Prompt (kept under 100 words per Seedream rules):**
```
A lone warrior standing on a fog-covered cliff at dawn, silhouetted against
a pale golden sky. Cinematic realism style. Epic wide shot, low angle.
Volumetric morning light breaking through mist. Color palette: soft gold,
grey-blue, and deep charcoal. Film grain. 16:9.
```

**Output Variable:** `heroImage`

**Why it works for Seedream:**
- Subject is first ("A lone warrior")
- Under 100 words
- Positive framing throughout — no negations
- One clear style direction (cinematic realism)

---

## Example 7 — Text Rendering, Gemini Nano Banana (Google)

**Use case:** Typographic poster with rendered text, generated from a text concept variable

**Generate Text block (runs first):** Produces `{{posterText}}` — e.g. "The Future Is Now"

**Generate Image prompt:**
```
A bold typographic poster on a solid black background. Large centered letters
spell "{{posterText}}", filling the frame. The text acts as a cut-out window —
a photograph of a neon-lit city at night is visible only inside the letterforms.
High contrast. Graphic design style. Color palette: black, white, and electric blue. 9:16.
```

**Output Variable:** `posterImage`

**Why it works for Gemini:**
- Text in quotes triggers Gemini's text rendering capability
- Scene described narratively with subject, context, and style
- Aspect ratio specified (9:16 for poster format)
- Dynamic variable `{{posterText}}` injected cleanly

---

## Pitfall examples

**Tag stacking — avoid this:**
```
golden retriever, meadow, 8K, epic, photorealistic, bokeh, HDR, cinematic
```

**Directed sentence — do this instead:**
```
Cinematic wide shot of a golden retriever sitting in a sun-drenched meadow,
gazing into the camera, warm golden hour light from the right, shallow depth
of field, photoreal style, warm golds and amber. 16:9.
```
