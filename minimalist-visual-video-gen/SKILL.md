---
name: minimalist-visual-video-gen
version: 1.0.0
description: "Generate minimalist visual videos with WeryAI—negative space, clean geometry, restrained palette, calm pacing for product, space, and art-forward shorts. Use when you need near-static frames, ultra-slow dolly, single-axis pan, measured crane; minimal piano or ambient beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [minimalist, negative-space, geometry, calm, design, product, art, restrained, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Minimalist visual & calm design video

**Minimalist visual video** in one line: **few elements**, **large negative space**, **clear composition**, **quiet rhythm**—**shape, line, and material** read over spectacle. Camera favors **lock-off or micro-move**, **ultra-slow push**, **single-direction pan**, **measured vertical lift**. Audio: **minimal piano**, **wide ambient**, **sparse minimal electronic** (generic).

**Style anchor:** `minimalist_visual_video`.

## Use when

- **Minimalist composition**, **negative space**, **clean geometry**, **restrained palette**, **calm design focus**, **gallery** or **brand quiet** pieces.
- Text or **single image-to-video**; **9:16** / **16:9** / **1:1**.

**Not this skill:** **busy collage**, **maximal neon**, **chaotic handheld** as default—unless overridden.

**Models:** **`KLING_V3_0_PRO`** / **`KLING_V3_0_STA`** / **`SEEDANCE_2_0`** (`720p`, no neg prompt). Kling: no **`resolution`**.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)** — full CLI. **`scripts/video_gen.js`** after pre-submit only.

## Prerequisites

`WERYAI_API_KEY`; Node 18+; **`https`** images; explicit consent for local paths; **`model`** required.

## Security, secrets, and trust

API file for hosts/images; never commit keys.

## Pre-submit gate (mandatory)

⚠️ Explicit OK on **full prompt** + params.

## Workflow

Brief → tier → aspect → **expand** → frozen validation → pre-submit → **`submit-*`** → notify → **`status`**/**`wait`** → **`[Video](url)`**.

## CLI reference (minimal; details in API file)

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)** — single source of truth.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**Filter:** Kling omit **`resolution`**; Seedance omit **`negative_prompt`**.

## Prompt expansion (mandatory)

- **Look:** **restrained palette** (one accent max), **matte or soft specular**, **large empty field**, **single hero form**.
- **Camera:** **almost static**; **creep dolly**; **one-axis pan**; **slow crane**—no whip unless user asks.
- **Audio:** **`Audio:`** **minimal piano** single notes, **soft room**, **bare electronic pulse**. Silent: off.
- **Anchors:** *minimalist composition, negative space, clean geometry, restrained palette, calm design focus, minimalist_visual_video.*

## Definition of done

Confirmed submit flow; inline video link.

## Boundaries (out of scope)

Field rules per frozen table; no duplicated API manual.

### Example prompts

- `16:9 single chair in white void—minimalist_visual_video, ultra-slow push, minimal piano`
- `9:16 ceramic vase—negative space, soft side light, single-axis pan, ambient bed`
- `Product cube—clean geometry, restrained palette, calm design focus (image URL)`

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
| ⭐ Best | `KLING_V3_0_PRO` | 10 (**15** for slower drift) | **Omit `resolution`.** Optional neg: `busy graphics, neon clutter, chaotic handheld, watermark` |
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

## Scenario: Text / image minimalist

Expand → pre-submit → **`submit-text`** or **`submit-image`**.

## Loop seam (optional)

`seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `minimalist-visual-video-gen`.
