---
name: commercial-product-video-gen
version: 1.0.0
description: "Generate commercial clean product videos with WeryAI—even light, crisp materials, clear hero read, premium advertising look for promos and brand spots. Use when you need slider pan, macro push, turntable orbit, detail cuts; light electronic with soft drums; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [commercial, product, advertising, clean, premium, macro, turntable, brand, promo, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Commercial clean & product advertising video

**Commercial clean video** in one line: **even sophisticated light**, **material fidelity**, **sharp hero clarity**, **sell-the-benefit** framing—**premium ad** grammar, not gritty doc. Camera: **slider pan**, **macro dolly**, **turntable**, **detail inserts**. Audio: **light electronic**, **soft branded-feel drums** (generic—no real brand audio).

**Style anchor:** `commercial_product_video`.

## Use when

- **Commercial clean**, **high detail**, **polished product lighting**, **premium advertising look**, **short promo**, **SKU hero**.
- Text or single **image-to-video**; **9:16** / **16:9** / **1:1**.

**Not this skill:** **VHS glitch**, **underexposed horror**, **muted luxury editorial** as default—unless user overrides toward hybrid.

**Models:** Kling Pro/Sta / Seedance **`720p`**. Kling: **omit `resolution`**, **omit `negative_prompt`** on Seedance.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**. **`scripts/video_gen.js`** post pre-submit.

## Prerequisites

`WERYAI_API_KEY`; Node 18+; **`https`** images; **`model`** required.

## Security, secrets, and trust

API file; no embedded secrets.

## Pre-submit gate (mandatory)

⚠️ Explicit approval: **full expanded `prompt`** + table.

## Workflow

Standard: brief → tier → aspect → expand → validate → pre-submit → **`submit-*`** → notify → **`status`**/**`wait`** → **`[Video](https://…)`**.

## CLI reference (minimal; details in API file)

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**Filter:** Kling no **`resolution`**; Seedance no **`negative_prompt`**.

## Prompt expansion (mandatory)

- **Light:** **softbox or strip** feel, **controlled specular**, **subtle gradient backdrop**, **rim for separation**.
- **Product:** **hero angle**, **logo-safe negative space** (no fake logos unless user supplies legal asset context), **feature-facing**.
- **Camera:** **slider parallel** to label; **macro creep** on texture; **360 turntable** even speed; **cut-friendly** beats.
- **Audio:** **`Audio:`** **light electronic** pulse, **soft kick**, **airy whoosh** on transitions—generic.
- **Anchors:** *commercial clean, high detail, polished product lighting, premium advertising look, commercial_product_video.*

## Definition of done

Inline **`[Video](https://…)`**; confirmed flow.

## Boundaries (out of scope)

Frozen JSON rules; one API doc.

### Example prompts

- `9:16 skincare bottle—commercial_product_video, turntable, macro cap thread, soft strip light, light electronic bed`
- `16:9 laptop hero—slider pan, polished product lighting, premium advertising look`
- `Animate packshot—detail cut rhythm, soft drums generic (image URL)`

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
| ⭐ Best | `KLING_V3_0_PRO` | 10 | **Omit `resolution`.** Optional neg: `gritty documentary, heavy grain, muddy exposure, watermark` |
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

## Scenario: Text / image commercial

Expand sell points → pre-submit → submit.

## Loop seam (optional)

`seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `commercial-product-video-gen`.
