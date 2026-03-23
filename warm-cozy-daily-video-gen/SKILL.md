---
name: warm-cozy-daily-video-gen
version: 1.0.0
description: "Generate warm cozy daily-life videos with WeryAI—orange-amber golden light, soft fill, low contrast, gentle bloom, homely comforting mood for cafés, home interiors, pets, routines, and small joys; slow dolly-in, light handheld sway, tabletop slide, follow hands; soft piano, folk, lo-fi, light acoustic guitar in audio when models allow. Use when you need gentle reassuring vertical/social clips from text or a single image; expand short briefs before submit. Default KLING_V3_0_PRO; draft SEEDANCE_2_0."

tags: [cozy, warm, golden-hour, cafe, home, pet, daily, healing, soft-light, low-contrast, handheld, folk, lo-fi, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Warm cozy daily video

**Warm cozy** in one line: **orange–amber golden light**, **soft diffusion**, **low contrast**, **subtle bloom / gentle halation** (not neon glow), and a **homely, reassuring** mood—ideal for **coffee shops**, **home corners**, **pets**, **everyday rituals**, and **small joys**. Camera favors **slow push-in**, **light handheld micro-sway** (calm, not chaotic), **tabletop slide** along surfaces, and **following hand gestures** (pour, stir, pet, arrange). Audio leans **soft**—**light piano**, **folk**-like intimacy, **lo-fi** warmth, **light acoustic guitar** (**generic** wording only; **no** copyrighted track titles or artist names).

**Try asking (short examples):**

- `9:16 café corner—latte steam, golden window light, slow push, soft piano bed`
- `Cat on sunny windowsill—warm amber tones, gentle handheld sway, cozy room tone`
- `Hands arranging flowers on table—tabletop slide, soft bloom, reassuring folk ambience`
- `Animate this home-cooking still—follow hands chopping, golden practicals (image URL)`

## Use when

- Someone asks for **warm cozy**, **golden light**, **soft contrast**, **healing**, **comforting**, **homely**, **café aesthetic**, **slow living**, **pet cozy**, **daily ritual**, or **small happiness** vertical/social clips.
- **Text-to-video** or **image-to-video** (single reference still) with **9:16** (default for short-form cozy content), **16:9**, or **1:1**.
- Users mention **orange–amber palette**, **soft fill**, **low contrast**, **gentle bloom**, **slow dolly**, **tabletop motion**, **hand follow**, **light piano**, **folk**, **lo-fi**, or **acoustic guitar** mood.

**Visual anchors (weave into expanded prompts):** *warm cozy, golden light, soft contrast, homely atmosphere, comforting visuals—plus calm motion grammar (slow push, light handheld, tabletop slide, follow hands).*

**Model choice (this package):** **⭐ Best** → **`KLING_V3_0_PRO`**. **👍 Good** → **`KLING_V3_0_STA`**. **⚡ Fast / draft** → **`SEEDANCE_2_0`** with **`resolution`** **`720p`** (or **`480p`**); **do not** send **`negative_prompt`** to Seedance. **Kling V3 rows must omit `resolution`** from JSON.

## Runtime docs vs this skill

- **When to open [`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md):** whenever you need **authoritative** `video_gen.js` usage—`submit-text` / `submit-image`, `status`, `wait`, **full JSON shape**, stdout fields, errors, **`https` image rules**, optional local-file upload behavior, polling, env vars, and fixed API hosts. **Do not** duplicate that material here.
- **When to run `scripts/video_gen.js`:** only **after** the user passes **`## Pre-submit gate (mandatory)`**, from the **skill package root**, using the **same** JSON as documented in the API file. Re-run **`weryai-model-capabilities-collect.js`** or **`video_gen.js models`** when refreshing platform metadata.

**Assembly:** Your pipeline supplies **`scripts/video_gen.js`** and **`references/WERYAI_VIDEO_API.md`** next to this `SKILL.md`. **No other Cursor skills required.**

## Prerequisites

- `WERYAI_API_KEY` **must be set** before real runs (see API file for exceptions such as `--dry-run`).
- Node.js **18+**.
- **Required JSON baseline** (`model`, `prompt`, `duration`, optional fields): defined in the API file; **which** optional fields are legal for **your** chosen row is in **`## Model and API constraints`** below.
- Paid / credit consumption on successful **`submit-*`** / **`wait`**: see API file.

## Security, secrets, and trust

- **`WERYAI_API_KEY`**: secret; never commit its value. OpenClaw metadata lists **`requires.env`** / **`primaryEnv`**.
- **Image input (public `https` vs local upload, consent):** follow **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**—this skill does not restate those rules.
- **Higher assurance:** isolated environment; review `scripts/video_gen.js` before production use.

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
3. Collect **aspect**: default **`9:16`** for vertical cozy / short-form social; use **`16:9`** for landscape home scenes; **`1:1`** when requested.
4. **Expand prompt (mandatory):** Unless the user supplied a finished long prompt (within **`prompt_length_limit`**) and asked **not** to rewrite, expand per **`## Prompt expansion (mandatory)`**. **Do not** submit only the user's minimal words.
5. Validate JSON fields against the frozen tables: **Kling** → **omit `resolution`**; **Seedance** → include **`resolution`** when using that model; **`negative_prompt`** only for Kling (optional; suggest lines that **protect** warmth and softness).
6. **Pre-submit gate:** Show the confirmation table with the **full expanded `prompt`**; **stop** until explicit approval or edits.
7. **Execute generation:** Run **`video_gen.js`** per **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**—choose **`submit-text`** vs **`submit-image`** by whether an `image` URL is in play. **Default:** async **`submit-*`**; **do not** start a long blocking **`wait`** in the same turn unless the user **already** asked to block until the video is ready.
8. **Immediate notify:** On success with task ids documented in the API file, **immediately** tell the user and **ask** whether to continue with **`status`** or **`wait`**—same rhythm as the API doc’s agent guidance.
9. **Continue (user-driven):** Only after the user agrees, poll **`status`** or run **`wait`** with the **same** JSON—exact flags and intervals are **only** in the API file.
10. When presenting playable URLs, use **Markdown inline links only** (e.g. `[Video](https://…)`). **Do not** wrap those links in code fences.

## CLI reference (minimal; details in API file)

**Full command reference:** **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

After user confirmation, run from the skill package root:

```sh
node scripts/video_gen.js submit-text --json '{"model":"<model_key>","prompt":"<expanded>","duration":10,"aspect_ratio":"9:16","generate_audio":true}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

Add **`negative_prompt`**, **`resolution`** (Seedance only), etc. **only** if allowed for that **`model_key`** in **`## Model and API constraints`**. Image-to-video: use **`submit-image`** and an **`image`** URL as in the API file. Blocking completion: **`wait --json '…'`** with the **same** object—see the API file.

**This skill’s extra filter:** never send **`resolution`** to Kling V3; include **`resolution`** for **Seedance 2.0**; never send **`negative_prompt`** to **Seedance 2.0**.

## Prompt expansion (mandatory)

`video_gen.js` does **not** auto-expand. Before **`submit-*`** or **`wait`**, turn short input into a **full English production `prompt`** aligned with **warm cozy daily** grammar—not a cold blue grade, harsh HDR crunch, high-contrast thriller look, or sterile clinic lighting unless the user explicitly overrides this skill’s look.

**When:** Brief, vague, missing shot/light/audio detail, or **no usable prompt** (topic only)—still build a full `prompt`.

**What to add (checklist):**

- **Color & light:** **orange–amber / golden** key; **soft wrap** fill; **low contrast** with **gentle rolloff**; **subtle bloom / soft halation** on practicals (lamps, window)—avoid **neon** or **laser** glow unless requested.
- **Texture:** **cozy fabrics**, **steam**, **wood grain**, **ceramic**, **fur**—keep **creamy highlights**, not blown clipping.
- **Camera grammar (pick 1–2):** **slow dolly / push-in**; **light handheld sway** (calm breathing, not shaky chaos); **tabletop slide** along a surface; **follow hands** for pour, stir, pet, arrange, flip a book page.
- **Subject & pacing:** match **café**, **home**, **pet**, **routine**, **small joy** cues; **unhurried** beats within **`duration`**.
- **Negative (Kling only, optional):** short lines such as *harsh contrast, cold blue cast, clinical white room, horror mood, aggressive shake, watermark, illegible text*—omit entirely for Seedance.
- **Audio (mandatory when `generate_audio` is true):** labeled **`Audio:`** block—**generic** cues: **soft piano** motif, **folk**-like intimacy, **lo-fi** warm bed, **light acoustic guitar** fingerpicks, soft **room tone**, gentle **café murmur** or **rain on window** at low level; **no** named songs or artists. If the user wants **silent** video: set **`generate_audio`: `false`** and omit audio from the prompt; state that in the confirmation table.

**Style anchors (weave naturally):** *warm cozy, golden light, soft contrast, homely atmosphere, comforting visuals.*

**Length:** Stay within **`prompt_length_limit` 2000** for the selected model; trim adjectives before losing core motion and light intent.

**Confirmation:** The pre-submit table **must** include the **full** expanded `prompt`.

**`### Example prompts`** below are **richness targets only**—always derive from the user's actual brief.

## Definition of done

Done when the user gets at least one playable Markdown inline link (e.g. **`[Video](https://…)`**), or a clear failure with next steps. **Pre-submit:** parameters and **full expanded `prompt`** were **explicitly** confirmed. **After submit:** user was notified with task id(s) and **chose** `status` vs blocking **`wait`**. Submitted JSON **must** match the frozen row for the chosen **`model_key`**. **`generate_audio`** defaults to **`true`** when the model supports audio unless the user requested silent output.

## Boundaries (out of scope)

- Do **not** send **`resolution`** on **`KLING_V3_0_PRO`** or **`KLING_V3_0_STA`**.
- Do **not** send **`negative_prompt`** for **`SEEDANCE_2_0`**.
- Do **not** name **copyrighted** music, **recognizable** branded audio, or **specific** commercial tracks in prompts.
- Do **not** rely on paths or unofficial docs outside this package for CLI/API details; use only **`references/WERYAI_VIDEO_API.md`**.
- Do **not** embed the secret value of `WERYAI_API_KEY` in files.
- Do **not** wrap user-facing playable URLs in Markdown code fences.

### Example prompts

- `9:16 rainy café—steam, amber practicals, slow push toward cup, soft lo-fi piano`
- `Bedside lamp reading nook—golden pool of light, gentle handheld, reassuring folk guitar bed`
- `Golden-hour pet nap—soft fur macro, subtle bloom, calm room ambience`
- `Animate this breakfast table photo—tabletop slide, follow hands, warm cozy grade (image URL)`

---

## Model and API constraints (frozen for this skill)

> Derived from `weryai-model-capabilities-collect.js` at **2026-03-23** (re-run after platform upgrades and refresh this table if values change).

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
| ⭐ Best (default) | `KLING_V3_0_PRO` | 10 | **Omit `resolution`.** Optional **`negative_prompt`** to preserve soft warmth |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** Lower cost than Pro |
| ⚡ Fast / draft | `SEEDANCE_2_0` | 10 | Set **`resolution`** **`720p`** (or **`480p`**). **No `negative_prompt`** |

**Tier keywords:** unspecified / quality-first → **Best**; "balanced" / "good" → **Good**; "draft" / "cheap" / "fast" → **Fast** (`SEEDANCE_2_0`).

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` unless tier or user overrides |
| `aspect_ratio` | **`9:16`** (default vertical cozy); **`16:9`** / **`1:1`** when the user names landscape or square |
| `duration` | `10` (use **`5`** for a single gentle beat; **`15`** on Kling/Seedance for slower cozy arcs) |
| `resolution` | **Only for `SEEDANCE_2_0`:** **`720p`** (or **`480p`**)—**omit for Kling** |
| `generate_audio` | **`true`** unless the user requests silent |
| `negative_prompt` | **Kling only** (optional); e.g. `harsh contrast, cold blue cast, clinical lighting, watermark`—omit for Seedance |

---

## Scenario: Text-to-video warm cozy daily

1. Capture subject (café ritual, pet moment, home corner, small joy) or accept a minimal brief.
2. **Expand** per **`## Prompt expansion (mandatory)`**; pick **`model_key`** from tier rules.
3. **Pre-submit gate** → explicit user confirmation (full **`prompt`**, correct optional fields per model).
4. **`submit-text`** → notify → user chooses **`status`** vs **`wait`**.

---

## Scenario: Image-to-video warm cozy daily

**Before use:** Prefer public **`https://`** URLs. Local paths only with verified runtime support and **explicit consent**.

1. Plan motion (slow push, tabletop slide, hand follow, gentle sway) that **amplifies** the still’s **golden soft** story.
2. Expand prompt; include `image` and **`model_key`** in the confirmation table.
3. After explicit confirm: **`submit-image`** (or **`wait`** if the user pre-requested blocking).

---

## Loop seam (optional)

If the user asks for a **seamless loop**, append at the **end** of the expanded prompt: `seamless loop`, `perfect loop`, `ends where it begins`—no separate API flag.

> Skill: `warm-cozy-daily-video-gen`.
