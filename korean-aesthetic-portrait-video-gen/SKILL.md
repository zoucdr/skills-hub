---
name: korean-aesthetic-portrait-video-gen
version: 1.0.0
description: "Generate Korean-aesthetic portrait and lifestyle videos with WeryAI—clean skin, moderate soft focus, soft pink or creamy palette, polished romantic mood for beauty, café visits, couples, and creator portraits. Use when you need smooth dolly-in, gentle semi-orbit, walking follow, or subtle jib moves with lyrical pop, light R&B, or gentle electronic-style audio; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [korean-aesthetic, beauty, portrait, romantic, soft-skin, creamy-palette, lifestyle, couples, café, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Korean aesthetic portrait & lifestyle video

**Korean aesthetic video** in one line: **clean, luminous skin read**, **moderate soft focus** (not mushy blur), **soft pink or creamy color grading**, **airy highlights**, **delicate polish**, and **romantic calm**—**not** heavy orange-teal blockbuster contrast, **not** gritty documentary grain unless requested. Camera favors **smooth slow dolly-in**, **gentle semi-orbit** (partial arc around subject), **steady walking follow** (gimbal-smooth), and **subtle jib lift or drop** (small vertical emphasis). Audio leans **lyrical pop mood**, **light R&B groove**, or **gentle electronic** beds (all **generic**—no named tracks or brands), with soft **room tone** and tasteful **foley**.

**Style anchor tag (optional in user briefs):** `korean_aesthetic_portrait_video`.

## Use when

- Someone asks for **Korean aesthetic**, **K-beauty lookbook energy**, **soft romantic portrait**, **creamy palette**, **pink-forward gentle grade**, **polished clean frame**, or **café / store-visit vlog** mood for **beauty**, **fashion portrait**, **couples**, or **creator lifestyle**.
- **Text-to-video** or **image-to-video** (single reference still) with **9:16** (default for vertical social), **16:9** (widescreen portrait or space), or **1:1**.
- Users mention **smooth push-in**, **half orbit**, **follow walk**, **slight crane / jib**, **soft skin tone**, **romantic clean look**, or phrases like *korean aesthetic, creamy palette, polished beauty, soft focus portrait*.

**Not this skill:** **gritty street doc**, **high-contrast action trailer**, **neon club saturation**, or **messy handheld chaos**—unless the user explicitly overrides and accepts leaving the default Korean-aesthetic grammar.

**Model choice (this package):** **⭐ Best** → **`KLING_V3_0_PRO`** (default for skin texture control and gentle motion). **👍 Good** → **`KLING_V3_0_STA`**. **⚡ Fast / draft** → **`SEEDANCE_2_0`** with **`resolution`** **`720p`** (omit **`negative_prompt`** for this model). Kling rows **must omit `resolution`** from JSON.

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
3. Collect **aspect**: default **`9:16`** for vertical portrait / social; use **`16:9`** for widescreen couple scenes or airy interiors; **`1:1`** when the user requests square feeds.
4. **Expand prompt (mandatory):** Unless the user supplied a finished long prompt (within **`prompt_length_limit`**) and asked **not** to rewrite, expand per **`## Prompt expansion (mandatory)`**. **Do not** submit only the user's minimal words.
5. Validate JSON fields against the frozen tables: **Kling** → **omit `resolution`**; **Seedance** → include **`resolution`** when using that model; **`negative_prompt`** only for Kling (optional but recommended with a short line that rejects harsh skin texture and garish saturation).
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

`video_gen.js` does **not** auto-expand. Before **`submit-*`** or **`wait`**, turn short input into a **full English production `prompt`** aligned with **Korean aesthetic / polished romantic portrait** grammar—**soft luminance and restraint over punchy commercial polish**.

**When:** Brief, vague, missing shot/light/audio detail, or **no usable prompt** (topic only)—still build a full `prompt`.

**What to add (checklist):**

