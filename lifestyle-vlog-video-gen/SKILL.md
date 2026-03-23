---
name: lifestyle-vlog-video-gen
version: 1.0.0
description: "Generate lifestyle vlog videos with WeryAI—casual daily moments, soft natural light, friendly handheld companion framing, relaxed but rhythmic cuts. Use when you need selfie handheld, follow cam, tabletop overhead, whip transitions; lo-fi, bedroom pop, or light pop beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [lifestyle, vlog, daily, casual, handheld, natural-light, companion, lo-fi, bedroom-pop, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Lifestyle vlog & casual daily video

**Lifestyle vlog video** in one line: **authentic-everyday** warmth, **soft window light**, **handheld intimacy**, **fragmented but paced** cuts—**companion** feeling, not prestige campaign. Camera: **selfie-arm drift**, **follow behind**, **tabletop overhead** of coffee / desk, **playful whip** to next beat. Audio: **lo-fi** crackle-adjacent bed, **bedroom pop**, **upbeat light pop** (generic).

**Style anchor:** `lifestyle_vlog_editor` (legacy tag from briefs; same as **lifestyle vlog** look).

## Use when

- **Lifestyle vlog**, **casual daily moments**, **soft natural light**, **friendly handheld perspective**, **relaxed authentic** social content.
- Text or **single image-to-video**.

**Not this skill:** **cinematic epic travel** default (use travel-epic) or **high-gloss beauty macro** primary (use beauty-glossy)—unless user wants hybrid.

**Models:** Kling Pro/Sta / Seedance. Kling **omit `resolution`**.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

## Prerequisites

`WERYAI_API_KEY`; Node 18+; **`https`** images; **`model`** required.

## Security, secrets, and trust

API file; consent for local uploads.

## Pre-submit gate (mandatory)

⚠️ **Full expanded `prompt`** + params explicitly approved.

## Workflow

Brief → tier → **`9:16`** default vertical vlog → expand → pre-submit → **`submit-*`** → notify → **`status`**/**`wait`** → **`[Video](https://…)`**.

## CLI reference (minimal; details in API file)

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**Filter:** Kling no **`resolution`**; Seedance no **`negative_prompt`**.

## Prompt expansion (mandatory)

- **Look:** **natural window** key, **gentle warm** or **clean daylight** per brief; **organic clutter** acceptable; avoid **over-lit commercial** unless “clean vlog” requested.
- **Motion:** **breathing handheld**; **quick reframe** to new activity; **match cut** energy optional.
- **Audio:** **`Audio:`** **lo-fi** drums, **bedroom pop** guitar pluck, **room tone**, **keyboard clack** / **coffee pour** foley—generic.
- **Anchors:** *lifestyle vlog, casual daily moments, soft natural light, friendly handheld perspective, lifestyle_vlog_editor.*

## Definition of done

Inline video link; pre-submit honored.

## Boundaries (out of scope)

Frozen JSON; respect privacy if user supplies identifiable spaces.

### Example prompts

- `9:16 morning desk—lifestyle vlog, soft natural light, handheld selfie drift, lo-fi bed generic`
- `Coffee pour overhead—tabletop whip to window, bedroom pop mood`
- `Walk follow through market—friendly handheld perspective, light pop (image URL)`

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
| ⭐ Best | `KLING_V3_0_PRO` | 10 | **Omit `resolution`.** Optional neg: `cinematic orange-teal blockbuster, heavy grade, watermark` |
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

## Scenario: Text / image lifestyle vlog

Expand daily beat → submit.

## Loop seam (optional)

`seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `lifestyle-vlog-video-gen`.
