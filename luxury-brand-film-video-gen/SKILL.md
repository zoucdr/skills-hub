---
name: luxury-brand-film-video-gen
version: 1.0.0
description: "Generate luxury advertising film videos with WeryAI—understated opulence, tactile materials, refined highlights, slow ritual motion for watches, jewelry, fragrance, couture. Use when you need macro sweep, silk-smooth orbit, slow push, detail inserts; minimal classical or fashion ambient beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [luxury, premium, jewelry, watch, fragrance, couture, slow, elegant, tactile, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Luxury brand film & quiet opulence video

**Luxury brand film** in one line: **quiet expense**, **material truth** (metal, glass, silk, stone), **refined micro-highlights**, **ceremonial slowness**—**exclusive mood**, not loud retail. Camera: **macro glide**, **butter-smooth orbit**, **slow dolly-in**, **tight inserts**. Audio: **sparse classical-adjacent strings**, **fashion ambient electronic** (generic).

**Style anchor:** `luxury_brand_film`.

## Use when

- **Luxury advertising**, **premium texture**, **refined highlights**, **elegant motion**, **exclusive mood**—watches, gems, perfume, high fashion.
- Text or **single image-to-video**.

**Not this skill:** **flash-sale neon**, **shouty promo** default, **budget bright**—unless overridden.

**Models:** Kling Pro/Sta / Seedance. Kling **omit `resolution`**.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

## Prerequisites

`WERYAI_API_KEY`; Node 18+; **`https`** images; **`model`** required.

## Security, secrets, and trust

Per API file; no secrets in skills.

## Pre-submit gate (mandatory)

⚠️ **Full prompt** + params explicitly approved.

## Workflow

Brief → tier → aspect → **expand** → frozen check → pre-submit → **`submit-*`** → notify → **`status`**/**`wait`** → **`[Video](url)`**.

## CLI reference (minimal; details in API file)

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**Filter:** Kling no **`resolution`**; Seedance no **`negative_prompt`**.

## Prompt expansion (mandatory)

- **Look:** **restrained contrast**, **tight highlight islands**, **deep rich blacks** or **soft velvet roll-off** per brief; **tactile** read.
- **Motion:** **slow**, **no snap** unless user wants tension; **breathing** pace.
- **Audio:** **`Audio:`** **solo piano** motif or **sparse strings**, **room hush**, **silk foley**—generic.
- **Anchors:** *luxury advertising, premium texture, refined highlights, elegant motion, exclusive mood, luxury_brand_film.*

## Definition of done

Confirmed; playable inline link.

## Boundaries (out of scope)

Same frozen rules as sibling skills; no IP logos unless user owns.

### Example prompts

- `9:16 watch macro—luxury_brand_film, slow orbit, brushed titanium read, minimal strings bed`
- `Fragrance bottle—refined highlights, silk-smooth macro sweep, fashion ambient electronic`
- `Couture fabric drape—elegant motion, exclusive mood (image URL)`

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
| ⭐ Best | `KLING_V3_0_PRO` | 10 (**15** for slow ritual) | **Omit `resolution`.** Optional neg: `loud retail stickers, flash sale graphics, harsh clipping, watermark` |
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

## Scenario: Text / image luxury

Expand ritual beats → submit.

## Loop seam (optional)

`seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `luxury-brand-film-video-gen`.