- **Look & grade:** **clean skin tone** with **healthy subtle glow**, **moderate soft focus** (gentle bloom, **not** plastic oversmooth), **creamy highlights**, **soft pink or peach undertones** in shadows, **low-to-mid contrast**, **delicate clarity**—avoid **muddy shadows**, **orange skin**, **harsh sharpening halos**, and **neon saturation** unless the user overrides.
- **Subject fit:** portrait **beauty macro**, **café / boutique visit**, **couple soft interaction** (hands, walking together, shared drink), **creator mirror or vanity setup**—keep **frame clean** (minimal clutter, tasteful props).
- **Composition:** **airy negative space**, **eye-level or slightly above** for flattering portrait; **shallow depth feel** described in words (bokeh-leaning background) without contradicting moderate soft focus.
- **Camera grammar (pick 1–2):** **smooth slow dolly-in** toward eyes or product; **gentle semi-orbit** (120–200° arc feel, slow); **steady walking follow** beside or behind subject (gimbal-smooth, no sprint); **subtle jib** (small rise or drop for romantic emphasis)—**not** whip pans, **not** shaky run-and-gun.
- **Audio (mandatory when `generate_audio` is true`):** labeled **`Audio:`** block—**lyrical pop mood**, **light R&B** pocket, or **gentle electronic** (soft pads, airy plucks, muted kick—**generic**, no recognizable songs), **quiet café ambience** or **soft room tone**, subtle **foley** (cup clink, fabric rustle, footsteps on tile); **no** copyrighted music or recognizable branded cues. If the user wants **silent** video: set **`generate_audio`: `false`** and omit audio from the prompt; state that in the confirmation table.

**Style anchors (weave naturally):** *korean aesthetic, soft skin tone, creamy palette, romantic clean look, polished beauty, korean_aesthetic_portrait_video.*

**Length:** Stay within **`prompt_length_limit` 2000** for the selected model in the frozen table; drop lower-priority adjectives before losing skin/camera/audio coherence.

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

- `9:16 Korean aesthetic—clean skin, creamy palette, soft window light, smooth dolly-in on beauty routine, gentle electronic bed`
- `16:9 couple in bright café—semi-orbit, romantic clean look, light R&B mood, walking follow toward the counter`
- `Portrait creator—peachy soft grade, moderate soft focus, subtle jib rise, lyrical pop underscore (generic)`
- `Boutique visit—polished shelves, pastel cream tones, steady follow walk, airy highlights, soft foley`
- `Animate this still—korean aesthetic, gentle semi-orbit, creamy palette, romantic clean look (image URL in JSON)`

---

## Model and API constraints (frozen for this skill)

> Derived from `node scripts/weryai-model-capabilities-collect.js --mode …` (snapshot `2026-03-23T10:06:20.727Z`). **Re-run** after platform upgrades and refresh this table if values change.

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
|------|-----------|------------------|--------|
| ⭐ Best (default) | `KLING_V3_0_PRO` | 10 (use **15** for slower orbit / follow) | **Omit `resolution`.** Optional **`negative_prompt`** e.g. `harsh contrast, orange skin, muddy shadows, oversharpened skin, plastic waxy face, neon saturation, busy clutter, watermark, gritty documentary grain, loud commercial polish` |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** Lower cost than Pro |
| ⚡ Fast / draft | `SEEDANCE_2_0` | 10 | Set **`resolution`** **`720p`** (or **`480p`**). **No `negative_prompt`** |

**Tier keywords:** unspecified / quality-first → **Best**; "balanced" / "good" → **Good**; "draft" / "cheap" / "fast" → **Fast** (`SEEDANCE_2_0`).

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` unless tier or user overrides |
| `aspect_ratio` | **`9:16`** (default); **`16:9`** or **`1:1`** when the user targets widescreen or square feeds |
| `duration` | `10` (use **`5`** for a single beat; **`15`** on Kling/Seedance for slower dolly / orbit / follow) |
| `resolution` | **Only for `SEEDANCE_2_0`:** **`720p`** (or **`480p`**)—**omit for Kling** |
| `generate_audio` | **`true`** unless the user requests silent |
| `negative_prompt` | **Kling only** (optional); keep lines compatible with **Korean aesthetic**—omit for Seedance |

---

## Scenario: Text-to-video Korean aesthetic

1. Capture **use case** (beauty / portrait / café visit / couple / creator), **hero subject**, and **palette lean** (pink vs cream)—or accept a minimal brief.
2. **Expand** per **`## Prompt expansion (mandatory)`**; pick **`model_key`** from tier rules.
3. **Pre-submit gate** → explicit user confirmation (full **`prompt`**, correct optional fields per model).
4. **`submit-text`** → notify → user chooses **`status`** vs **`wait`**.

---

## Scenario: Image-to-video Korean aesthetic

**Before use:** Prefer public **`https://`** URLs. Local paths only with verified runtime support and **explicit consent**.

1. Plan motion (**smooth dolly-in**, **semi-orbit**, **walking follow**, **subtle jib**) and respect likeness if the user requests identity preservation.
2. Expand prompt; include `image` and **`model_key`** in the confirmation table.
3. After explicit confirm: **`submit-image`** (or **`wait`** if the user pre-requested blocking).

---

## Loop seam (optional)

If the user asks for a **seamless loop**, append at the **end** of the expanded prompt: `seamless loop`, `perfect loop`, `ends where it begins`—no separate API flag.

> Skill: `korean-aesthetic-portrait-video-gen`.
