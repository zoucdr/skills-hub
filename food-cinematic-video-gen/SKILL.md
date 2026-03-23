---
name: food-cinematic-video-gen
version: 1.0.0
description: "Generate food cinematic videos with WeryAI—steam, sheen, appetite macro, warm tones, cooking rhythm for restaurant and food content. Use when you need macro push, lateral slide, top-down press, orbit around dish; light jazz, funk, or warm lo-fi beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [food, culinary, steam, macro, appetite, restaurant, cooking, warm, cinematic, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Food cinematic & appetite macro video

**Food cinematic video** in one line: **steam ribbons**, **oil sheen**, **toothsome texture**, **warm appetizing grade**—**slow-food** spectacle. Camera: **macro dolly**, **lateral slide** across board, **overhead gentle press**, **orbit** around plated hero. Audio: **light jazz**, **funky bass**, **warm lo-fi** kitchen ambience (generic).

**Style anchor:** `food_cinematic_video`.

## Use when

- **Food cinematic**, **delicious texture**, **steam**, **rich warm tones**, **close-up appetizing detail**, recipe / restaurant promos.
- Text or **single image-to-video**.

**Not this skill:** **clinical nutrition label** flat look or **horror spoilage**—unless user overrides.

**Models:** Kling Pro/Sta / Seedance **`720p`**. Kling **omit `resolution`**.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

## Prerequisites

`WERYAI_API_KEY`; Node 18+; **`https`** images; **`model`** required.

## Security, secrets, and trust

API file; consent for local uploads.

## Pre-submit gate (mandatory)

⚠️ **Full expanded `prompt`** + params explicitly approved.

## Workflow

Brief → tier → aspect → expand → pre-submit → **`submit-*`** → notify → **`status`**/**`wait`** → **`[Video](https://…)`**.

## CLI reference (minimal; details in API file)

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**Filter:** Kling no **`resolution`**; Seedance no **`negative_prompt`**.

## Prompt expansion (mandatory)

- **Look:** **warm key** + **soft bounce**; **steam backlit**; **glistening sauce**; **crumb and fiber** micro read.
- **Motion:** **slow pour**, **fork tension**, **knife through crust**—pacing matches **sizzle** or **simmer** audio cues in prompt.
- **Audio:** **`Audio:`** **brush drums**, **upright bass**, **sizzle pan**, **gentle kitchen room**—generic, no recipe ASMR trademarks.
- **Anchors:** *food cinematic, delicious texture, steam, rich warm tones, close-up appetizing detail, food_cinematic_video.*

## Definition of done

Inline video link; pre-submit honored.

## Boundaries (out of scope)

Frozen JSON; no false health claims.

### Example prompts

- `9:16 ramen steam—food_cinematic_video, macro push through steam, warm amber grade, light jazz trio generic`
- `Overhead pasta twirl—lateral slide, oil sheen, funky bass bed`
- `Steak crust macro—orbit plate, sizzle foley layer (image URL)`

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
| ⭐ Best | `KLING_V3_0_PRO` | 10 (**15** for slow cook beats) | **Omit `resolution`.** Optional neg: `flat cafeteria lighting, cold blue cast, watermark` |
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

## Scenario: Text / image food cinematic

Expand appetite beats → submit.

## Loop seam (optional)

`seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `food-cinematic-video-gen`.
