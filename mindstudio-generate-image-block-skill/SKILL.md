---
name: mindstudio-generate-image-block-skill
description: Write, configure, and optimize prompts for MindStudio's Generate Image block. Use this skill whenever someone wants to generate an image inside a MindStudio workflow, write an image generation prompt, use a variable in an image prompt, choose an AI image model, access the output image URL, resize or transform an image via CDN, or connect image generation to a downstream block. Always use this skill for any MindStudio image generation task, even if the request seems simple.
---

# MindStudio Generate Image Block Skill

A skill for configuring and writing production-ready prompts for MindStudio's Generate Image block — covering prompt structure, dynamic variables, model selection, output URL handling, and CDN image transformations.

---

## Step 1: Interview the User First

Before writing a single line of prompt, gather context. The right prompt depends on what the image is for, what variables exist upstream, and what happens to the image URL after it's generated.

Ask as many of these as apply:

**About the image:**
- What should the image depict? (subject, scene, mood, style)
- Is the image content fixed, or should it vary based on workflow variables?
- What art style or aesthetic are you going for? (photorealistic, illustrated, cinematic, minimal, etc.)

**About the variables:**
- What variables are available at this point in the workflow?
- What are their exact names (e.g., `productName`, `userDescription`, `topic`)?

**About the output:**
- What variable name will you use to store the image URL?
- What happens next — does the image URL get displayed, sent to an API, injected into an interface, or passed to a downstream block?
- Do you need to resize or reformat the image using CDN parameters?

Never invent variable names. Use exactly what the user has defined in their workflow.

---

## How the Generate Image Block Works

The Generate Image block sends a text prompt to an AI image model and returns a URL pointing to the generated image.

**Key configurations:**

| Setting | What it does |
|---|---|
| Prompt | Text description of the image. Supports `{{variables}}`. |
| Source Image | Optional. Feed an existing image URL into the block as a reference or base. Supports `{{variables}}`. |
| Output Variable | Name of the variable that stores the returned image URL. |
| Model | The AI image model to use (e.g., DALL-E 3, Stable Diffusion, Flux). |
| Model-specific settings | Each model may expose additional options — negative prompt, style, quality, rendering mode. Always review the selected model's settings. |

The block outputs a single image URL. That URL can be injected into other blocks using `{{outputVariableName}}`.

---

## Using the Source Image Field

The **Source Image** field lets you pass an existing image into the block alongside your text prompt. This unlocks image-to-image generation — transforming, restyling, or extending an existing image rather than generating from scratch.

### When to use it

- **Style transfer** — take a user-uploaded photo and restyle it (e.g. "make this look like a watercolor painting")
- **Product variations** — use a base product image and generate color or context variations
- **Avatar or portrait editing** — take an existing headshot and apply changes via the prompt
- **Iterative refinement** — feed a previously generated `{{image}}` back in and refine it with a new prompt

### What to pass in

The Source Image field accepts a URL. This can be:

- A hardcoded URL to a fixed reference image
- A `{{variable}}` holding a URL from a previous block (e.g. a user upload, a prior Generate Image output, or an HTTP request response)

### Example patterns

**User uploads a photo, workflow restyled it:**
```
Source Image: {{userUpload}}
Prompt: Restyle this portrait as a vintage oil painting. Warm, muted tones. Painterly brush strokes. Keep the subject's likeness intact.
```

**Chain two Generate Image blocks — refine the first output:**
```
Source Image: {{firstImage}}
Prompt: Same composition, but change the background to a snowy mountain landscape. Keep the subject and lighting unchanged.
```

**Fixed reference image with dynamic prompt:**
```
Source Image: https://images.mindstudio-cdn.com/assets/base-product.jpg
Prompt: Show this product in a {{setting}} environment. Photorealistic. Match the existing product lighting.
```

### Model support

Not all models support the Source Image field. Check the selected model's settings panel — if Source Image is not supported, the field may be hidden or ignored. Models based on img2img pipelines (common in Stable Diffusion and Flux variants) typically support it. DALL-E 3 has limited image input support.

### Interview addition for Source Image

When interviewing the user, add this question if image-to-image seems relevant:

> "Do you have an existing image you want to use as a starting point, or are you generating from scratch?"

If yes, ask:
- Where is that image coming from — a user upload, a previous block's output, or a fixed URL?
- What exact variable holds it, or what is the URL?

---

## Writing Effective Image Prompts

### Think Like a Director, Not a Typist

