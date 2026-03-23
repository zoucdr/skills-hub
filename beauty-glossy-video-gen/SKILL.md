---
name: beauty-glossy-video-gen
version: 1.0.0
description: "Generate beauty glossy videos with WeryAI—flawless skin read, controlled highlights, clean color, makeup macro for premium feminine-leaning aesthetics. Use when you need macro push, face orbit, tabletop sweep, slow close; gentle electronic, R&B, or light pop beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [beauty, makeup, skin, glossy, macro, feminine, premium, cosmetics, portrait, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Beauty glossy & makeup macro video

**Beauty glossy video** in one line: **refined skin texture**, **tasteful specular**, **clean color science**, **makeup micro-detail**—**premium glam** without plastic uncanny default. Camera: **macro push**, **slow face orbit**, **tabletop sweep** over products, **gentle close**. Audio: **soft electronic**, **R&B** groove, **fashion light pop** (generic).

**Style anchor:** `beauty_glossy_video`.

## Use when

- **Beauty glossy**, **flawless skin texture**, **soft highlights**, **polished makeup detail**, **feminine premium look**, skincare / cosmetics hero.
- Text or **single image-to-video**.

**Not this skill:** **gritty horror skin**, **heavy documentary pores** as default—unless user wants “raw beauty” hybrid.

**Models:** Kling Pro/Sta / Seedance. Kling **omit `resolution`**.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

## Prerequisites

`WERYAI_API_KEY`; Node 18+; **`https`** images; respect likeness; **`model`** required.

## Security, secrets, and trust

API file; local image consent.

## Pre-submit gate (mandatory)

⚠️ **Full prompt** + params explicitly approved.

## Workflow

Brief → tier → **`9:16`** default vertical beauty → expand → pre-submit → **`submit-*`** → notify → **`status`**/**`wait`** → **`[Video](url)`**.

## CLI reference (minimal; details in API file)

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**Filter:** Kling no **`resolution`**; Seedance no **`negative_prompt`**.

## Prompt expansion (mandatory)

- **Light:** **large soft source** + **tight eye catchlight**; **controlled gloss** on lips/cheek; **no harsh cross-shadow** unless dramatic brief.
- **Skin:** **natural pore micro-texture** with **polish**—avoid wax figure unless user wants extreme glam.
- **Product:** **brush**, **dropper**, **compact**—macro readable.
- **Audio:** **`Audio:`** **gentle electronic** pads, **R&B** snap, **soft airy vocals-optional bed** (instrumental generic).
- **Anchors:** *beauty glossy, flawless skin texture, soft highlights, polished makeup detail, feminine premium look, beauty_glossy_video.*

## Definition of done

Inline **`[Video](https://…)`**; ethics: no minors focused on adult glam unless user confirms appropriate context.

## Boundaries (out of scope)

Frozen JSON; no medical claims in prompts unless user provides compliant copy.

### Example prompts

- `9:16 skincare macro—beauty_glossy_video, slow orbit, soft key, R&B instrumental bed generic`
- `Lip gloss close—polished makeup detail, macro push, clean color`
- `Vanity tabletop sweep—beauty glossy, gentle electronic (image URL)`

---

## Model and API constraints (frozen for this skill)

> 2026-03-23 snapshot.

### Text-to-video

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | prompt limit |
|-----------|-----------|---------------|-------------|-------|-----------------|--------------|
| `KLING_V3_0_PRO` | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit)* | Yes | Yes | 2000 |
| `KLING_V3_0_STA` | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit)* | Yes | Yes | 2000 |
| `SEEDANCE_2_0` | 5–15 | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No | 2000 |

### Image-to-video (single `image`)

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | Notes |
|-----------|-----------|---------------|-------------|-------|-----------------|--------|
| `KLING_V3_0_PRO` | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | Yes | Yes | single `image` |
| `KLING_V3_0_STA` | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | Yes | Yes | single `image` |
| `SEEDANCE_2_0` | 5–15 | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No | `upload_image_limit` **3** |

---

## Recommended models

| Tier | model_key | Default duration | Notes |
|------|-----------|------------------|--------|
| ⭐ Best | `KLING_V3_0_PRO` | 10 | **Omit `resolution`.** Optional neg: `harsh flash, orange skin cast, heavy pores horror, watermark` |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** |
| ⚡ Fast | `SEEDANCE_2_0` | 10 | **`720p`**. **No `negative_prompt`** |

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` |
| `aspect_ratio` | **`9:16`** default |
| `duration` | `10` |
| `resolution` | **Seedance:** **`720p`** |
| `generate_audio` | **`true`** unless silent |
| `negative_prompt` | **Kling only** |

## Scenario: Text / image beauty glossy

Expand → pre-submit → submit.

## Loop seam (optional)

`seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `beauty-glossy-video-gen`.
