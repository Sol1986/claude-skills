# mindstudio-generate-image-block-skill

A MindStudio skill for configuring and writing production-ready prompts for the **Generate Image block** — covering prompt structure, dynamic variables, model-specific frameworks, Source Image (img2img), CDN transformations, and downstream URL handling.

---

## What it does

When triggered, this skill:

1. Interviews the user before writing a single word of prompt
2. Detects the target model and applies the correct prompting framework
3. Optimizes or writes the image prompt using director-style language
4. Configures the Output Variable and CDN display URL if needed
5. Flags model-specific settings to review in the MindStudio block panel

---

## Supported models

The skill includes model-specific prompting frameworks for:

| Model | Approach |
|---|---|
| **Grok Imagine** (xAI) | Narrative sentences, emotion-driven adjectives, cinematic framing |
| **Gemini Nano Banana** (Google) | Deep reasoning, multimodal references, text rendering, conversational editing |
| **Seedream v4.5** (ByteDance) | Subject-first ordering, 30-100 word limit, positive framing, iterative blending |
| **Luma Photon 1** (Luma Labs) | Natural language, strong prompt adherence, fine detail direction |
| **Unknown / unlisted model** | Universal 7-component formula combining best practices from all models |

---

## Triggers

Use this skill whenever someone:

- Wants to generate an image inside a MindStudio workflow
- Needs to write or optimize an image generation prompt
- Asks how to use a variable in an image prompt
- Wants to use Source Image (img2img) in a workflow
- Needs to resize or reformat an image URL using CDN parameters
- Asks how to pass a generated image URL to a downstream block

---

## What's covered

- Universal 7-component prompt formula (subject, setting, mood, lighting, camera, style, color)
- Director-style prompting principles (think scene, not object)
- Style keyword reference (photorealistic, cinematic, anime, painterly, surreal, graphic novel, minimalist)
- Iteration strategy (one variable at a time)
- Static, dynamic, pass-through, enriched, and multi-variable prompt templates
- Source Image field — when, why, and how to use it
- Model-specific prompting frameworks with examples
- MindStudio CDN image transformation parameters (`w`, `h`, `fm`, `fit`)
- Downstream URL usage (Display block, Interface Designer, HTTP Request, logging)
- Pre-flight checklist

---

## Installation

1. Copy `SKILL.md` into your Claude skills directory
2. The skill triggers automatically when image generation requests are detected

---

## Files

```
mindstudio-generate-image-block-skill/
├── SKILL.md       — Main skill instructions
├── README.md      — This file
├── examples.md    — Prompt examples by use case and model
└── skill.json     — Skill metadata
```

---

## Related skills

- [mindstudio-generate-text-block-prompting-skill](https://github.com/yourusername/mindstudio-generate-text-block-prompting-skill)
- [mindstudio-http-request-block](https://github.com/yourusername/mindstudio-http-request-block)
- [mindstudio-interface-designer-skill](https://github.com/yourusername/mindstudio-interface-designer-skill)

---

## Author

Built by Sol — MindStudio builder and educator.
