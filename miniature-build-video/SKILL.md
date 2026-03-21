---
name: miniature-build-video
version: 1.0.0
description: "Generate vertical miniature build & reveal shorts (WeryAI): text or finished-shot image to shallow-depth push-ins, lights coming on, immersive camera. Use when you need a miniature diorama reel, tilt-shift room clip, or the user asks for a tiny house lights-on moment or a slow push into interior detail. SEO: miniature build video; miniature model build video generation."

tags: [craft, miniature, immersive, creative, aesthetic, short-video]

metadata: { "openclaw": { "emoji": "🏗️", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Miniature build & diorama video generation

Late night: a matchbox-sized Japanese konbini glows amber; the camera eases out of darkness toward it—bokeh swallows the world while the glass door, shelves, and tiny fluorescents stay sharp. That “small but real” immersion is the emotional hook this skill targets.

Conbini, rainy noodle stand, cyberpunk workshop—or a photo of a finished build: one line or one image → clip.

**Dependencies**: `scripts/video_gen.js` in this directory + `WERYAI_API_KEY` + Node.js 18+. No dependency on other Cursor skills.


## Prerequisites

- `WERYAI_API_KEY` **must be set** in the environment before running `video_gen.js`.
- Node.js **18+** is required. Image inputs **must** be public `https` URLs (no local file paths).
- Each successful `wait` run consumes WeryAI credits; re-running creates new paid tasks.

## Security, secrets, and API hosts

- **`WERYAI_API_KEY`**: Treat as a secret. Only configure it if you trust this skill's source; it is listed in OpenClaw metadata as **`requires.env`** / **`primaryEnv`** so installers know it is mandatory at runtime (never commit it inside the skill package).
- **Optional URL overrides** (`WERYAI_BASE_URL`, `WERYAI_MODELS_BASE_URL`): `video_gen.js` defaults to **`https://api.weryai.com`** and **`https://api-growth-agent.weryai.com`**. Overrides are intended for testing or approved alternate endpoints. If these variables are set in your environment, confirm they point to hosts you trust—otherwise prompts, images, and your bearer token could be sent elsewhere.
- **Higher assurance**: Run generation in a short-lived or isolated environment (separate account or container), and review `scripts/video_gen.js` (HTTPS submit + poll loop) before production use.


## Prompt expansion (mandatory)

`video_gen.js` does **not** expand prompts. Before every `wait --json`, turn the user's short or vague brief into a **full English production `prompt`**.

**When:** The user gives only keywords, one line, or loose intent—or asks for richer video language. **Exception:** They paste a finished long prompt within the model's `prompt_length_limit` and ask you **not** to rewrite; still show the **full** text in the confirmation table.

**Always add (video language):** shot scale and angle; camera move or lock-off; light quality and motivation; subject action paced to `duration`; **one clear payoff** for this niche; state **9:16 vertical** when this skill defaults to vertical.

**Length:** Obey `prompt_length_limit` for the chosen `model_key` when this doc lists it; trim filler adjectives before removing core action, lens, or light clauses.

**Confirmation:** The pre-submit table **must** include the **full expanded `prompt`** (never a one-line summary). Wait for **confirm** or edits.

### Niche checklist

- **Miniature world:** shallow DOF, practical warm lights, **dolly/push-in** reveal; diorama or room-in-a-box.
- **Beats:** build → paint → **lights on** or vehicle pass; cozy atmosphere, tactile materials.
- **Scale cues:** matchstick, coin, or fingertip references where useful.

**`### Example prompts`** at the top of this file are **short triggers only**—always expand from the user's actual request.

## Workflow

1. Confirm the user request matches this skill's scenario (text-to-video and/or image-to-video as documented).
2. Collect the user's **brief**, optional image URL(s), tier (**best** / **good** / **fast**) or an explicit `model` key.
3. **Expand prompt (mandatory):** Unless the user supplied a finished long prompt and explicitly asked not to rewrite it, expand the brief into a full English production `prompt` using `## Prompt expansion (mandatory)` below. **Do not** call the API with only the user's minimal words.
4. Check the **expanded** `prompt` against the selected model's `prompt_length_limit` in the frozen tables in this document (when present); shorten if needed.
5. Verify `duration`, `aspect_ratio`, `resolution`, `generate_audio`, `negative_prompt`, and other fields against the frozen tables and API notes in this SKILL.md.
6. Show the pre-submit parameter table including the **full expanded `prompt`**; wait for **confirm** or edits.
7. After confirmation, run `node {baseDir}/scripts/video_gen.js wait --json '...'` with the **expanded** prompt.
8. Parse stdout JSON and return video URLs; on failure, surface `errorCode` / `errorMessage` and suggest parameter fixes.

## CLI reference

```sh
node {baseDir}/scripts/video_gen.js wait --json '{"model":"…","prompt":"…","duration":5,"aspect_ratio":"9:16"}'
node {baseDir}/scripts/video_gen.js wait --json '…' --dry-run
node {baseDir}/scripts/video_gen.js status --task-id <id>
```

## Definition of done

Done when the user receives at least one playable video URL from the API response, or a clear failure explanation with next steps. All parameters used **must** fall within the selected model's allowed sets in this document. The submitted `prompt` **must** be the **expanded** production prompt unless the user explicitly supplied a finished long prompt and asked not to rewrite it.

## Boundaries (out of scope)

- Does not review platform compliance, copyright, or portrait rights; does not guarantee commercial usability of outputs.
- Does not provide non-WeryAI offline rendering, traditional edit timelines, or API field combinations not documented here.
- Does not hard-code absolute paths in the skill doc; `{baseDir}` means the skill package root (same level as `SKILL.md`).

### Example prompts

- `Hand-built miniature café: final beat lights on inside, warm fill, slow push vertical`
- `Use this street diorama still: tiny car passes, shop windows light up`
- `Painted model on base, rotate reveal, ultra-shallow depth of field`
- `Miniature build reveal 9:16, cozy warm lights turning on inside tiny house`

---

## Default parameters

| Field | Value |
|-------|-------|
| Model | KLING_V3_0_PRO |
| Aspect ratio | 9:16 (fixed vertical) |
| Duration | Short (`duration`: 5); use 10 for a longer build pass |
| Style | Very shallow DOF, believable materials, accent light on hero props, slow push or subtle orbit (fixed) |
| Audio | On (room tone + tiny prop detail for immersion) |

> **API validity (default `KLING_V3_0_PRO`):** Text-to-video `duration` only **5 / 10 / 15**; `aspect_ratio` only **9:16, 1:1, 16:9**. Image-to-video `aspect_ratio` only **9:16, 16:9, 1:1**. **No `resolution` field—do not send it.** For **fast** tier with VEO: text-to-video **`VEO_3_1_FAST`**, image-to-video **`CHATBOT_VEO_3_1_FAST`**, with `duration` **fixed at 8**, `aspect_ratio` only **9:16** or **16:9**. When switching `model_key`, follow the allowed sets in this section’s model/API constraints and the API validity note above; do not send `resolution` to models that do not support it.

---

## Scene description → miniature world

**Purpose:** Concrete scene ideas, fast visual tests, or batching mood variants—no physical model required.

**Inputs:**

**Scene type** (konbini / kitchen / cottage / corner stall / workshop / cyber set) and **mood** (warm healing / neon cyber / daylight / rainy amber). If mood is omitted, pick the lighting that usually performs for that scene. Optionally stress **build process** vs **finished tour**.

Extract key props and light, pick an angle that triggers “I want this,” build the prompt, show full parameters, wait for confirm, then `node {baseDir}/scripts/video_gen.js wait --json '…'`.

> Full parameters are shown before generation; wait for confirmation.

**Parameter configuration:**

| Field | Value |
|-------|-------|
| model | KLING_V3_0_PRO |
| aspect_ratio | 9:16 |
| duration | 5 |
| generate_audio | true |

Example inputs:

“Japanese konbini at night, warm healing” → the skill builds:

> Extreme close-up macro shot at eye level, a palm-sized miniature Japanese convenience store diorama on a dark surface, warm amber interior light glows through tiny sliding glass doors and frostwork windows illuminating a perfectly detailed interior — miniature shelves stocked with thumb-sized product packages, a tiny register counter, a softbox ceiling panel. The camera dollies in very slowly at 30fps toward the entrance as if walking toward it, the shallow depth of field keeps only the storefront razor-sharp while the surrounding darkness melts away. Cold blue night-time ambient fills the exterior, creating a warm-cold color split at the door threshold. ASMR soft ambient hum, distant city night sound, faint interior muzak.

“Cyberpunk workshop, neon” →

> Medium close-up at 45-degree overhead angle, a miniature cyberpunk studio diorama the size of a shoebox — crammed with tiny glowing monitors, cable-draped desk, neon-lit bookshelves in pink and cyan, a miniature action figure at the keyboard. The camera orbits slowly around the scene at 60fps in a single smooth rotation, neon light refracts off every tiny reflective surface — monitor bezels, coffee mug, glossy floor tile. Volumetric light rays from the neon tubes create a soft haze above the scene. Deep black void background isolates the diorama completely. ASMR low electronic hum, faint keyboard clicks, ambient synth drone.

“Hand-build a log cabin from scratch, full process” →

> Close-up tracking shot over a clean white work mat, precise hands assemble a miniature log cabin frame piece by piece in time-lapse — wooden strips cut, glued, pressed together, roof tiles laid row by row, a tiny front door hung on micro hinges. Final sequence returns to real time 60fps: a hand places the last roof tile, steps back, and a small warm interior light switches on inside the finished cabin, its glow spilling through two windows onto the mat. Neutral overhead workshop light throughout, shallow depth of field isolates the build in progress from surrounding tools. ASMR wood snapping, glue applicator, the soft click of the interior light turning on.

> **Note:** More specific beats (palette, hero prop, time of day) track intent better. “Konbini” vs “late-night warm konbini push toward glass door” are different pictures.

---

## Model photo → immersive motion

**Purpose:** Finished physical model or reference still → camera motion for posts or archive.

**Inputs:**

**Image URL** (public HTTPS). Optional motion direction:
- **Slow push in:** From outside toward interior or front—depth emphasis
- **Lights on:** Dark to lit, sources coming up in sequence
- **Orbit:** Slow 360-style pass around the build

Default pick: if already bright inside → push; if dark → lights-on.

Read scene, existing light, and hero props; match motion; show parameters; confirm; `node {baseDir}/scripts/video_gen.js wait --json '…'`.

> Full parameters are shown before generation; wait for confirmation.

**Parameter configuration:**

| Field | Value |
|-------|-------|
| model | KLING_V3_0_PRO |
| aspect_ratio | 9:16 |
| duration | 5 |
| generate_audio | true |
| image | User-supplied image URL |

Example:

“This is my cyber street build, orbit it https://...” →

> Starting from the miniature diorama in the image, the camera performs a slow smooth 180-degree orbital rotation around the scene at 30fps, each angle revealing new details of the model — signage, light sources, material textures. Shallow depth of field maintains focus on the nearest model surface throughout the rotation, blurring the far side into bokeh. The existing lighting in the image is preserved and enhanced with subtle lens flare at bright neon sources. ASMR ambient city soundscape matching the scene aesthetic.

> **Note:** URL must be public HTTPS—local paths and some in-app links fail. Clearer, more centered photos track camera motion better.

---

**Prompt keyword cheat sheet**

- Immersion: `shallow depth of field bokeh background`, `only the miniature in sharp focus`, `diorama isolated on dark surface`
- Light: `warm amber spill light`, `neon volumetric haze`, `cold-warm color contrast at threshold`, `interior light switching on`
- Camera: `slow dolly push-in`, `smooth orbital rotation`, `static locked-off reveal`, `gradual light fade-in`
- Materials: `visible wood grain texture`, `reflective tiny surface`, `thumb-sized detail props`, `micro scale furniture`
- Emotion: `inviting glow`, `tiny world feeling`, `perfectly scaled details`, `a world you could step into`
