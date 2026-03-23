---
name: travel-epic-video-gen
version: 1.0.0
description: "Generate travel epic videos with WeryAI—grand landscape, wide atmosphere, human-in-environment scale, freedom and destination awe. Use when you need aerial pull-back, forward follow, wide lateral, transition push; world music, light epic pads, or indie folk beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [travel, landscape, epic, destination, aerial, freedom, scenic, adventure, wide, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Travel epic & grand destination video

**Travel epic video** in one line: **vast scenery**, **readable scale**, **figure-in-land**, **open-sky freedom**—**immersive destination**, not studio lock-off. Camera: **aerial pull-back** (describe as drone-like motion without claiming real drone ops), **forward follow** on path, **wide lateral** along ridgeline, **push through** natural portal (arch, trees). Audio: **world-music-adjacent** pads, **light epic** swells, **indie folk** guitar (generic).

**Style anchor:** `travel_epic_video`.

## Use when

- **Travel epic**, **grand landscape**, **freedom**, **immersive destination**, **wide scenic atmosphere**, tourism / adventure promos.
- Text or **single image-to-video**.

**Not this skill:** **claustrophobic interior** default or **urban grit street** (use urban-street skill)—unless hybrid.

**Models:** Kling Pro/Sta / Seedance. Kling **omit `resolution`**.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

## Prerequisites

`WERYAI_API_KEY`; Node 18+; **`https`** images; **`model`** required.

## Security, secrets, and trust

API file; consent for local paths.

## Pre-submit gate (mandatory)

⚠️ **Full prompt** + params explicitly approved.

## Workflow

Brief → tier → **`16:9`** often fits landscape; user may want **`9:16`** vertical travel → expand → pre-submit → **`submit-*`** → notify → **`status`**/**`wait`** → **`[Video](url)`**.

## CLI reference (minimal; details in API file)

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**Filter:** Kling no **`resolution`**; Seedance no **`negative_prompt`**.

## Prompt expansion (mandatory)

- **Look:** **atmospheric perspective**, **golden or blue hour** bias per brief, **polarizer-like sky** richness (describe effect, not gear).
- **Scale:** **tiny hiker / huge peak**, **road ribbon**, **coastline curve**.
- **Camera (1–2):** **slow aerial pull** revealing vista; **Steadicam follow** on trail; **wide truck** parallel to cliff.
- **Audio:** **`Audio:`** **light epic** strings swell, **hand drum** loop, **wind** and **distant surf** foley—generic.
- **Anchors:** *travel epic, grand landscape, freedom, immersive destination, wide scenic atmosphere, travel_epic_video.*

## Definition of done

Inline **`[Video](https://…)`**; respectful of protected / sensitive locations if user names them.

## Boundaries (out of scope)

No claiming unsafe drone behavior; frozen JSON rules.

### Example prompts

- `16:9 mountain ridge—travel_epic_video, aerial pull-back reveal, light epic pads, wind foley`
- `Coastal road—wide lateral truck, immersive destination, indie folk guitar generic`
- `Desert figure small in frame—grand landscape, forward follow (image URL)`

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
| ⭐ Best | `KLING_V3_0_PRO` | 10 (**15** for slow reveals) | **Omit `resolution`.** Optional neg: `studio gray cyc, claustrophobic room, watermark` |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** |
| ⚡ Fast | `SEEDANCE_2_0` | 10 | **`720p`**. **No `negative_prompt`** |

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` |
| `aspect_ratio` | **`16:9`** default for landscape epic; **`9:16`** if user wants vertical travel |
| `duration` | `10` |
| `resolution` | **Seedance:** **`720p`** |
| `generate_audio` | **`true`** unless silent |
| `negative_prompt` | **Kling only** |

## Scenario: Text / image travel epic

Expand scale + light → submit.

## Loop seam (optional)

`seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `travel-epic-video-gen`.
