---
name: futuristic-tech-showcase-video-gen
version: 1.0.0
description: "Generate futuristic tech showcase videos with WeryAI—cool blue-white palette, UI-adjacent glow, precision lighting, sleek materials, and minimal lab or keynote energy for AI, hardware, products, and launches. Use when you need precision slider, mechanical push, tilt transitions, turntable orbit; ambient tech beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [futuristic, tech, blue-white, minimal, lab, product, AI, hardware, precision, sci-fi, UI, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Futuristic tech & clean showcase video

**Futuristic tech showcase** in one line: **calm sterile clarity**, **blue–white balance**, **interface-adjacent rim light**, **precision-machined surfaces**, and **negative space**—**modern lab / keynote** mood, not rainy cyberpunk alleys. Camera favors **precision slider**, **mechanical dolly push**, **controlled tilt transitions**, and **turntable orbit** around hero objects. Audio leans **ambient tech pads**, **minimal electronic pulses**, **soft room hush** (all **generic**), plus subtle **foley** (fan hum, key clicks).

**Style anchor tag (optional in user briefs):** `futuristic_tech_showcase`.

## Use when

- Someone asks for **futuristic tech**, **clean blue white palette**, **precision lighting**, **sleek modern surfaces**, **sci-fi UI feel**, **lab**, **hardware hero**, **AI product** vibe.
- **Text-to-video** or **image-to-video** (single reference still) with **9:16**, **16:9**, or **1:1**.
- Users mention **slider move**, **turntable**, **minimal keynote**, **glass and aluminum**, or phrases like *futuristic tech, precision lighting, sci-fi UI feel*.

**Not this skill:** **gritty cyberpunk rain**, **neon-soaked nightlife**, or **warm cozy domestic** as the default—unless the user explicitly overrides.

**Model choice (this package):** **⭐ Best** → **`KLING_V3_0_PRO`**. **👍 Good** → **`KLING_V3_0_STA`**. **⚡ Fast / draft** → **`SEEDANCE_2_0`** with **`resolution`** **`720p`** (omit **`negative_prompt`**). Kling rows **must omit `resolution`**.

## Runtime docs vs this skill

