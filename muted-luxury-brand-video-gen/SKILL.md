---
name: muted-luxury-brand-video-gen
version: 1.0.0
description: "Generate muted luxury brand videos with WeryAI—desaturated palette, unified gray tonality, soft contrast, editorial fashion tone, and premium minimal motion for apparel, interiors, beauty, and design. Use when you need very slow lateral drift, slight dolly-in, static hero framing with subtle local motion, and minimal electronic / ambient runway-style audio; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [muted, luxury, desaturated, editorial, fashion, brand, minimalism, soft-contrast, gray-tone, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Muted luxury & editorial brand video

**Muted luxury video** in one line: **low saturation**, **unified gray tonality**, **soft contrast**, **restrained texture**, and **calm premium mood**—**not** candy neons, **not** punchy commercial saturation. Camera favors **very slow horizontal drift**, **gentle slow dolly-in**, **locked hero composition**, and **micro-motion in small zones** (fabric breath, steam, curtain sway, liquid ripple). Audio stays **minimal**: **sparse electronic pulses**, **wide ambient pads**, **fashion-runway underscore feel** (all **generic**—no named tracks or brands), plus subtle **room tone** and light **foley**.

**Style anchor tag (optional in user briefs):** `muted_luxury_brand_video`.

## Use when

- Someone asks for **muted luxury**, **desaturated brand film**, **editorial fashion tone**, **premium minimalism**, **soft contrast**, **gray-forward grading**, or **quiet high-end campaign** energy for **apparel**, **interiors**, **beauty**, or **design** subjects.
- **Text-to-video** or **image-to-video** (single reference still) with **9:16** (default for vertical brand/social), **16:9** (widescreen lookbook / space), or **1:1**.
- Users mention **very slow pan**, **slight push-in**, **static composition**, **local micro-movement**, **ambient electronic**, **runway / show music mood** (generic), or phrases like *muted palette, desaturated luxury, soft contrast, editorial fashion tone, premium minimalism*.

**Not this skill:** **high-saturation** retail promos, **neon nightlife**, **blockbuster contrast**, or **loud meme energy**—unless the user explicitly overrides and accepts leaving the default muted-luxury grammar.

**Model choice (this package):** **⭐ Best** → **`KLING_V3_0_PRO`** (default for controlled micro-motion and restrained grade). **👍 Good** → **`KLING_V3_0_STA`**. **⚡ Fast / draft** → **`SEEDANCE_2_0`** with **`resolution`** **`720p`** (omit **`negative_prompt`** for this model). Kling rows **must omit `resolution`** from JSON.

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
3. Collect **aspect**: default **`9:16`** for vertical brand/social; use **`16:9`** for widescreen lookbook / architecture / campaign; **`1:1`** when the user requests square feeds.
4. **Expand prompt (mandatory):** Unless the user supplied a finished long prompt (within **`prompt_length_limit`**) and asked **not** to rewrite, expand per **`## Prompt expansion (mandatory)`**. **Do not** submit only the user's minimal words.
5. Validate JSON fields against the frozen tables: **Kling** → **omit `resolution`**; **Seedance** → include **`resolution`** when using that model; **`negative_prompt`** only for Kling (optional but recommended with a short luxury-safe line that rejects garish saturation).
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

`video_gen.js` does **not** auto-expand. Before **`submit-*`** or **`wait`**, turn short input into a **full English production `prompt`** aligned with **muted luxury / editorial brand** grammar—**restraint and tonal unity over pop**.

**When:** Brief, vague, missing shot/light/audio detail, or **no usable prompt** (topic only)—still build a full `prompt`.

**What to add (checklist):**

