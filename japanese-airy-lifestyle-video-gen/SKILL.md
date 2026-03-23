---
name: japanese-airy-lifestyle-video-gen
version: 1.0.0
description: "Generate Japanese airy lifestyle videos with WeryAI—soft bright highlights, white-forward palette, low contrast, delicate color, generous negative space, and calm daily-life mood for still life, routines, and gentle youthful aesthetics. Use when you need static or near-static framing, ultra-slow dolly-in, eye-level follow, slight drift, and a light instrumental bed (soft piano, acoustic guitar); expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [japanese, airy, lifestyle, soft-light, low-contrast, still-life, daily-life, minimal, negative-space, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Japanese airy lifestyle video

**Japanese airy** in one line: **soft bright highlights**, **white-forward exposure**, **low contrast**, **thin delicate color**, **clean quiet daily life**, and **breathing room in the frame**—**not** punchy HDR, **not** neon pop, **not** heavy film contrast. Camera favors **locked or near-static composition**, **ultra-slow dolly-in**, **eye-level follow** with **barely perceptible drift** (slider / gentle handheld calm). Audio stays **light and acoustic**: **soft felt piano**, **fingerpicked wooden acoustic guitar**, **airy pads**, **very slow tempo** (all **generic**—no named songs, artists, or recognizable hooks), plus subtle **room hush** and delicate **foley** (pages, fabric, distant city hum).

**Style anchor tag (optional in user briefs):** `japanese_airy_lifestyle_video`.

## Use when

- Someone asks for **Japanese airy**, **空气感**, **soft white lifestyle**, **minimal contrast daily clip**, **calm still life motion**, **quiet room tour**, **gentle girlhood / soft youthful aesthetic** (non-explicit), or **留白**-style framing for **food**, **desk**, **plants**, **linen**, **windows**, and **rainy-day interiors**.
- **Text-to-video** or **image-to-video** (single reference still) with **9:16** (default for vertical social), **16:9** (wider calm interior), or **1:1**.
- Users mention **very slow push-in**, **static composition**, **eye-level follow**, **slight drift**, **soft piano**, **acoustic guitar**, or phrases like *japanese airy, soft white tones, minimal contrast, delicate daily life, calm composition*.

**Not this skill:** **high-contrast blockbuster**, **neon nightlife**, **aggressive commercial punch**, **fast whip-pan vlog energy**—unless the user explicitly overrides and accepts leaving the default airy grammar.

**Model choice (this package):** **⭐ Best** → **`KLING_V3_0_PRO`** (default for controlled slow motion and soft grade). **👍 Good** → **`KLING_V3_0_STA`**. **⚡ Fast / draft** → **`SEEDANCE_2_0`** with **`resolution`** **`720p`** (omit **`negative_prompt`** for this model). Kling rows **must omit `resolution`** from JSON.

## Runtime docs vs this skill

