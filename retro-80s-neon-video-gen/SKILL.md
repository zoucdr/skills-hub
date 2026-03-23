---
name: retro-80s-neon-video-gen
version: 1.0.0
description: "Generate 1980s retro neon nightlife videos with WeryAI—synthwave palette, magenta-cyan contrast, exaggerated practicals, wet-street reflections, and fashion-forward retro energy. Use when you need lateral slider moves, fast push-ins, spin-follow tracking, disco or synthwave beds; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [80s, retro, neon, synthwave, magenta, cyan, nightlife, fashion, street, disco, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# 1980s retro neon & nightlife video

**1980s retro neon video** in one line: **synthwave color blocking**, **hot magenta–electric cyan** interplay, **chrome and glass reads**, **wet-pavement reflections**, and **big practical glow**—evoking **vintage nightlife** and **retro fashion** campaigns, not flat daylight realism. Camera favors **lateral slider or truck**, **fast punch-in**, and **spin-follow** or **orbit** around a hero figure. Audio leans **synthwave arps**, **four-on-the-floor disco pulse**, **retro pop groove** (all **generic**—no named hits), plus **club room swell** and light **foley**.

**Style anchor tag (optional in user briefs):** `retro_80s_neon_video`.

## Use when

- Someone asks for **80s retro**, **neon glow**, **synthwave palette**, **magenta cyan** grading, **nostalgic nightlife**, **retro street** fashion, **synth city** backdrops.
- **Text-to-video** or **image-to-video** (single reference still) with **9:16**, **16:9**, or **1:1**.
- Users mention **lateral slide**, **fast dolly in**, **spin follow**, **disco** or **synthwave** mood, or phrases like *80s retro, neon glow, synthwave palette, nostalgic nightlife*.

**Not this skill:** **muted documentary** naturalism, **corporate flat** lighting, or **contemporary minimal** gray UI aesthetic as the default—unless the user explicitly overrides and accepts leaving the default 80s-neon grammar.

**Model choice (this package):** **⭐ Best** → **`KLING_V3_0_PRO`**. **👍 Good** → **`KLING_V3_0_STA`**. **⚡ Fast / draft** → **`SEEDANCE_2_0`** with **`resolution`** **`720p`** (omit **`negative_prompt`** for this model). Kling rows **must omit `resolution`** from JSON.

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
3. Collect **aspect**: default **`9:16`** for vertical fashion / social; use **`16:9`** for widescreen city-night tableau; **`1:1`** when the user requests square retro posts.
4. **Expand prompt (mandatory):** Unless the user supplied a finished long prompt (within **`prompt_length_limit`**) and asked **not** to rewrite, expand per **`## Prompt expansion (mandatory)`**. **Do not** submit only the user's minimal words.
5. Validate JSON fields against the frozen tables: **Kling** → **omit `resolution`**; **Seedance** → include **`resolution`** when using that model; **`negative_prompt`** only for Kling (optional but recommended with a short line that rejects muddy desaturation if the user wants pure neon).
6. **Pre-submit gate:** Show the confirmation table with the **full expanded `prompt`**; **stop** until explicit approval or edits.
7. **Submit (async, default):** After approval, run **`submit-text`** or **`submit-image`**. **Do not** start a long blocking **`wait`** in the same turn unless the user **already** asked to block until the video is ready.
8. **Immediate notify:** On success with **`taskId`** / **`batchId`** (or documented id fields), **immediately** tell the user: accepted, id(s), short queue note, and **ask** whether to continue with **`status`** polling or **`wait`**. **Do not** run long invisible **`wait`** loops without this choice.
9. **Continue (user-driven):** Only after the user agrees, poll `status --task-id …` **or** run `wait --json '…'` with the **same** payload—per user preference.
10. When presenting playable URLs, use **Markdown inline links only** (e.g. `[Video](https://…)`). **Do not** wrap those links in code fences.

## CLI reference (minimal; details in API file)

**Single source of truth for commands, JSON shape, hosts, polling, stdout fields, and image rules:** **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**. **Do not** treat this section as a second copy of that manual.

**Skeleton only (placeholders):** after explicit user confirmation, from the skill package root:

```sh
node scripts/video_gen.js submit-text --json '{"model":"…","prompt":"…","duration":…}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

Fill the JSON object using **only** keys allowed for the chosen **`model_key`** in **`## Model and API constraints (frozen for this skill)`**, plus the rules in the API file (e.g. when `image` / `images` switch routes). **`submit-image`**, **`submit-multi-image`**, and blocking **`wait --json '…'`**—**see the API file**; use the **same** object you would submit.

**This skill’s filter (not duplicated in the API file):** never send **`resolution`** to Kling V3; include **`resolution`** for **Seedance 2.0**; never send **`negative_prompt`** to **Seedance 2.0**.

## Prompt expansion (mandatory)

`video_gen.js` does **not** auto-expand. Before **`submit-*`** or **`wait`**, turn short input into a **full English production `prompt`** aligned with **1980s neon retro** grammar—**color spectacle and nightlife rhythm over flat realism**.

**When:** Brief, vague, missing shot/light/audio detail, or **no usable prompt** (topic only)—still build a full `prompt`.

**What to add (checklist):**

