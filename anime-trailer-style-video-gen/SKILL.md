---
name: anime-trailer-style-video-gen
version: 1.0.0
description: "High-density anime trailer teasers—world flashes, title card negative space, cliffhanger punctuation for vertical hype. Use when you need teaser montage energy, epic sting beats, or motion from promo key art; expand briefs before submit. Paid API; **VEO_3_1** **8s** **teaser** stings; **SORA_2_PRO** **silent** **scale** flashes; **WERYAI_VIDEO_1_0** budget **5/10s** **sizzle**."

tags: [anime, trailer, teaser, epic, montage, vertical, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Anime trailer style vertical video

**Anime trailer style** in one line: **rapid world glimpses**, **impact text negative space** (no real logos), **braam** stings **generic**.

**Style anchor:** `anime trailer, epic teaser, dramatic montage, worldbuilding flashes, suspenseful climax`.

## Use when

- **Logo-safe title card** with **original** typography description, **cliffhanger** cut implied.
- Text or **single image-to-video**.

**Not this skill:** **slow slice-of-life** single beat default.

**Models:** tiered—**Veo 3.1** (default best), **Sora 2 Pro** (good), **WeryAI Video 1.0** (fast draft). See frozen table for JSON field rules.

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

**Filter:** **Veo 3.x** (text): `duration` **must be 8**; set `resolution` as allowed. **Image fast tier** uses **`CHATBOT_VEO_3_1_FAST`**, not `VEO_3_1_FAST`. **Sora 2 / 2 Pro**: **no** generated audio—use `generate_audio` **false** or omit; choose `duration` from **4 / 8 / 12**.

## Prompt expansion (mandatory)

- **When:** brief, vague, or missing cinematic detail—or **no** usable prompt (topic only). **Always** expand before submit unless the user provides a finished long prompt and opts out of rewriting.
- **Look / materials / VFX (genre):**
  - **Montage:** describe **flashes**: city, character close, monster eye **generic**, symbol—within **one** continuous camera whip if needed.
  - **Contrast:** push saturation on stings, brief desat breathers.
  - **Legal:** **no** real studio/network marks; **original** title words only if user supplies.
- **Camera / motion / 9:16 framing (genre):**
  - **Moves:** slam zooms, whip transitions **implied**, handheld epic.
  - **Framing:** **9:16**—save top for **text-safe** negative space **optional**.
- **Audio:** for models with **`generate_audio` default `true`**, add a labeled **`Audio:`** block: ambience + rhythm + SFX layers matched to on-screen beats—**even if the user said nothing about sound**. Use **generic** instrumental/SFX wording (**no** copyrighted tracks). If the user wants **silent** output, set `generate_audio`: `false` and **omit** audio lines; state that in the confirmation table.
- **Style re-anchor:** weave the **Style anchor** English keywords into the final prompt naturally.
- **Negative prompt:** Optional **Kling** `negative_prompt`: `boring static frame, soft ASMR, watermark`.
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

- `9:16` anime teaser—whip montage feel, braam sting, epic drum build (generic)
- Cliffhanger eye close—pull to black **implied**
- Image-to-video: key art trio → light streak + camera slam

---

## Model and API constraints (frozen for this skill)

> Registry snapshot via `weryai-model-capabilities-collect.js` / `video_gen.js models` at authoring (**2025-03-23**). Re-run after platform upgrades.

### Text-to-video

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | prompt limit |
|-----------|-----------|---------------|-------------|-------|-----------------|--------------|
| `VEO_3_1` | **8 only** | 9:16, 16:9 | 720p, 1080p, 4k | Yes | No | 2000 |
| `SORA_2_PRO` | 4, 8, 12 | 9:16, 16:9 | 720p, 1080p | No | No | 2000 |
| `WERYAI_VIDEO_1_0` | 5, 10 | 9:16, 16:9, 4:3, 3:4, 1:1, 21:9 | 480p, 720p | No | No | 2000 |

### Image-to-video (single `image`)

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | Notes |
|-----------|-----------|---------------|-------------|-------|-----------------|--------|
| `VEO_3_1` | **8 only** | 9:16, 16:9 | 720p, 1080p, 4k | Yes | No | `upload_image_limit` **3** |
| `SORA_2_PRO` | 4, 8, 12 | 9:16, 16:9 | 720p, 1080p | No | No | single `image` |
| `WERYAI_VIDEO_1_0` | 5, 10 | 9:16, 16:9, 4:3, 3:4, 1:1, 21:9 | 480p, 720p | No | Yes | single `image` |

---

## Recommended models

| Tier | model_key | Default duration | Notes |
|------|-----------|------------------|--------|
| ⭐ Best (default) | `VEO_3_1` | 8 | **Veo 3.1**—**braam**-friendly **audio** in **8s**. |
| 👍 Good | `SORA_2_PRO` | 8 | **Sora 2 Pro**—**realistic** **destruction** glimpses; **no audio**. |
| ⚡ Fast | `WERYAI_VIDEO_1_0` | 10 | **WeryAI Video 1.0**—quick **vertical** **drafts**; **no audio**. |

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `VEO_3_1` (unless user picks another **listed** tier) |
| `aspect_ratio` | **`9:16`** when the chosen row lists it; **omit** when the row says omit |
| `duration` | **8** for default best tier (adjust to any **allowed** value in that row) |
| `resolution` | `1080p` (or `720p` / `4k` per row) for **default best**; **Seedance / Vidu / Veo / Sora / Wan / Doubao / Dreamina / Grok / Pika / WERYAI** per row—**Kling 3.x** **omit** |
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

> Skill: `anime-trailer-style-video-gen`.