- **When to open [`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md):** authoritative `video_gen.js` usage—`submit-text` / `submit-image` / `submit-multi-image`, `status`, `wait`, **full JSON shape**, stdout, **`https` images**, local upload behavior, polling, env. **Do not** duplicate here.
- **When to run `scripts/video_gen.js`:** only **after** **`## Pre-submit gate (mandatory)`**, from **skill package root**.

**Assembly:** **`scripts/video_gen.js`** + **`references/WERYAI_VIDEO_API.md`** supplied by your pipeline.

## Prerequisites

- `WERYAI_API_KEY` **must be set** (see API file for `--dry-run`).
- Node.js **18+**; prefer **`https`** images; local paths only with script review and **explicit consent**.
- **Non-empty `model`** required every `submit-*` / `wait`.
- Paid tasks may consume credits.

## Security, secrets, and trust

- **`WERYAI_API_KEY`**: secret; never commit. See **`requires.env`** in metadata.
- **Images:** follow **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.
- **Higher assurance:** isolated environment; review `scripts/video_gen.js`.

## Pre-submit gate (mandatory)

⚠️ **No paid submit without explicit user confirmation**—including **full expanded `prompt`**. **Explicit** = confirm / go / approved / yes generate (or equivalent).

**Confirmation table:** `model`, `duration`, `aspect_ratio`, `resolution` (if supported—never Kling V3), `generate_audio`, `negative_prompt` (if supported), **full `prompt`**.

## Workflow

1. Match skill (text and/or single image-to-video).
2. **Tier:** Best → **`KLING_V3_0_PRO`**; Good → **`KLING_V3_0_STA`**; Fast → **`SEEDANCE_2_0`** + **`resolution`** **`720p`/`480p`**; no **`negative_prompt`** on Seedance.
3. **Aspect:** default **`9:16`**; **`16:9`** for keynote/widescreen; **`1:1`** if requested.
4. **Expand prompt (mandatory)** per **`## Prompt expansion (mandatory)`**.
5. Validate against frozen tables (Kling: no `resolution`; Seedance: no `negative_prompt`).
6. **Pre-submit gate** → wait for approval.
7. **`submit-text`** / **`submit-image`** (async default); no long **`wait`** same turn unless user asked.
8. **Notify** task id; user chooses **`status`** vs **`wait`**.
9. **User-driven** polling or blocking **`wait`**.
10. Playable URLs: **`[Video](https://…)`** inline only—no code fences.

## CLI reference (minimal; details in API file)

**Single source of truth:** **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

**`submit-image`**, **`submit-multi-image`**, **`wait`**—see API file. **Filter:** no **`resolution`** on Kling V3; **`resolution`** on Seedance; no **`negative_prompt`** on Seedance.

## Prompt expansion (mandatory)

**When:** Short or empty brief—still produce full English `prompt`.

**Checklist:**

- **Look:** **cool blue-white** key, **soft cyan** accents, **controlled speculars** on glass/metal, **deep but clean** shadows, **subtle UI-like** light bars (generic, no real OS or brand UI).
- **Subject:** **device**, **chip macro**, **robot arm**, **clean bench**, **abstract data sculpture**—readable **tech authority**.
- **Composition:** **generous negative space**, **symmetry** or **golden-ratio** offset, **hero object** isolation.
- **Camera (1–2):** **precision slider** lateral; **slow mechanical push**; **tilt from table to object**; **turntable orbit** at even speed—**not** handheld chaos unless user wants “found footage lab.”
- **Audio (`generate_audio` true):** **`Audio:`**—**ambient tech** drones, **sparse arp**, **minimal clicks**, low **HVAC/fan** bed. **Silent:** `generate_audio: false`, note in table.

**Anchors:** *futuristic tech, clean blue white palette, precision lighting, sleek modern surfaces, sci-fi UI feel, futuristic_tech_showcase.*

**Length:** ≤ **`prompt_length_limit`** (2000 for listed models).

## Definition of done

Playable **`[Video](https://…)`** or clear failure. Pre-submit confirmed; post-submit user chose **`status`**/**`wait`**. JSON matches frozen row.

## Boundaries (out of scope)

- No **`resolution`** on Kling V3; no **`negative_prompt`** on Seedance.
- No API details outside **`references/WERYAI_VIDEO_API.md`**.
- No secret values in files; no code-fenced user-facing video links.

### Example prompts

- `9:16 AI chip hero on graphite slab—futuristic_tech_showcase, precision slider, blue-white rim, ambient tech pad`
- `16:9 clean lab—glass and aluminum, turntable orbit, minimal electronic pulse, sci-fi UI feel lighting only`
- `Hardware wedge product—mechanical push-in, restrained palette, soft fan hum bed (generic)`
- `Abstract data light sculpture—negative space, tilt transition, ambient tech`
- `Animate still—sleek modern surfaces, slow orbit, cool white key (image URL in JSON)`

---

## Model and API constraints (frozen for this skill)

> 2026-03-23 snapshot. **Re-run** `weryai-model-capabilities-collect.js` after platform changes.

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
| ⭐ Best | `KLING_V3_0_PRO` | 10 | **Omit `resolution`.** Optional **`negative_prompt`**: `neon-soaked cyberpunk alley, rain-soaked grime, warm tungsten home, watermark` |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** |
| ⚡ Fast | `SEEDANCE_2_0` | 10 | **`720p`** or **`480p`**. **No `negative_prompt`** |

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` unless overridden |
| `aspect_ratio` | **`9:16`** default; **`16:9`** / **`1:1`** on request |
| `duration` | `10` |
| `resolution` | **Seedance only:** **`720p`** |
| `generate_audio` | **`true`** unless silent requested |
| `negative_prompt` | **Kling only**, optional |

---

## Scenario: Text-to-video futuristic tech

1. Brief: product type, environment, cool vs neutral white bias.
2. Expand; tier; **pre-submit**; **`submit-text`**; notify.

## Scenario: Image-to-video futuristic tech

Prefer **`https`**. Plan **slider/orbit**; **`submit-image`** after confirm.

## Loop seam (optional)

Append: `seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `futuristic-tech-showcase-video-gen`.
