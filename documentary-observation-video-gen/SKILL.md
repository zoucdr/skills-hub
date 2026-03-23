---
name: documentary-observation-video-gen
version: 1.0.2
description: "Generate documentary-observation style videos with WeryAI—natural light, authentic skin tone, handheld follow and slight shake, long-take feel, observational pans, mild exposure drift, and production-style ambience plus light documentary piano or underscore. Use when you need vérité-style character docs, interview B-roll, follow-cam street scenes, or image-to-video from a still; expand short briefs before submit. Paid API; default KLING_V3_0_PRO, draft SEEDANCE_2_0."

tags: [documentary, observational, handheld, interview, follow-cam, realism, natural-light, verite, text-to-video, image-to-video, video-gen]

metadata: { "openclaw": { "emoji": "🎬", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Documentary observation & vérité-style video

**Documentary observation video** in one line: **unpolished authenticity**, **present-tense “this is happening” energy**, **natural light**, **believable skin**, **handheld realism** with **gentle micro-shake**, **long-take** pacing, **observational pans**, and **mild exposure breathing**—**not** glam beauty lighting or music-video polish. Audio favors **location ambience**, **room tone**, and **sparse documentary piano / light underscore** (all **generic**, no named films or scores).

**Try asking (short examples):**

- `Handheld follow behind a cyclist at golden hour—natural light, subtle shake, city ambience`
- `Interview cutaway: observational pan across a workshop, long lens, authentic skin, room tone`
- `16:9 doc scene—two people talking in a kitchen, available light, slight handheld drift`
- `Bring this photo to life—gentle handheld, documentary realism, soft piano and ambience (image URL)`

## Use when

- Someone asks for **documentary**, **doc style**, **vérité** / **verité**, **observational**, **fly-on-the-wall**, **raw** or **ungraded** realism, **character study**, **street follow**, **behind-the-scenes**, or **interview** atmosphere.
- **Text-to-video** or **image-to-video** (single reference still) with **16:9** (default for classic doc framing), **9:16**, or **1:1**.
- Users mention **documentary realism**, **natural light**, **authentic skin tone**, **observational camera**, **handheld realism**, **long take**, **slight wobble**, **available light**, or **production sound** / **ambient** audio.

**Visual anchors (weave into expanded prompts):** *documentary realism, natural light, authentic skin tone, observational camera, handheld realism, mild exposure fluctuation, imperfect but intentional framing, depth from real environments—not studio keys.*

**Model choice (this package):** **⭐ Best** → **`KLING_V3_0_PRO`** (default for skin, available light, and subtle motion). **👍 Good** → **`KLING_V3_0_STA`**. **⚡ Fast / draft** → **`SEEDANCE_2_0`** with **`resolution`** **`720p`** (omit **`negative_prompt`** for this model). Kling rows **must omit `resolution`** from JSON.

## Runtime docs vs this skill

- **When to open [`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md):** whenever you need **authoritative** `video_gen.js` usage—`submit-text` / `submit-image` / `submit-multi-image`, `status`, `wait`, **full JSON shape**, stdout fields, errors, **`https` image rules**, optional local-file upload behavior, polling/backoff, env vars, and fixed API hosts. **Do not** duplicate that material here.
- **When to run `scripts/video_gen.js`:** only **after** the user passes **`## Pre-submit gate (mandatory)`**, from the **skill package root**, using the **same** JSON as documented in the API file. Use **`models`** (see API file) when refreshing platform metadata.

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
3. Collect **aspect**: default **`16:9`** for broadcast-style documentary; use **`9:16`** for mobile doc / vertical social; **`1:1`** when requested.
4. **Expand prompt (mandatory):** Unless the user supplied a finished long prompt (within **`prompt_length_limit`**) and asked **not** to rewrite, expand per **`## Prompt expansion (mandatory)`**. **Do not** submit only the user's minimal words.
5. Validate JSON fields against the frozen tables: **Kling** → **omit `resolution`**; **Seedance** → include **`resolution`** when using that model; **`negative_prompt`** only for Kling (optional but recommended with a short doc-safe line).
6. **Pre-submit gate:** Show the confirmation table with the **full expanded `prompt`**; **stop** until explicit approval or edits.
7. **Execute generation:** Run **`video_gen.js`** per **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**—choose **`submit-text`** vs **`submit-image`** by whether an `image` URL is in play. **Default:** async **`submit-*`**; **do not** start a long blocking **`wait`** in the same turn unless the user **already** asked to block until the video is ready.
8. **Immediate notify:** On success with task ids documented in the API file, **immediately** tell the user and **ask** whether to continue with **`status`** or **`wait`**—same rhythm as the API doc’s agent guidance.
9. **Continue (user-driven):** Only after the user agrees, poll **`status`** or run **`wait`** with the **same** JSON—exact flags and intervals are **only** in the API file.
10. When presenting playable URLs, use **Markdown inline links only** (e.g. `[Video](https://…)`). **Do not** wrap those links in code fences.

## CLI reference (minimal; details in API file)

**Full command reference:** **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)**.

After user confirmation, run from the skill package root:

```sh
node scripts/video_gen.js submit-text --json '{"model":"<model_key>","prompt":"<expanded>","duration":10}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
```

Add **`aspect_ratio`**, **`generate_audio`**, **`negative_prompt`**, etc. **only** if allowed for that **`model_key`** in **`## Model and API constraints`**. Image-to-video: use **`submit-image`** and an **`image`** URL as in the API file. Blocking completion: **`wait --json '…'`** with the **same** object—see the API file.

**This skill’s extra filter:** never send **`resolution`** to Kling V3; include **`resolution`** for **Seedance 2.0**; never send **`negative_prompt`** to **Seedance 2.0**.

## Prompt expansion (mandatory)

`video_gen.js` does **not** auto-expand. Before **`submit-*`** or **`wait`**, turn short input into a **full English production `prompt`** aligned with **documentary observation** grammar—not a glossy commercial or trailer look unless the user explicitly asks to depart from doc realism.

**When:** Brief, vague, missing shot/light/audio detail, or **no usable prompt** (topic only)—still build a full `prompt`.

**What to add (checklist):**

- **Realism & light:** **available light** or **soft window light**; **natural color**; **authentic skin tone**; **gentle highlight roll-off**; allow **slight exposure drift** between frames if it sells “live camera.”
- **Camera grammar (pick 1–2):** **handheld follow** or **walk-along**; **observational pan** across a real space; **long-lens compression** for interviews; **modest micro-shake**—never chaotic nausea unless the user requests it. Avoid **gimbal-perfect** float unless they ask for stabilized doc.
- **Composition:** **imperfect but believable** framing; **foreground objects** for depth; **eye-line** continuity for dialogue beats; **respect documentary distance** (not fashion close-ups unless brief says portrait doc).
- **Mood:** **“happening now”**—small real-world actions (gesture, turning head, hands working) rather than posed hero poses.
- **Negative prompts (Kling only, optional):** e.g. `beauty filter, plastic skin, heavy makeup glam, music video lighting, CGI sheen, watermark, illegible text`—trim if near limit.
- **Audio (mandatory when `generate_audio` is true):** labeled **`Audio:`** block—**production ambience** (room tone, distant traffic, wind, café murmur as fits), **sparse documentary piano** or **very light neutral underscore**, subtle **foley** tied to hands/environment; **no** copyrighted tracks, named documentaries, or recognizable branded cues. If the user wants **silent** video: set **`generate_audio`: `false`** and omit audio from the prompt; state that in the confirmation table.

**Style anchors (weave naturally):** *documentary realism, natural light, authentic skin tone, observational camera, handheld realism.*

**Length:** Stay within **`prompt_length_limit` 2000** for the selected model in the frozen table; drop lower-priority adjectives before losing the core observational beat.

**Confirmation:** The pre-submit table **must** include the **full** expanded `prompt`.

**`### Example prompts`** below are **richness targets only**—always derive from the user's actual brief.

## Definition of done

Done when the user gets at least one playable Markdown inline link whose label is **Video** and whose target is **`https://…`**, or a clear failure with next steps. **Pre-submit:** parameters and **full expanded `prompt`** were **explicitly** confirmed. **After submit:** user was notified with task id(s) and **chose** `status` vs blocking **`wait`**. Submitted JSON **must** match the frozen row for the chosen **`model_key`**. **`generate_audio`** defaults to **`true`** when the model supports audio unless the user requested silent output.

## Boundaries (out of scope)

- Do **not** send **`resolution`** on **`KLING_V3_0_PRO`** or **`KLING_V3_0_STA`**—it is unsupported and may cause parameter errors.
- Do **not** send **`negative_prompt`** for **`SEEDANCE_2_0`** (not supported).
- Do **not** invent CLI flags, hosts, or JSON fields—use **[`references/WERYAI_VIDEO_API.md`](references/WERYAI_VIDEO_API.md)** for anything not listed in **`## Model and API constraints`**.
- Do **not** embed the secret value of `WERYAI_API_KEY` in files.
- Do **not** wrap user-facing playable URLs in Markdown code fences.
- Do **not** name real broadcast series, networks, or copyrighted scores in prompts—use **generic** doc-audio language only.
- This skill is **not** for **epic blockbuster**, **music-video glam**, or **heavy VFX spectacle** unless the user explicitly overrides the documentary brief.

### Example prompts

- `16:9 vérité—handheld follow through a rainy street market, natural light, authentic faces, city ambience and light piano`
- `Interview B-roll: slow observational pan across tools on a bench, shallow depth, window light, room tone`
- `Long-take feel: documentary realism in a community kitchen, two people listening, subtle handheld drift, mild exposure breathing`
- `9:16 vertical doc beat—subject walks and talks to camera, handheld, natural skin, wind and distant traffic`
- `Image to video: gentle handheld on a portrait in soft daylight—observational, documentary piano bed (image URL in JSON)`

---

## Model and API constraints (frozen for this skill)

> Aligned with **`cinematic-story-video-gen`** and repository WeryAI metadata snapshot (**2026-03-21**). **Re-run** `node scripts/video_gen.js models` from an assembled package (with working `video_gen.js`) after platform upgrades and refresh this table if values change.

### Text-to-video

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | prompt limit |
|-----------|-----------|---------------|-------------|-------|-----------------|--------------|
| `KLING_V3_0_PRO` | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit—do not send)* | Yes | Yes | 2000 |
| `KLING_V3_0_STA` | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit—do not send)* | Yes | Yes | 2000 |
| `SEEDANCE_2_0` | 5, 10, 15 | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No (do not send) | 2000 |