- **Look & grade:** **neon glow**, **synthwave palette**, **magenta and cyan** separation (plus **purple rim** or **hot pink** accents if it stays coherent), **specular streaks** on wet asphalt, **soft bloom** on tubes and signs, **black crush** with **lifted neon cores**. Avoid **flat corporate** gray, **overcast doc** look, and **desaturated luxury** unless the user overrides toward “neon noir.”
- **Subject & story:** **fashion silhouette**, **leather jacket**, **convertible curb**, **arcade façade**, **metro underpass tubes**—props that **read** the decade without needing literal logos.
- **Composition:** **strong leading lines** from road and signage; **layered depth** with foreground bokeh tubes; **hero offset** for editorial attitude.
- **Camera grammar (pick 1–2):** **lateral truck / slider** parallel to neon row; **fast push-in** on face or sign; **spin-follow** or **180-orbit** around subject at walking pace; optional **whip transition** only if user wants hype—default stays **controlled retro glam**.
- **Audio (mandatory when `generate_audio` is true`):** labeled **`Audio:`** block—**synthwave arpeggio**, **punchy retro kick-snare**, **disco four-on-the-floor** or **retro pop groove** (generic), **club room reverb tail**, subtle **rain-on-street** or **distant traffic** foley. **No** copyrighted music or recognizable branded cues. If the user wants **silent** video: set **`generate_audio`: `false`** and omit audio from the prompt; state that in the confirmation table.

**Style anchors (weave naturally):** *80s retro, neon glow, synthwave palette, magenta cyan tones, nostalgic nightlife, retro_80s_neon_video.*

**Length:** Stay within **`prompt_length_limit` 2000** for the selected model in the frozen table; trim secondary color adjectives before losing neon read and motion beat.

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

- `9:16 wet downtown—magenta cyan neon rows, synthwave palette, lateral truck, fast push on leather jacket, arp and four-on-the-floor bed`
- `16:9 retro boulevard—chrome reflections, nostalgic nightlife, spin-follow at walking pace, disco pulse, soft rain foley`
- `Fashion editorial—neon glow, exaggerated practicals, retro_80s_neon_video, orbit around model, retro pop groove (generic)`
- `Arcade alley—synthwave palette, wet ground reflections, snap zoom on sign, club reverb swell`
- `Animate this still—80s retro neon, lateral slide, magenta cyan grade, synthwave underscore (image URL in JSON)`

---

## Model and API constraints (frozen for this skill)

> Derived from `node scripts/weryai-model-capabilities-collect.js --mode …` (2026-03-23 snapshot). **Re-run** after platform upgrades and refresh this table if values change.

### Text-to-video

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | prompt limit |
|-----------|-----------|---------------|-------------|-------|-----------------|--------------|
| `KLING_V3_0_PRO` | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit—do not send)* | Yes | Yes | 2000 |
| `KLING_V3_0_STA` | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit—do not send)* | Yes | Yes | 2000 |
| `SEEDANCE_2_0` | 5–15 | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No (do not send) | 2000 |

### Image-to-video (single `image`)

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | Notes |
|-----------|-----------|---------------|-------------|-------|-----------------|--------|
| `KLING_V3_0_PRO` | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | Yes | Yes | single `image` |
| `KLING_V3_0_STA` | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | Yes | Yes | single `image` |
| `SEEDANCE_2_0` | 5–15 | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No (do not send) | `upload_image_limit` **3** (this skill documents **single-image** flow by default) |

---

## Recommended models

| Tier | model_key | Default duration | Notes |
|------|-----------|------------------|--------|
| ⭐ Best (default) | `KLING_V3_0_PRO` | 10 (use **15** for longer glide / orbit beats) | **Omit `resolution`.** Optional **`negative_prompt`** e.g. `flat gray corporate look, muddy desaturation, overcast documentary, watermark, heavy CGI, modern minimal UI` |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** Lower cost than Pro |
| ⚡ Fast / draft | `SEEDANCE_2_0` | 10 | Set **`resolution`** **`720p`** (or **`480p`**). **No `negative_prompt`** |

**Tier keywords:** unspecified / quality-first → **Best**; "balanced" / "good" → **Good**; "draft" / "cheap" / "fast" → **Fast** (`SEEDANCE_2_0`).

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` unless tier or user overrides |
| `aspect_ratio` | **`9:16`** (default); **`16:9`** or **`1:1`** when the user targets widescreen or square |
| `duration` | `10` (use **`5`** for a punchy neon beat; **`15`** on Kling/Seedance for longer slider passes) |
| `resolution` | **Only for `SEEDANCE_2_0`:** **`720p`** (or **`480p`**)—**omit for Kling** |
| `generate_audio` | **`true`** unless the user requests silent |
| `negative_prompt` | **Kling only** (optional); keep lines compatible with **80s neon retro**—omit for Seedance |

---

## Scenario: Text-to-video 1980s neon retro

1. Capture **setting** (street, club exterior, parking under tubes), **wardrobe cue**, and **color bias** (magenta-dominant vs cyan-dominant) or accept a minimal brief.
2. **Expand** per **`## Prompt expansion (mandatory)`**; pick **`model_key`** from tier rules.
3. **Pre-submit gate** → explicit user confirmation (full **`prompt`**, correct optional fields per model).
4. **`submit-text`** → notify → user chooses **`status`** vs **`wait`**.

---

## Scenario: Image-to-video 1980s neon retro

**Before use:** Prefer public **`https://`** URLs. Local paths only with verified runtime support and **explicit consent**.

1. Plan motion (**lateral slide**, **push-in**, **orbit**) that **amplifies** neon reflections without losing outfit or product read.
2. Expand prompt; include `image` and **`model_key`** in the confirmation table.
3. After explicit confirm: **`submit-image`** (or **`wait`** if the user pre-requested blocking).

---

## Loop seam (optional)

If the user asks for a **seamless loop**, append at the **end** of the expanded prompt: `seamless loop`, `perfect loop`, `ends where it begins`—no separate API flag.

> Skill: `retro-80s-neon-video-gen`.
