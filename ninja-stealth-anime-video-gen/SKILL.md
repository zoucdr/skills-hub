---
name: ninja-stealth-anime-video-gen
version: 1.0.0
description: "Generate ninja anime stealth action—night roofs, smoke bursts, agile parkour, and shadow pursuit for vertical shorts. Use when you need rooftop chases, kunai/shuriken **stylized** flashes, or motion from ninja art; expand briefs before submit. Paid API; **KLING_V2_6_PRO** for **night** contrast **10s**; **SEEDANCE_2_0** for **roof** chases with **audio**; **PIKA 2.2** for **stylized** flash (**no** `aspect_ratio`)."

tags: [anime, ninja, stealth, action, night, vertical, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Ninja stealth anime vertical video

**Ninja stealth anime** in one line: **ink-black blues**, **smoke pops**, **tile rhythm**, **disappear** into shadow.

**Style anchor:** `ninja anime, stealth action, rooftop chase, smoke, agile movement, shadow pursuit`.

## Use when

- **Roof hop**, **grapple line** swing, **smoke bomb** exit, **forest** moon chase.
- Text or **single image-to-video**.

**Not this skill:** **open-field shonen** punch-up as default composition.

**Models:** tiered—**Kling 2.6** (default best), **Seedance 2.0** (good), **Pika 2.2** (fast draft). See frozen table for JSON field rules.

## Runtime docs vs this skill

**[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)** is the **single source of truth** for CLI shapes, hosts, uploads, polling, `wait`, and errors. This skill adds **genre**, **prompt expansion**, and **frozen model rows** only.

## Prerequisites

- `WERYAI_API_KEY` **must be set** before running `video_gen.js`.
- Node.js **18+**; prefer public **`https`** image URLs. If the bundled runtime supports local file paths, review `scripts/video_gen.js`, verify read-and-upload behavior, and obtain **explicit consent** before use (details in the API file).
- The bundled `video_gen.js` **requires** a non-empty `model` in every `wait` / `submit-*` JSON; **no default model** (`MISSING_PARAM` if blank).
- Each successful `submit-*` / `wait` may consume credits.

## Security, secrets, and trust

- **`WERYAI_API_KEY`**: secret; never commit its value. OpenClaw metadata lists `requires.env` / `primaryEnv`.
- **Hosts, upload endpoints, polling, stdout/errors:** **[references/WERYAI_VIDEO_API.md](references/WERYAI_VIDEO_API.md)**—do not duplicate here.
- **Local images:** prefer `https`; local path upload behavior requires review + **explicit consent** (API file).
- **Higher assurance:** isolated environment; review `scripts/video_gen.js` before production.

## Pre-submit gate (mandatory)

⚠️ **No paid submit without explicit user confirmation.** Do **not** call `submit-text` / `submit-image` / `submit-multi-image` or `wait` until the user has **explicitly approved** the parameter table (including the **full expanded `prompt`**). **Never** infer consent from silence. **Explicit** means **confirm** / **go** / **approved** / **yes, generate** (or equivalent).

**Parameter confirmation table (before submit):** `model`, `duration`, `aspect_ratio` (if used), `resolution` (when the frozen row lists it), `generate_audio` (if used), `negative_prompt` (when the frozen row allows it), **full expanded `prompt` (entire text)**.

## Workflow

1. Confirm the request matches **this** genre skill (text and/or single-image as below).
2. Collect a **brief** (may be one line), optional **`https`** image URL, tier or explicit `model_key`.
3. **Expand prompt (mandatory):** unless the user supplied a final long prompt and asked not to rewrite, expand using **`## Prompt expansion (mandatory)`** (camera, light, motion, genre beats, **`Audio:`** when audio is on). **Do not** submit only the raw few words.
4. Validate length vs `prompt_length_limit` (2000 for listed models); trim lower-priority clauses if over.
5. **Pre-submit gate:** show the confirmation table with the **full** expanded `prompt`. **Stop** until explicit approval.
6. **Submit (async default):** after approval, run `submit-text` or `submit-image` with JSON allowed by the frozen table. **Do not** start a long blocking `wait` in the same turn unless the user already asked to block until done.
7. **Immediate notify:** on success with task id, tell the user the id(s), short queue note, and **ask** whether to poll `status` or use blocking `wait` next.
8. **Continue (user-driven):** only after they agree, poll `status` or run `wait --json` with the **same** object.
9. Return playable URLs or explain errors. Present URLs as **`[Video](https://…)`** inline links—**not** inside code fences.

## CLI reference

**Authoritative CLI / JSON / `wait` / image routes:** **[references/WERYAI_VIDEO_API.md](references/WERYAI_VIDEO_API.md)** only.

**Skeleton (placeholders—do not mirror API Examples):**

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js submit-image --json '{"model":"…","prompt":"…","duration":…,"image":"…"}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

`submit-multi-image`, blocking `wait --json '…'`, and field rules: **see the API file**. Build JSON using **only** keys supported for the chosen row in **`## Model and API constraints`**.

**Filter:** **Kling 2.6** (image): **omit** `aspect_ratio` and `resolution` unless API updates. **Seedance 2.0**: set `resolution` (`720p` default); **no** `negative_prompt`. **Pika 2.2**: **do not** send `aspect_ratio` (text or image per frozen row).

## Prompt expansion (mandatory)

- **When:** brief, vague, or missing cinematic detail—or **no** usable prompt (topic only). **Always** expand before submit unless the user provides a finished long prompt and opts out of rewriting.
- **Look / materials / VFX (genre):**
  - **Palette:** moonlit cool, occasional warm lantern spill.
  - **FX:** smoke sheets, afterimage **subtle**, shuriken streak **anime**.
  - **Readability:** silhouette-first; mask/eyes catch light.
- **Camera / motion / 9:16 framing (genre):**
  - **Moves:** rooftop tracking, quick pan following leap, sudden tilt down to alley.
  - **Framing:** **9:16**—vertical climb paths; avoid losing subject in pure black.
- **Audio:** for models with **`generate_audio` default `true`**, add a labeled **`Audio:`** block: ambience + rhythm + SFX layers matched to on-screen beats—**even if the user said nothing about sound**. Use **generic** instrumental/SFX wording (**no** copyrighted tracks). If the user wants **silent** output, set `generate_audio`: `false` and **omit** audio lines; state that in the confirmation table.
- **Style re-anchor:** weave the **Style anchor** English keywords into the final prompt naturally.
- **Negative prompt:** Optional **Kling** `negative_prompt`: `daylight picnic, bright pastel, watermark`.
- **Length:** stay within **2000** chars for these models; drop adjectives before core action if tight.
- **Confirmation:** show the **full** expanded `prompt` in the pre-submit table.

**Examples elsewhere** show target richness only—**derive** from the user's actual brief.

## Definition of done

Done when the user receives a playable **`[Video](url)`** link (Markdown inline) or a clear failure path. Pre-submit: **explicit** confirmation with **full** prompt. Post-submit: user got task id **immediately** and **chose** `status` vs `wait`. Parameters match the frozen row. `generate_audio` defaults **`true`** when supported unless the user requested silent.

## Boundaries (out of scope)

- **Do not** invent API fields beyond the frozen table + API file.
- **Do not** embed secrets. **Do not** use local image paths without runtime review + **explicit consent**.
- **Do not** wrap user-facing playable URLs in code fences.
- **Do not** recreate copyrighted shows shot-for-shot; **inspired-by** must stay **generic** and **original**.
- **Ethics:** avoid sexualized minors, graphic gore, or hateful stereotypes; escalate only if the user provides an **adult** and **policy-compliant** brief.

### Example prompts

- `9:16` rooftop chase—moon, smoke puff, fast pan, tense drums (generic)
- Forest sprint—branch shadows, shuriken streak stylized
- Image-to-video: ninja key art → scarf flutter + smoke

---

## Model and API constraints (frozen for this skill)

> Registry snapshot via `weryai-model-capabilities-collect.js` / `video_gen.js models` at authoring (**2025-03-23**). Re-run after platform upgrades.

### Text-to-video

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | prompt limit |
|-----------|-----------|---------------|-------------|-------|-----------------|--------------|
| `KLING_V2_6_PRO` | 5, 10 | 9:16, 1:1, 16:9 | *(omit)* | Yes | Yes | 2000 |
| `SEEDANCE_2_0` | 5–15 (integer seconds) | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No | 2000 |
| `PIKA_2_2` | 5, 10 | *(do not send `aspect_ratio`)* | 720p, 1080p | No | Yes | 2000 |

### Image-to-video (single `image`)

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | Notes |
|-----------|-----------|---------------|-------------|-------|-----------------|--------|
| `KLING_V2_6_PRO` | 5, 10 | *(omit)* | *(omit)* | Yes | Yes | single `image`; **omit** `aspect_ratio` / `resolution` |
| `SEEDANCE_2_0` | 5–15 (integer seconds) | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No | `upload_image_limit` **3** |
| `PIKA_2_2` | 5, 10 | *(omit)* | 720p, 1080p | No | Yes | `upload_image_limit` **3**; **omit** `aspect_ratio` |

---

## Recommended models

| Tier | model_key | Default duration | Notes |
|------|-----------|------------------|--------|
| ⭐ Best (default) | `KLING_V2_6_PRO` | 10 | **Kling 2.6**—punchy **blue** moon reads (**5/10s**). |
| 👍 Good | `SEEDANCE_2_0` | 10 | **Seedance 2.0**—longer **chase** lanes + **SFX**. |
| ⚡ Fast | `PIKA_2_2` | 10 | **Pika 2.2**—graphic **smoke** pops; **silent**; omit **aspect_ratio**. |

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V2_6_PRO` (unless user picks another **listed** tier) |
| `aspect_ratio` | **`9:16`** when the chosen row lists it; **omit** when the row says omit |
| `duration` | **10** for default best tier (adjust to any **allowed** value in that row) |
| `resolution` | *(omit)* for **default best**; **Seedance / Vidu / Veo / Sora / Wan / Doubao / Dreamina / Grok / Pika / WERYAI** per row—**Kling 3.x** **omit** |
| `generate_audio` | **`true`** for tiers with **Audio = Yes** in the frozen table; **`false`** (or omit per API file) for **Sora / Wan / Dreamina / Grok / Pika / WERYAI_VIDEO_1_0 / DOUBAO_1_PRO_FAST / KLING_V2_5_TURBO** rows |
| `negative_prompt` | **Only** if frozen row = **Yes** (genre lines from **Prompt expansion**) |
## Scenario: Text-to-video (genre)

1. Collect brief + tier/model. **Expand** per **`## Prompt expansion (mandatory)`**.
2. **Pre-submit** table with **full** prompt → wait for **explicit** confirmation.
3. `submit-text` → notify task id → user chooses `status` / `wait`.
4. Return **`[Video](url)`** when ready.

## Scenario: Image-to-video (single `https` image)

1. Collect **`https`** URL + motion brief. **Expand** prompt (motion, light, **Audio:**…).
2. **Pre-submit** → **explicit** confirm.
3. `submit-image` with same JSON field rules → notify id → user-driven `status` / `wait`.
4. Return **`[Video](url)`**.

## Loop seam (optional)

Add to prompt if user wants seamless loops: `seamless loop`, `perfect loop`, `ends where it begins`.

> Skill: `ninja-stealth-anime-video-gen`.