### Image-to-video (single `image`)

| model_key | durations | aspect_ratios | resolutions | Audio | negative_prompt | Notes |
|-----------|-----------|---------------|-------------|-------|-----------------|--------|
| `KLING_V3_0_PRO` | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | Yes | Yes | single `image` |
| `KLING_V3_0_STA` | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | Yes | Yes | single `image` |
| `SEEDANCE_2_0` | 5, 10, 15 | 9:16, 1:1, 16:9 | 480p, 720p | Yes | No (do not send) | `upload_image_limit` **3** (this skill documents single-image flow by default) |

---

## Recommended models

| Tier | model_key | Default duration | Notes |
|------|-----------|------------------|--------|
| ⭐ Best (default) | `KLING_V3_0_PRO` | 10 (use **15** for slower observational beats) | **Omit `resolution`.** Optional **`negative_prompt`** for clean doc output |
| 👍 Good | `KLING_V3_0_STA` | 10 | **Omit `resolution`.** Lower cost than Pro |
| ⚡ Fast / draft | `SEEDANCE_2_0` | 10 | Set **`resolution`** **`720p`** (or **`480p`**). **No `negative_prompt`** |

**Tier keywords:** unspecified / quality-first → **Best**; "balanced" / "good" → **Good**; "draft" / "cheap" / "fast" → **Fast** (`SEEDANCE_2_0`).

