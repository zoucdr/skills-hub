---
name: fashion-editorial-video-gen
version: 1.0.0
description: "Generate fashion editorial videos with WeryAI—bold composition, strong pose, unified color, magazine rhythm for outfits and brand portraits. Use when you need orbit, low-angle hero, walk-and-follow, quick beat cuts; runway or deep-house fashion beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [fashion, editorial, magazine, runway, pose, styling, portrait, outfit, bold, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Fashion editorial & magazine rhythm video

**Fashion editorial video** in one line: **assertive framing**, **sculptural pose**, **cohesive palette**, **silhouette-first** storytelling—**runway / print** energy. Camera: **orbit**, **low-angle power**, **walk-with follow**, **hard cut** on beat. Audio: **runway pulse**, **electronic fashion house**, **deep house** groove (generic).

**Style anchor:** `fashion_editorial_video`.

## Use when

- **Fashion editorial**, **strong pose**, **magazine composition**, **designer styling**, **bold visual rhythm**, lookbook motion.
- Text or **single image-to-video**; **9:16** default for vertical fashion.

**Not this skill:** **flat e-commerce packshot only** (use commercial-product) or **cozy vlog** default—unless hybrid requested.

**Models:** Kling Pro/Sta / Seedance **`720p`**. Kling **omit `resolution`**.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

## Prerequisites

`WERYAI_API_KEY`; Node 18+; **`https`** images; **`model`** required.

## Security, secrets, and trust

API file; consent for local images.

## Pre-submit gate (mandatory)

⚠️ **Full expanded `prompt`** + params approved explicitly.

## Workflow

Standard async **`submit-*`** → notify → **`status`**/**`wait`**; **`[Video](https://…)`** for output.

## CLI reference (minimal; details in API file)

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**Filter:** Kling no **`resolution`**; Seedance no **`negative_prompt`**.

## Prompt expansion (mandatory)

- **Look:** **unified grade** (warm, cool, or monochrome—pick one); **sharp tailoring read**; **negative space** for type-safe areas if user mentions graphics.
- **Pose / blocking:** **chin line**, **shoulder architecture**, **hand narrative**; **attitude** without cruelty.
- **Camera (1–2):** **orbit** at hip level; **worm's-eye** strut; **lateral follow** matching stride.
- **Audio:** **`Audio:`** **four-on-the-floor** fashion runway bed, **sub bass**, **shutter-click** SFX sparse—generic.
- **Anchors:** *fashion editorial, strong pose, magazine composition, designer styling, bold visual rhythm, fashion_editorial_video.*

## Definition of done

Pre-submit met; inline video link.

## Boundaries (out of scope)

Frozen tables; no trademarked show names.

### Example prompts

- `9:16 couture coat—fashion_editorial_video, low-angle orbit, unified cool grade, deep house fashion bed`
- `Walk-and-follow sidewalk—bold visual rhythm, quick cut on beat, runway pulse generic`
- `Animate lookbook still—strong pose, magazine composition (image URL)`

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
| ⭐ Best | `KLING_V3_0_PRO` | 10 | **Omit `resolution`.** Optional neg: `messy tourist snapshot, flat mall lighting, watermark` |
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

## Scenario: Text / image fashion editorial

Expand → pre-submit → submit.

## Loop seam (optional)

`seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `fashion-editorial-video-gen`.