- **Look & grade:** **muted palette**, **desaturated luxury**, **soft contrast**, **unified gray tonality** (warm gray, cool gray, or neutral—pick one family and stay consistent), **matte highlights**, **controlled speculars**, **premium material read** (wool, stone, glass, ceramic, skin with **understated** sheen). Avoid **neon**, **hyper-saturation**, **rainbow gradients**, and **busy graphic overlays** unless the user overrides.
- **Subject fit:** frame as **fashion editorial**, **quiet interior**, **beauty macro**, or **design object hero**—**negative space** and **clean lines**; **no** shouty retail stickers unless requested.
- **Composition:** **static or near-static hero frame**; **center-weighted** or **editorial thirds**; **long lens compression** optional for product dignity.
- **Camera grammar (pick 1–2):** **very slow lateral drift** (slider pace, almost imperceptible); **gentle slow dolly-in**; **locked-off tableau** with **only local motion** (fabric, hair micro-movement, steam, curtain, liquid meniscus, dust in soft light)—**not** whip pans, **not** fast handheld chaos.
- **Audio (mandatory when `generate_audio` is true`):** labeled **`Audio:`** block—**minimal electronic** (soft pulses, sparse arpeggios), **wide ambient pads**, **fashion-show runway underscore mood** (generic, no recognizable songs), **low room tone**, subtle **foley** (fabric rustle, distant HVAC hush); **no** copyrighted music or recognizable branded cues. If the user wants **silent** video: set **`generate_audio`: `false`** and omit audio from the prompt; state that in the confirmation table.

**Style anchors (weave naturally):** *muted palette, desaturated luxury, soft contrast, editorial fashion tone, premium minimalism, muted_luxury_brand_video.*

**Length:** Stay within **`prompt_length_limit` 2000** for the selected model in the frozen table; drop lower-priority adjectives before losing tonal unity and camera restraint.

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

- `9:16 muted luxury—gray wool coat on a concrete plinth, very slow lateral drift, soft contrast, minimal ambient electronic`
- `16:9 editorial interior—limestone and oak, locked composition, curtain micro-sway, desaturated luxury, runway underscore mood`
- `Beauty macro—matte skin, cool gray backdrop, gentle dolly-in, steam wisps only, premium minimalism`
- `Design object hero—brushed aluminum, unified gray grade, static frame, subtle reflection ripple, sparse pulses`
- `Animate this still—muted palette, slight push-in, local fabric motion only, editorial fashion tone (image URL in JSON)`

---

## Model and API constraints (frozen for this skill)

> Derived from `node scripts/weryai-model-capabilities-collect.js --mode …` (snapshot `2026-03-23T10:03:40.784Z`). **Re-run** after platform upgrades and refresh this table if values change.

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
| ⭐ Best (default) | `KLING_V3_0_PRO` | 10 (use **15** for slower luxury drift) | **Omit `resolution`.** Optional **`negative_prompt`** e.g. `oversaturated colors, neon, candy pop, busy graphics, harsh clipping, comic look, watermark, flashy transitions, loud commercial polish` |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** Lower cost than Pro |
| ⚡ Fast / draft | `SEEDANCE_2_0` | 10 | Set **`resolution`** **`720p`** (or **`480p`**). **No `negative_prompt`** |

**Tier keywords:** unspecified / quality-first → **Best**; "balanced" / "good" → **Good**; "draft" / "cheap" / "fast" → **Fast** (`SEEDANCE_2_0`).

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` unless tier or user overrides |
| `aspect_ratio` | **`9:16`** (default); **`16:9`** or **`1:1`** when the user targets widescreen lookbook / space or square feeds |
| `duration` | `10` (use **`5`** for a single tableau beat; **`15`** on Kling/Seedance for slower drift) |
| `resolution` | **Only for `SEEDANCE_2_0`:** **`720p`** (or **`480p`**)—**omit for Kling** |
| `generate_audio` | **`true`** unless the user requests silent |
| `negative_prompt` | **Kling only** (optional); keep lines compatible with **muted luxury**—omit for Seedance |

---

## Scenario: Text-to-video muted luxury brand

1. Capture **category** (apparel / space / beauty / design), **hero subject**, and **tonal family** (warm gray vs cool gray)—or accept a minimal brief.
2. **Expand** per **`## Prompt expansion (mandatory)`**; pick **`model_key`** from tier rules.
3. **Pre-submit gate** → explicit user confirmation (full **`prompt`**, correct optional fields per model).
4. **`submit-text`** → notify → user chooses **`status`** vs **`wait`**.

---

## Scenario: Image-to-video muted luxury brand

**Before use:** Prefer public **`https://`** URLs. Local paths only with verified runtime support and **explicit consent**.

1. Plan motion (**very slow pan**, **slight dolly-in**, **local micro-motion only**) and respect product likeness if requested.
2. Expand prompt; include `image` and **`model_key`** in the confirmation table.
3. After explicit confirm: **`submit-image`** (or **`wait`** if the user pre-requested blocking).

---

## Loop seam (optional)

If the user asks for a **seamless loop**, append at the **end** of the expanded prompt: `seamless loop`, `perfect loop`, `ends where it begins`—no separate API flag.

> Skill: `muted-luxury-brand-video-gen`.