The difference between a weak and a strong prompt is not more words — it's better direction. Name the scene, not just the object.

- **Weak:** "A woman walking on a street."
- **Strong:** "Cinematic shot of a woman walking alone on a rainy Paris street at night, neon reflections on wet pavement, moody atmosphere, shallow depth of field, 35mm film look."

What changed: camera, lighting, mood, environment — not just more adjectives.

---

### The Universal Prompt Formula

Use this as the default structure for any model unless a model-specific framework applies (see Model-Specific Prompting below).

```
[Subject + Action] + [Setting / Environment] + [Mood / Atmosphere] + [Lighting] + [Camera / Composition] + [Style / Medium] + [Color Palette]
```

Not every prompt needs all seven. Prioritize subject first, style last.

**Example:**
```
A lone samurai standing on a foggy mountain ridge, stoic and timeless mood,
soft dawn light with diffused mist, wide shot with deep depth of field,
cinematic realism style, muted greens and grey-blue tones. 16:9.
```

---

### The Seven Components

**1. Subject + Action**
Who or what is in the image, and what are they doing?
> "a lone samurai standing", "a product floating in space", "a woman laughing"

Use strong verbs — "surges", "unfurls", "shatters" imply motion and drama. "Standing" is fine; "charges through" is cinematic.

**2. Setting / Environment**
Where is the scene? What surrounds the subject?
> "foggy mountain ridge", "rain-soaked Paris street at night", "minimalist white studio"

Add time and weather cues — "at dusk", "in heavy rain", "fog drifting" — they add atmosphere instantly.

**3. Mood / Atmosphere**
How should the viewer feel?
> "stoic and timeless", "nostalgic and warm", "tense and electric", "dreamlike"

Replace generic words ("cool", "nice") with emotional direction. Models react strongly to tone.

**4. Lighting**
How is the scene lit?
> "soft natural window light from the left", "golden hour backlighting", "dramatic side lighting", "three-point softbox setup", "Chiaroscuro lighting"

Lighting is the single biggest shaper of mood. Always specify it.

**5. Camera / Composition**
How is the shot framed?
> "wide establishing shot", "low-angle hero shot", "close-up with shallow depth of field", "overhead perspective", "rule of thirds", "85mm lens"

Standard cinematic and photography terms work well across all major models.

**6. Style / Medium**
What is the visual aesthetic?
> "photorealistic", "watercolor illustration", "flat design", "cinematic still", "oil painting", "graphic novel", "anime cel-shading", "3D render"

If you don't specify a style, results will be inconsistent. Always pick one direction and commit.

**7. Color Palette**
What dominant colors set the tone?
> "warm oranges and golds", "muted earth tones", "deep navy and amber"

---

### Style Keyword Reference

Embed these naturally in a sentence — not stacked as a tag list.

| Style | Keywords to use |
|---|---|
| Photorealistic | "photoreal detail", "natural lighting", "lifelike textures", "documentary feel" |
| Cinematic | "volumetric lighting", "film grain", "dynamic shadows", "epic wide shot", "35mm film look" |
| Anime | "vibrant cel-shading", "expressive linework", "bright anime palette" |
| Painterly | "oil-painting texture", "lush brushstrokes", "impressionist glow" |
| Surreal | "dreamlike distortion", "ethereal glow", "otherworldly hues" |
| Graphic Novel | "inked outlines", "bold contrasts", "neon-charged lines" |
| Minimalist | "clean composition", "negative space", "flat lighting", "editorial feel" |

---

### Iteration Strategy

Change one variable at a time:

- Version 1: "Portrait of a woman with flowers."
- Version 2: "Portrait of a woman holding yellow tulips under warm light."
- Version 3: "Cinematic portrait of a woman holding yellow tulips, shallow depth of field, 85mm lens look, gentle morning glow."

Keep the core scene stable. Adjust lighting, framing, or mood words one at a time.

---

## Prompt Templates

### Static Prompt (no variables)

```
A hyper-realistic photograph of a steaming cup of black coffee on a minimalist
white marble surface. Soft natural light from the left. Shallow depth of field.
Clean, editorial feel. Color palette: warm cream and deep espresso brown.
```

### Dynamic Prompt (with variables)

```
A professional product photograph of {{productName}} on a clean white background.
Studio lighting. Sharp focus. Premium and modern feel.
Color palette: white, silver, and subtle grey gradients.
```

