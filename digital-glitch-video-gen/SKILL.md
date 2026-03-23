---
name: digital-glitch-video-gen
version: 1.0.0
description: "Generate digital glitch–style videos with WeryAI—signal damage, RGB split, strobing flashes, tearing, broken-media texture for cyber-anomaly and experimental energy. Use when you need erratic motion, sudden push, jitter, jump-cut rhythm; glitch hop or noise beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [glitch, RGB, digital, distortion, experimental, noise, cyber, broken-media, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Digital glitch & broken-signal video

**Digital glitch video** in one line: **corrupted signal**, **RGB channel split**, **frame tear**, **strobe flashes**, and **compression-artifact mood**—**digital interference**, not clean commercial polish. Camera favors **irregular drift**, **sudden punch-in**, **micro jitter**, and **jump-cut** rhythm. Audio leans **glitch hop** stutters, **experimental electronic** bursts, **noise texture** beds (all **generic**—no named tracks).

**Style anchor tag (optional in user briefs):** `digital_glitch_video_style`.

## Use when

- Someone asks for **glitch**, **RGB split**, **signal noise**, **digital interference**, **broken media**, **experimental** aggressive visuals.
- **Text-to-video** or **image-to-video** (single `image`) with **9:16**, **16:9**, or **1:1**.
- Users mention **datamosh mood** (generic), **VHS-digital hybrid**, **corrupted file** aesthetic.

**Not this skill:** **clean product ads**, **soft romantic** default, or **naturalistic drama**—unless user overrides.

**Model choice:** **⭐** **`KLING_V3_0_PRO`** · **👍** **`KLING_V3_0_STA`** · **⚡** **`SEEDANCE_2_0`** + **`720p`**, no **`negative_prompt`**. Kling: **omit `resolution`**.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)** for all CLI/json/polling. Run **`scripts/video_gen.js`** only after **Pre-submit**.

## Prerequisites

`WERYAI_API_KEY`; Node 18+; **`https`** images preferred; local upload per API + consent; non-empty **`model`**.

## Security, secrets, and trust

Secret key; image rules in **API file**; review script for local paths.

## Pre-submit gate (mandatory)

⚠️ Explicit approval of **full expanded `prompt`** + parameters before **`submit-*`** / **`wait`**.

## Workflow

Collect brief → tier → aspect (**`9:16`** default) → **expand prompt** → validate frozen table → **pre-submit** → **`submit-*`** → notify id → user **`status`** / **`wait`** → **`[Video](url)`** links not in fences.

## CLI reference (minimal; details in API file)

**Single source of truth:** **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**`submit-image`**, **`wait`**: API file. **Filter:** Kling no **`resolution`**; Seedance no **`negative_prompt`**.

## Prompt expansion (mandatory)

- **Look:** **RGB separation**, **scan-line tear**, **block compression** cues, **random color flashes**, **horizontal smear**, **brief frame skip** feel—keep **readable subject** unless user wants pure abstraction.
- **Motion:** **irregular path**, **sudden zoom**, **shake bursts**, **jump-cut** on beat.
- **Audio:** **`Audio:`** **glitch-hop** stutter drums, **bit-crush** texture, **noise swells**—generic. Silent: `generate_audio: false`.
- **Anchors:** *glitch distortion, RGB split, signal noise, digital interference, broken media texture, digital_glitch_video_style.*

## Definition of done

Confirmed pre-submit; task id flow; valid JSON; **`[Video](https://…)`**.

## Boundaries (out of scope)

Kling/Seedance field rules; no second API manual in SKILL; no secrets in repo.

### Example prompts

- `9:16 portrait glitch storm—RGB split, tearing flashes, jitter push, glitch hop stutter bed`
- `Abstract signal break—digital_glitch_video_style, noise texture, jump-cut rhythm`
- `City plate with corruption overlay—horizontal smear, experimental electronic pulses`
- `Animate still—sudden punch-in through glitch bands (image URL in JSON)`

---

## Model and API constraints (frozen for this skill)

> 2026-03-23 snapshot. Re-run collect after upgrades.

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
| ⭐ Best | `KLING_V3_0_PRO` | 10 | **Omit `resolution`.** Optional **`negative_prompt`**: `clean commercial grade, soft romantic haze, naturalistic drama, watermark` |
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

## Scenario: Text-to-video glitch

Expand → pre-submit → **`submit-text`** → notify.

## Scenario: Image-to-video glitch

**`https`** image → expand motion + corruption → **`submit-image`**.

## Loop seam (optional)

`seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `digital-glitch-video-gen`.
