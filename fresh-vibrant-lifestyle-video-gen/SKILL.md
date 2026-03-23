---
name: fresh-vibrant-lifestyle-video-gen
version: 1.0.0
description: "Generate fresh, high-saturation lifestyle videos with WeryAI—vibrant colors, bright daylight, airy clarity, clean highlights, and lively mood for daily life, travel, food, couples, and outdoor scenes; quick pans, light push/pull, follow moves, upbeat handheld energy; City-pop–inspired, lo-fi upbeat, light electronic, or ukulele-like plucks in audio when models allow. Use when you need energetic social-ready clips from text or a single image; expand short briefs before submit. Default KLING_V3_0_PRO; draft SEEDANCE_2_0."

tags: [lifestyle, travel, food, couples, outdoor, vibrant, saturated, daylight, handheld, social, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Fresh vibrant lifestyle video

**Fresh vibrant lifestyle** in one line: **high saturation** without mud, **bright daylight** or clean high-key fill, **airy transparency** (readable shadows, crisp speculars), **lively** pacing, and **social-native** framing—great for **daily life**, **travel**, **food**, **couples**, and **outdoors**. Camera favors **quick lateral pans**, **small push-in / pull-out**, **follow** movement, and **light handheld** micro-energy (not shaky-cam chaos unless requested). Audio leans **bright groove**—think **city-pop–inspired** brightness, **lo-fi upbeat** bed, **light plucky electronic**, or **ukulele-like** acoustic plucks (**generic** wording only; **no** copyrighted track titles or artist names).

**Try asking (short examples):**

- `9:16 travel brunch—saturated fruit and juice, sun through window, quick pan, upbeat plucky bed`
- `Couple cycling coastal path—bright daylight, follow cam, fresh air mood, light handheld`
- `Street food stall—vibrant signage, steam, fast lateral move, lively city ambience`
- `Animate this picnic photo—small push-in, clean highlights, fresh atmosphere (image URL)`

## Use when

- Someone asks for **fresh**, **vibrant**, **high saturation**, **bright**, **airy**, **clean**, **lifestyle**, **travel vlog**, **food pop**, **couple moment**, **outdoor energy**, or **feel-good** vertical/social clips.
- **Text-to-video** or **image-to-video** (single reference still) with **9:16** (default for Reels/TikTok-style lifestyle), **16:9**, or **1:1**.
- Users mention **vibrant colors**, **bright daylight**, **fresh atmosphere**, **clean highlights**, **lively mood**, **City Pop** vibe, **lo-fi upbeat**, **ukulele**-ish sound, or **quick pan / follow / handheld** motion.

**Visual anchors (weave into expanded prompts):** *vibrant colors, bright daylight, fresh atmosphere, clean highlights, lively mood—plus intentional motion grammar (quick pan, subtle dolly, follow, light handheld).*

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
3. Collect **aspect**: default **`9:16`** for vertical lifestyle / short-form social; use **`16:9`** for landscape travel or desktop; **`1:1`** when requested.
4. **Expand prompt (mandatory):** Unless the user supplied a finished long prompt (within **`prompt_length_limit`**) and asked **not** to rewrite, expand per **`## Prompt expansion (mandatory)`**. **Do not** submit only the user's minimal words.
5. Validate JSON fields against the frozen tables: **Kling** → **omit `resolution`**; **Seedance** → include **`resolution`** when using that model; **`negative_prompt`** only for Kling (optional; suggest lines that **protect** saturation and daylight clarity).
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

`video_gen.js` does **not** auto-expand. Before **`submit-*`** or **`wait`**, turn short input into a **full English production `prompt`** aligned with **fresh vibrant lifestyle** grammar—not a dark cinematic grade, muddy log look, or heavy film desaturation unless the user explicitly overrides this skill’s look.

**When:** Brief, vague, missing shot/light/audio detail, or **no usable prompt** (topic only)—still build a full `prompt`.

**What to add (checklist):**

- **Color & light:** **vibrant colors**, **bright daylight** or clean high-key; **fresh atmosphere**; **clean highlights**; avoid gray mush—prefer **airy** separation between subject and background.
- **Texture:** crisp edges on food/drink details, **readable skin** in sun, **sparkle** on water or glass where appropriate—**not** plastic oversharpen unless requested.
- **Camera grammar (pick 1–2):** **quick lateral pan**; **small push-in or pull-out**; **follow** alongside subject; **light handheld** energy with **short** moves—keep **lively** but controlled unless the user wants aggressive shake.
- **Subject & pacing:** match **daily life / travel / food / couple / outdoor** cues; show **clear focal action** within **`duration`** (bite, laugh, splash, turn, clink).
- **Negative (Kling only, optional):** short lines such as *desaturated, muddy colors, dull gray cast, underexposed, heavy vignette, dirty lens, watermark, illegible text*—omit entirely for Seedance.
- **Audio (mandatory when `generate_audio` is true):** labeled **`Audio:`** block—**bright groove** with **generic** cues: *city-pop–inspired* brightness (no named songs), **lo-fi upbeat** pulse, **light electronic** plucks, **ukulele-like** acoustic rhythm, soft **shaker**, airy **room tone** or **outdoor breeze**; sync micro-hit to motion beats. If the user wants **silent** video: set **`generate_audio`: `false`** and omit audio from the prompt; state that in the confirmation table.

**Style anchors (weave naturally):** *vibrant colors, bright daylight, fresh atmosphere, clean highlights, lively mood.*

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

- `9:16 rooftop coffee—saturated city skyline, sun flare, quick pan, upbeat plucky rhythm`
- `Farmers market haul—bright daylight, colorful produce, follow walk, light handheld, lively ambience`
- `Beach couple splash—fresh blues, clean highlights, small push-in, breezy outdoor sound bed`
- `Turn this dessert photo into motion—macro crumbs, gentle pull-back, sparkling highlights (image URL)`

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
| ⭐ Best (default) | `KLING_V3_0_PRO` | 10 | **Omit `resolution`.** Optional **`negative_prompt`** to keep colors clean and bright |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** Lower cost than Pro |
| ⚡ Fast / draft | `SEEDANCE_2_0` | 10 | Set **`resolution`** **`720p`** (or **`480p`**). **No `negative_prompt`** |

**Tier keywords:** unspecified / quality-first → **Best**; "balanced" / "good" → **Good**; "draft" / "cheap" / "fast" → **Fast** (`SEEDANCE_2_0`).

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` unless tier or user overrides |
| `aspect_ratio` | **`9:16`** (default vertical lifestyle); **`16:9`** / **`1:1`** when the user names landscape or square |
| `duration` | `10` (use **`5`** for a single snappy beat; **`15`** on Kling/Seedance for slower lifestyle arcs) |
| `resolution` | **Only for `SEEDANCE_2_0`:** **`720p`** (or **`480p`**)—**omit for Kling** |
| `generate_audio` | **`true`** unless the user requests silent |
| `negative_prompt` | **Kling only** (optional); e.g. `desaturated look, muddy colors, heavy vignette, watermark`—omit for Seedance |

---

## Scenario: Text-to-video fresh vibrant lifestyle

1. Capture subject (food, travel beat, couple gesture, outdoor action) or accept a minimal brief.
2. **Expand** per **`## Prompt expansion (mandatory)`**; pick **`model_key`** from tier rules.
3. **Pre-submit gate** → explicit user confirmation (full **`prompt`**, correct optional fields per model).
4. **`submit-text`** → notify → user chooses **`status`** vs **`wait`**.

---

## Scenario: Image-to-video fresh vibrant lifestyle

**Before use:** Prefer public **`https://`** URLs. Local paths only with verified runtime support and **explicit consent**.

1. Plan motion (quick pan, small dolly, follow, handheld) that **amplifies** the still’s **color and daylight** story.
2. Expand prompt; include `image` and **`model_key`** in the confirmation table.
3. After explicit confirm: **`submit-image`** (or **`wait`** if the user pre-requested blocking).

---

## Loop seam (optional)

If the user asks for a **seamless loop**, append at the **end** of the expanded prompt: `seamless loop`, `perfect loop`, `ends where it begins`—no separate API flag.

> Skill: `fresh-vibrant-lifestyle-video-gen`.