```
A cinematic portrait of {{personRole}} in a {{industry}} setting.
Mood: focused and ambitious. Flat illustration style with bold outlines.
Color palette: navy, white, and amber accents.
```

### Pass-through (user writes the full prompt)

```
{{userPrompt}}
```

### Enriched pass-through (user writes the core, skill appends technical direction)

```
{{userPrompt}}

Shot on 85mm lens. Shallow depth of field. Cinematic color grading.
Photorealistic quality. Sharp focus.
```

### Multi-variable prompt

```
A detailed {{artStyle}} illustration of {{subject}} set in {{setting}}.
Mood: {{mood}}. Color palette: {{colorPalette}}. Composition: {{compositionStyle}}.
```

---

## Common Pitfalls to Avoid

| Pitfall | Fix |
|---|---|
| Too vague ("a tree") | Add setting, lighting, style, mood, and color |
| Tag stacking ("epic, 8K, knight, castle") | Write a directed sentence with intent |
| No style specified | Always include one clear style direction |
| Weak or no lighting | Lighting is non-negotiable — always include it |
| Conflicting aesthetics | Pick one direction and commit |
| Missing time or weather | Add "at dusk", "in fog", "on a rainy night" |
| Vague verbs ("standing") | Use action verbs ("charges through", "emerges from") |
| Prompt too long (Seedream) | Keep to 30-100 words for Seedream v4.5 |

---

## Model Selection and Model-Specific Prompting

When the user specifies a model, apply the model-specific framework below. When no model is specified, use the Universal Prompt Formula and combine the best principles from all three.

---

### Grok Imagine (xAI)

**How it thinks:** Natural language — write a scene description, not a keyword list. Grok responds well to narrative sentences with embedded direction.

**Formula:**
```
[Shot type / camera] of [subject + action] in [setting], [mood] atmosphere,
[lighting], [style keywords]. [Aspect ratio].
```

**Key strengths:**
- Emotion-driven adjectives ("nostalgic", "melancholic", "electric", "tense")
- Cinematic framing terms ("low-angle shot", "over-the-shoulder", "wide establishing shot")
- Strong verbs ("surges", "unfurls", "shatters")
- Time and weather cues ("at dusk", "fog drifting", "heavy rain")

**Editing prompts (image-to-image):** Explicitly lock what stays the same, then list changes.
```
Keep the subject's face, pose, and layout unchanged.
Add [new element]. Do not change camera angle or composition.
```

**Example:**
```
Cinematic shot of a woman walking alone on a rainy Paris street at night,
neon reflections on wet pavement, moody and melancholic atmosphere,
shallow depth of field, 35mm film look. 16:9.
```

---

### Gemini Nano Banana (Google)

**How it thinks:** Deep reasoning — it fully understands the prompt before generating. Supports real-time web search, text rendering, and multimodal (multiple reference image) inputs.

**Formula:**
```
[Subject] + [Action] + [Location/context] + [Composition] + [Style]
```

**Key strengths:**
- Multimodal: mix up to 14 reference images per prompt
- Text rendering in 10+ languages — enclose text in quotes, name the font
- Real-time web search integration for localized or current-data visuals
- Conversational editing — refine with follow-up prompts
- Native aspect ratios: 1:1, 3:2, 9:16, 16:9, 21:9 and more
- Resolutions up to 4K

**Lighting as creative direction:**
> "three-point softbox setup", "Chiaroscuro lighting", "Golden hour backlighting", "1980s color film, slightly grainy"

**Text rendering tip:** Generate the text concept first in a Generate Text block, then pass it via `{{textConcept}}`.

**Example:**
```
A striking fashion model wearing a tailored brown dress, posing with a confident
stance, slightly turned. Seamless deep cherry red studio backdrop. Medium-full shot,
center-framed. Fashion magazine editorial style, medium-format analog film,
pronounced grain, high saturation, cinematic lighting.
```

---

### Seedream v4.5 (ByteDance)

**How it thinks:** Keyword-aware — places greater emphasis on concepts mentioned earlier. Keep prompts focused and ordered by priority.

**Formula:**
```
[Subject Description] + [Style Specification] + [Compositional Details] + [Lighting & Atmosphere] + [Technical Parameters]
```

**Key strengths:**
- Photorealistic output at up to 4K
- Highly responsive to explicit style descriptors and lighting cues
- Strong compositional control with standard photography terms
- Multi-image blending (two images at a time, iteratively)