- **When to open [`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md):** whenever you need **authoritative** `video_gen.js` usage—`submit-text` / `submit-image` / `submit-multi-image`, `status`, `wait`, **full JSON shape**, stdout fields, errors, **`https` image rules**, optional local-file upload behavior, polling/backoff, env vars, and fixed API hosts. **Do not** duplicate that material here.
- **When to run `scripts/video_gen.js`:** only **after** the user passes **`## Pre-submit gate (mandatory)`**, from the **skill package root**, using the **same** JSON as documented in the API file.

**Assembly:** Your pipeline supplies **`scripts/video_gen.js`** and **`references/WERYAI_VIDEO_API.md`** next to this `SKILL.md`. **No other Cursor skills required.**

## Prerequisites

- `WERYAI_API_KEY` **must be set** before real runs (see API file for exceptions such as `--dry-run`).
- Node.js **18+**; prefer public **`https`** image URLs. If the bundled runtime supports local file paths, review `scripts/video_gen.js`, verify read-and-upload behavior, and obtain **explicit consent** before using a local path.
- **Model:** `video_gen.js` **requires** a non-empty `model` in every `submit-*` / `wait` JSON—no script default. List the chosen **`model_key`** in the confirmation table.
- Each successful **`submit-*`** / **`wait`** may consume credits; re-submit creates new paid tasks.

## Security, secrets, and trust

- **`WERYAI_API_KEY`**: Treat as a secret; never commit its value. OpenClaw metadata lists **`requires.env`** / **`primaryEnv`** so installers know it is mandatory at runtime.
- **Image input (public `https` vs local upload, consent):** follow **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**—this skill does not restate those rules.
- **Higher assurance:** Run in an isolated environment; review `scripts/video_gen.js` before production use.

## Pre-submit gate (mandatory)

⚠️ **No paid submit without explicit user confirmation.** Do **not** call `submit-text`, `submit-image`, or `wait` until the user has **explicitly approved** the parameter table below, including the **full expanded `prompt`** (entire text, not a summary). **Never** infer consent from silence. **Explicit** means **confirm** / **go** / **approved** / **yes, generate** (or equivalent).

**Parameter confirmation table (show before submit):** `model`, `duration`, `aspect_ratio`, `resolution` (**only** if the chosen model supports it—**never** for Kling V3), `generate_audio`, `negative_prompt` (**only** for models that support it), **full expanded `prompt`**.

## Workflow

1. Confirm the request matches this skill (text-to-video and/or single image-to-video).
2. Collect the user's **brief**, optional **`image`** URL, and **tier**:
   - **Default / best / "final"** → **`KLING_V3_0_PRO`**
   - **Balanced / good** → **`KLING_V3_0_STA`**
   - **Draft / cheap / fast** → **`SEEDANCE_2_0`** (use **`resolution`** **`720p`** or **`480p`**; **do not** send **`negative_prompt`**)
   - User may **name a `model_key`** explicitly if it appears in the frozen table for the active channel—then ignore tier defaults for `model`.
3. Collect **aspect**: default **`9:16`** for vertical lifestyle/social; use **`16:9`** for calm widescreen interior or window light; **`1:1`** when the user requests square feeds.
4. **Expand prompt (mandatory):** Unless the user supplied a finished long prompt (within **`prompt_length_limit`**) and asked **not** to rewrite, expand per **`## Prompt expansion (mandatory)`**. **Do not** submit only the user's minimal words.
5. Validate JSON fields against the frozen tables: **Kling** → **omit `resolution`**; **Seedance** → include **`resolution`** when using that model; **`negative_prompt`** only for Kling (optional but recommended with a short line that rejects harsh contrast and garish saturation).
6. **Pre-submit gate:** Show the confirmation table with the **full expanded `prompt`**; **stop** until explicit approval or edits.
7. **Submit (async, default):** After approval, run **`submit-text`** or **`submit-image`**. **Do not** start a long blocking **`wait`** in the same turn unless the user **already** asked to block until the video is ready.
8. **Immediate notify:** On success with **`taskId`** / **`batchId`** (or documented id fields), **immediately** tell the user: accepted, id(s), short queue note, and **ask** whether to continue with **`status`** polling or **`wait`**. **Do not** run long invisible **`wait`** loops without this choice.
9. **Continue (user-driven):** Only after the user agrees, poll `status --task-id …` **or** run `wait --json '…'` with the **same** payload—per user preference.
10. When presenting playable URLs, use **Markdown inline links only** (e.g. `[Video](https://…)`). **Do not** wrap those links in code fences.

## CLI reference (minimal; details in API file)

**Full command reference:** **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

After user confirmation, run from the skill package root:

```sh
node scripts/video_gen.js submit-text --json '{"model":"KLING_V3_0_PRO","prompt":"<expanded English prompt>","duration":10,"aspect_ratio":"9:16","generate_audio":true}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

Add **`negative_prompt`** only for **Kling** rows in the frozen table. Image-to-video: **`submit-image`** with **`image`** as in the API file. Blocking completion: **`wait --json '…'`** with the **same** object—see the API file.

**This skill’s filter:** never send **`resolution`** to Kling V3; include **`resolution`** for **Seedance 2.0**; never send **`negative_prompt`** to **Seedance 2.0**.

## Prompt expansion (mandatory)

`video_gen.js` does **not** auto-expand. Before **`submit-*`** or **`wait`**, turn short input into a **full English production `prompt`** aligned with **Japanese airy lifestyle** grammar—**quiet brightness and restraint over punch**.

**When:** Brief, vague, missing shot/light/audio detail, or **no usable prompt** (topic only)—still build a full `prompt`.

**What to add (checklist):**

- **Look & grade:** **soft white-forward** exposure, **airy highlights**, **low contrast**, **delicate pastel or near-monochrome color** (sage, milk tea, pale blue-gray, warm white), **clean shadows** (lifted, not crushed), **film-soft clarity**—avoid **HDR crunch**, **neon**, **heavy orange-teal**, **lens-flare heroics**, and **busy stickers** unless the user overrides.
- **Subject fit:** **daily life** and **still life**—tea steam, **linen folds**, **wood grain**, **rain on glass**, **stationery**, **bento**, **flowers**, **bathroom tile steam**, **quiet street corner** in soft daylight—**negative space** as a compositional feature.
- **Composition:** **static tableau** or **minimal reframing**; **centered calm** or **gentle rule-of-thirds** with **room to breathe**; optional **soft window backlight** and **dust-mote** calm.
- **Camera grammar (pick 1–2):** **ultra-slow dolly-in**; **eye-level follow** with **micro drift** only; **locked-off** with **tiny parallax** (plant leaves, curtain breath)—**not** whip pans, **not** snap zooms, **not** chaotic handheld.
- **Audio (mandatory when `generate_audio` is true`):** labeled **`Audio:`** block—**soft felt piano**, **fingerpicked wooden acoustic guitar**, **light Japanese-inspired instrumental mood** (generic bed: sparse notes, wide stereo, very slow BPM), **airy pads**, **low room tone**, subtle **foley** (paper, fabric, kettle hiss); **no** copyrighted music, **no** named artists, **no** recognizable melodies. If the user wants **silent** video: set **`generate_audio`: `false`** and omit audio from the prompt; state that in the confirmation table.