---

## Default parameters

| Field | Value |
|-------|--------|
| `model` | `KLING_V3_0_PRO` unless tier or user overrides |
| `aspect_ratio` | **`16:9`** (classic doc); **`9:16`** / **`1:1`** when the user names mobile or square |
| `duration` | `10` (use **`5`** for a single beat; **`15`** for slower long-take feel) |
| `resolution` | **Only for `SEEDANCE_2_0`:** **`720p`** (or **`480p`**)—**omit for Kling** |
| `generate_audio` | **`true`** unless the user requests silent |
| `negative_prompt` | **Kling only** (optional); e.g. `beauty filter, plastic skin, music video lighting`—omit for Seedance |

---

## Scenario: Text-to-video documentary observation

1. Capture subject, location, and intent (interview B-roll, follow-cam, slice-of-life—or accept a minimal brief).
2. **Expand** per **`## Prompt expansion (mandatory)`**; pick **`model_key`** from tier rules.
3. **Pre-submit gate** → explicit user confirmation (full **`prompt`**, correct optional fields per model).
4. **`submit-text`** → notify → user chooses **`status`** vs **`wait`**.

---

## Scenario: Image-to-video documentary observation

**Before use:** Prefer public **`https://`** URLs. Local paths only with verified runtime support and **explicit consent**.

1. Plan motion (subtle handheld, pan, micro push) and respect likeness if requested.
2. Expand prompt; include `image` and **`model_key`** in the confirmation table.
3. After explicit confirm: **`submit-image`** (or **`wait`** if the user pre-requested blocking).

---

## Loop seam (optional)

If the user asks for a **seamless loop**, append at the **end** of the expanded prompt: `seamless loop`, `perfect loop`, `ends where it begins`—no separate API flag.

> Skill: `documentary-observation-video-gen`.