**Critical rules for Seedream:**
- Keep prompts to **30-100 words** — longer prompts cause confusion
- Put the subject **first**
- Use positive framing — "empty street" not "no cars"
- For inconsistent subject results: move subject to the very start and simplify

**Lighting keywords:**
> "golden hour lighting", "dramatic side lighting", "moody low-key lighting", "bright and airy high-key lighting"

**Example:**
```
A fluffy orange tabby cat sitting on a windowsill, soft morning light streaming in,
cinematic composition, shallow depth of field, 85mm lens, photorealistic style.
```

---

### No model specified — Universal approach

Combine the strongest principles from all three:

1. Write a narrative sentence (Grok style) — not a tag list
2. Use the 7-component formula as a checklist
3. Put the subject first (Seedream rule)
4. Include emotion-driven mood words (Grok strength)
5. Specify lighting explicitly (universal)
6. Add camera and composition terms (universal)
7. Name one clear style direction (universal)
8. Keep it under 120 words unless the model is known to handle long prompts

---

### Model settings to always review in MindStudio

After selecting any model, check the block's settings panel for:

| Setting | What to look for |
|---|---|
| Negative prompt | What to exclude from the image |
| Quality / rendering mode | Standard vs. HD |
| Style preset | Vivid vs. natural |
| Aspect ratio | Match to your display target |
| Resolution | 1K / 2K / 4K if available |

---

## Output Variable and Using the Image URL

When the block runs, it saves the generated image URL to the variable name you specify in **Output Variable**.

Example: if you name it `generatedImage`, you can reference it downstream as:

```
{{generatedImage}}
```

### Common downstream uses

**Display to user in a chat or interface:**
Inject `{{generatedImage}}` into a Display block or a Generate Text block that returns HTML with an `<img>` tag.

**Pass to an HTTP Request block:**
Use `{{generatedImage}}` as a body field value when sending the URL to an external API or webhook.

**Use in an interface component:**
In a MindStudio Interface Designer block, bind the image `src` attribute to `{{generatedImage}}`.

**Log to a spreadsheet or database:**
Pass `{{generatedImage}}` as a field value in an HTTP POST to Airtable, Google Sheets, or similar.

---

## CDN Image Transformation

MindStudio serves generated images from its CDN at:

```
https://images.mindstudio-cdn.com/
```

You can append query parameters to the URL to resize, reformat, or crop images dynamically — without regenerating them.

### Supported Parameters

| Parameter | What it does | Example |
|---|---|---|
| `w` | Sets width in pixels | `?w=400` |
| `h` | Sets height in pixels | `?h=300` |
| `fm` | Changes file format | `?fm=webp` (also: `avif`, `jpeg`, `auto`) |
| `fit` | Controls how the image fills the dimensions | `?fit=crop` or `?fit=cover` (default) |

### How to combine parameters

Append `?` followed by parameters joined with `&`:

```
https://images.mindstudio-cdn.com/path/to/image.jpg?w=400&h=300&fm=webp&fit=crop
```

### When to use CDN parameters

- **Displaying in a UI:** Resize to the exact pixel dimensions of the container
- **Sending to an API:** Some APIs have image size limits — use `w` and `h` to comply
- **Optimizing for web:** Use `fm=auto` to let the CDN choose the best format per device
- **Thumbnails:** Use `fit=crop` to get a tightly cropped thumbnail at fixed dimensions

### Building a dynamic CDN URL in a workflow

If `generatedImage` holds the raw CDN URL, you can append parameters directly in a Generate Text or Display block:

```
{{generatedImage}}?w=800&fm=webp
```

Or construct a full URL string in a Generate Text block and assign it to a new variable for use downstream.

---

## Quick Checklist Before Finalizing

- [ ] Is the subject clearly defined?
- [ ] Is an art style or medium specified?
- [ ] Is the mood or atmosphere described?
- [ ] Is a color palette included?
- [ ] Are any `{{variables}}` using the exact names from the workflow?
- [ ] Is the Output Variable name set in the block settings?
- [ ] If using Source Image, does the selected model support img2img input?
- [ ] If using Source Image, is the variable holding the URL correctly referenced (e.g. `{{userUpload}}`)?
- [ ] Have you reviewed the selected model's additional settings (negative prompt, quality, aspect ratio)?
- [ ] If the image URL will be displayed or passed downstream, is `{{outputVariableName}}` referenced correctly in the next block?
- [ ] If CDN resizing is needed, are the correct query parameters appended to the URL?