**Style anchors (weave naturally):** *japanese airy, soft white tones, minimal contrast, delicate daily life, calm composition, japanese_airy_lifestyle_video.*

**Length:** Stay within **`prompt_length_limit` 2000** for the selected model in the frozen table; drop lower-priority adjectives before losing airy tone and camera restraint.

**Confirmation:** The pre-submit table **must** include the **full** expanded `prompt`.

**`### Example prompts`** below are **richness targets only**—always derive from the user's actual brief.

## Definition of done

Done when the user gets at least one playable Markdown inline link whose label is **Video** and whose target is **`https://…`**, or a clear failure with next steps. **Pre-submit:** parameters and **full expanded `prompt`** were **explicitly** confirmed. **After submit:** user was notified with task id(s) and **chose** `status` vs blocking **`wait`**. Submitted JSON **must** match the frozen row for the chosen **`model_key`**. **`generate_audio`** defaults to **`true`** when the model supports audio unless the user requested silent output.

## Boundaries (out of scope)

- Do **not** send **`resolution`** on **`KLING_V3_0_PRO`** or **`KLING_V3_0_STA`**—it is unsupported and may cause parameter errors.
- Do **not** send **`negative_prompt`** for **`SEEDANCE_2_0`** (not supported).
- Do **not** rely on paths or unofficial docs outside this package for CLI/API details; use only **`references/WERYAI_VIDEO_API.md`**.
- Do **not** embed the secret value of `WERYAI_API_KEY` in files.
- Do **not** wrap user-facing playable URLs in Markdown code fences.

### Example prompts

- `9:16 japanese airy—white linen and morning window light, ultra-slow dolly-in, minimal contrast, soft piano and nylon guitar`
- `16:9 still life—ceramic cup, pale wood table, generous negative space, eye-level drift only, delicate daily life mood`
- `Quiet desk scene—stationery, gentle rain on glass, locked composition, airy highlights, calm composition`
- `Soft youthful lifestyle—pastel cardigan fold, micro curtain motion, japanese airy grade, fingerpicked acoustic bed (generic)`
- `Animate this still—soft white tones, slight push-in, minimal contrast, light instrumental only (image URL in JSON)`

---

## Model and API constraints (frozen for this skill)

> Derived from `node scripts/weryai-model-capabilities-collect.js --mode …` (snapshot `2026-03-23T10:05:48.559Z`). **Re-run** after platform upgrades and refresh this table if values change.

### Text-to-video

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | prompt limit |
|-----------|-----------|---------------|-------------|-------|-----------------|--------------|
| `KLING_V3_0_PRO` | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit—do not send)* | Yes | Yes | 2000 |
| `KLING_V3_0_STA` | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit—do not send)* | Yes | Yes | 2000 |
| `SEEDANCE_2_0` | 5–15 | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No (do not send) | 2000 |

### Image-to-video (single `image`)

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | Notes |
|-----------|-----------|---------------|-------------|-------|-----------------|--------|
| `KLING_V3_0_PRO` | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | Yes | Yes | single `image`; `upload_image_limit` **1** |
| `KLING_V3_0_STA` | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | Yes | Yes | single `image`; `upload_image_limit` **1** |
| `SEEDANCE_2_0` | 5–15 | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No (do not send) | `upload_image_limit` **3** (this skill documents single-image flow by default) |

---

## Recommended models

| Tier | model_key | Default duration | Notes |
|------|-----------|------------------|-------|
| ⭐ Best (default) | `KLING_V3_0_PRO` | 10 (use **15** for slower airy drift) | **Omit `resolution`.** Optional **`negative_prompt`** e.g. `harsh contrast, crushed blacks, neon saturation, HDR punch, busy graphics, watermark, flashy transitions, loud commercial polish, comic look` |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** Lower cost than Pro |
| ⚡ Fast / draft | `SEEDANCE_2_0` | 10 | Set **`resolution`** **`720p`** (or **`480p`**). **No `negative_prompt`** |

**Tier keywords:** unspecified / quality-first → **Best**; "balanced" / "good" → **Good**; "draft" / "cheap" / "fast" → **Fast** (`SEEDANCE_2_0`).

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` unless tier or user overrides |
| `aspect_ratio` | **`9:16`** (default); **`16:9`** or **`1:1`** when the user targets widescreen interior or square feeds |
| `duration` | `10` (use **`5`** for a single beat; **`15`** on Kling/Seedance for slower push-in) |
| `resolution` | **Only for `SEEDANCE_2_0`:** **`720p`** (or **`480p`**)—**omit for Kling** |
| `generate_audio` | **`true`** unless the user requests silent |
| `negative_prompt` | **Kling only** (optional); keep lines compatible with **airy lifestyle**—omit for Seedance |

---

## Scenario: Text-to-video Japanese airy lifestyle

1. Capture **subject** (still life / room / routine prop) and **light mood** (overcast daylight, soft window, warm tungsten mix)—or accept a minimal brief.
2. **Expand** per **`## Prompt expansion (mandatory)`**; pick **`model_key`** from tier rules.
3. **Pre-submit gate** → explicit user confirmation (full **`prompt`**, correct optional fields per model).
4. **`submit-text`** → notify → user chooses **`status`** vs **`wait`**.

---

## Scenario: Image-to-video Japanese airy lifestyle

**Before use:** Prefer public **`https://`** URLs. Local paths only with verified runtime support and **explicit consent**.

1. Plan motion (**ultra-slow push-in**, **eye-level follow**, **slight drift only**) and respect product likeness if requested.
2. Expand prompt; include `image` and **`model_key`** in the confirmation table.
3. After explicit confirm: **`submit-image`** (or **`wait`** if the user pre-requested blocking).

---

## Loop seam (optional)

If the user asks for a **seamless loop**, append at the **end** of the expanded prompt: `seamless loop`, `perfect loop`, `ends where it begins`—no separate API flag.

> Skill: `japanese-airy-lifestyle-video-gen`.
