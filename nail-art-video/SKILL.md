---
name: nail-art-video
version: 1.0.0
description: "Generate vertical short videos of nail-art process (WeryAI): extreme-detail close-ups—cat-eye, flowing sand, galaxy, cream gel, and 3D accents. Use when you need nail art process video, tutorial-style manicure reels, manicure ASMR, or users ask for holiday themes and micro-encapsulated charms under top coat. SEO: nail art video; nail art generation."

tags: [nail-art, refined, asmr, glow-up, handmade, short-video]

metadata: { "openclaw": { "emoji": "💅", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Nail art video generation

The arc of gel strokes, cat-eye powder snapping into lines under a magnet, mirror gloss after cure—those seconds are the hook. Bunnies or kittens as tiny hand models doing polished nails: every detail is healing content, plus brush ASMR in post for completion rate.

**Dependencies:** `scripts/video_gen.js` in this folder + `WERYAI_API_KEY` in the environment + Node.js 18+. No other Cursor skills required.


## Prerequisites

- `WERYAI_API_KEY` **must be set** in the environment before running `video_gen.js`.
- Node.js **18+** is required. Prefer public **`https`** image URLs. If the assembled `scripts/video_gen.js` supports local file paths, review and verify the script first, then get **explicit consent** before it reads a local image and uploads it to WeryAI to obtain a public URL.
- Each successful `wait` run consumes WeryAI credits; re-running creates new paid tasks.

## Security, secrets, and API hosts

- **`WERYAI_API_KEY`**: Treat as a secret. Only configure it if you trust this skill's source; it is listed in OpenClaw metadata as **`requires.env`** / **`primaryEnv`** so installers know it is mandatory at runtime (never commit it inside the skill package).
- **API hosts (fixed in `video_gen.js`)**: Video tasks use **`https://api.weryai.com`**; the models list uses **`https://api-growth-agent.weryai.com`**. Only **`WERYAI_API_KEY`** is read from the environment—do not rely on URL-related environment variables.
- **Local image handling disclosure**: Prefer public **`https`** image URLs. If the assembled `scripts/video_gen.js` supports local file paths, it may read a local image and upload it to WeryAI to obtain a public URL; require review / verification and explicit consent before using that path.
- **Higher assurance**: Run generation in a short-lived or isolated environment (separate account or container), and review `scripts/video_gen.js` (HTTPS submit + poll loop) before production use. Verify whether the runtime can read local image files and upload them to WeryAI, and obtain explicit consent before using that path.


## Prompt expansion (mandatory)

`video_gen.js` does **not** expand prompts. Before every `wait --json`, turn the user's short or vague brief into a **full English production `prompt`**.

**When:** The user gives only keywords, one line, or loose intent—or asks for richer video language. **Exception:** They paste a finished long prompt within the model's `prompt_length_limit` and ask you **not** to rewrite; still show the **full** text in the confirmation table.

**Always add (video language):** shot scale and angle; camera move or lock-off; light quality and motivation; subject action paced to `duration`; **one clear payoff** for this niche; state **9:16 vertical** when this skill defaults to vertical.

**Length:** Obey `prompt_length_limit` for the chosen `model_key` when this doc lists it; trim filler adjectives before removing core action, lens, or light clauses.

**Confirmation:** The pre-submit table **must** include the **full expanded `prompt`** (never a one-line summary). Wait for **confirm** or edits.

### Niche checklist

- **Macro nails:** base → art steps → **cure/gloss reveal**; magnet cat-eye, decals, or French line as user asks.
- **Light:** soft table or salon diffusion; specular on gel; ASMR brush/scrape if audio on.
- **Hands/paws:** keep motion delicate and readable on vertical crop.

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

- We do not review platform compliance, copyright, or likeness; we do not warrant commercial usability of outputs.
- We do not provide offline rendering outside WeryAI, traditional NLE projects, or API field combinations not documented here.
- Do not hard-code absolute paths in this doc; `{baseDir}` is this skill root (next to `SKILL.md`).

### Example prompts

- `Bunny hand model, galaxy cat-eye gel, magnet pulls a line, vertical macro`
- `From this bare-nail photo: base coat then cream-gel flower build motion`
- `Christmas nails: snowflake studs, final top-coat shine`
- `Nail art tutorial vibe 9:16, macro brush details, cozy table light`

---

## Default parameters

| Field | Value |
|-------|--------|
| Model | KLING_V3_0_PRO |
| Aspect | 9:16 (fixed, vertical hand macro) |
| Duration | 5 s (`duration: 5`, focus on peak moments) |
| Audio | On (gel + UV beeps are core ASMR) |
| Look | Extreme macro, hand close-up, white backdrop, soft diffuse light, gloss-first, high color saturation |

> **API validity (default `KLING_V3_0_PRO`):** Text-to-video: `duration` only **5 / 10 / 15**, `aspect_ratio` only **9:16, 1:1, 16:9**; image-to-video: `aspect_ratio` only **9:16, 16:9, 1:1**; **no `resolution` field—do not send.** Fast VEO tier: text **`VEO_3_1_FAST`**, image **`CHATBOT_VEO_3_1_FAST`**, `duration` **fixed 8**, `aspect_ratio` only **9:16** or **16:9**. For other `model_key` values, follow the allowed sets in this document and the API validity notes above; do not send unsupported fields such as `resolution`.

---

## Anthropomorphic nail art

Tiny paws doing polished nails is a native format for this niche: small scale, gentle motion, rounded nails—ideal for healing macro content.

Describe the character and the nail look; the rest is filled by prompt.

**Flow:**

Collect character + nail design → build a prompt stressing gel gloss, cat-eye lines, mirror top coat → show parameter confirmation → run `node {baseDir}/scripts/video_gen.js wait --json '…'` (fields match the confirmation table)

> About to generate with:
> - model: KLING_V3_0_PRO
> - aspect_ratio: 9:16
> - duration: 5
> - generate_audio: true
> - seamless loop: off (reply "loop" to enable)

**Expanded prompt:** Compose at generation time per `## Prompt expansion (mandatory)` from the user's actual brief—do not reuse fixed sample paragraphs.

**Parameters:**

| Field | Value |
|-------|--------|
| model | KLING_V3_0_PRO |
| aspect_ratio | 9:16 |
| duration | 5 |
| generate_audio | true |

---

## Material macro ASMR (no character)

No character—pure material payoffs: quicksand powder flowing on the nail, cream gel spread with a spatula, gel going from semi-clear to fully cured—texture is the content.

State the material type; generate directly.

**Show all parameters in a table and wait for explicit user confirmation before submitting:**

   > 📋 **Ready to generate—please confirm:**
   >
   > | Field | This run | Notes |
   > |-------|----------|-------|
   > | `model` | `KLING_V3_0_PRO` | Best tier default; fast: text `VEO_3_1_FAST`, image `CHATBOT_VEO_3_1_FAST` (`duration` fixed 8); good → `KLING_V3_0_STA`; or specify a model name |
   > | `aspect_ratio` | `9:16` | Default KLING: 9:16, 1:1, 16:9 only; if you switch model, check that row’s `aspect_ratios` etc. |
   > | `duration` | `5s` | KLING family: 5 / 10 / 15; VEO fast: duration 8 only |
   > | `generate_audio` | `true` | Auto-generate audio or not |
   > | `prompt` | **Full expanded English prompt** (entire text for this run) | Revise before confirm |
   > | `seamless loop` | off | Reply "loop" to add seamless loop |
   >
   > Reply **"confirm"** to start, or list what to change.

**Sample prompt:**

> Extreme close-up macro shot of quicksand nail art being manipulated, a nail's surface shimmers with ultrafine copper and gold sand particles that shift and flow in a liquid-like wave as the hand tilts slowly, microscopic metallic particles catch a focused spot light and scatter rainbow micro-glints, the sand settles into a new pattern with each movement, slow motion 120fps, ASMR dry sand whisper sound, black velvet background to maximize sparkle contrast, static locked-off macro lens

---

## ASMR and texture keywords

**Cat-eye line:** `magnetic cat-eye line gathers precisely`, `aurora shimmer streak`, `luminous moving line under light`, `metalite particles align in perfect gradient`

**Mirror top coat:** `mirror topcoat reflects surroundings`, `glossy gel surface catches every light`, `depth illusion under clear gel encapsulation`, `3D dimensional effect preserved under dome coat`

**Gel ASMR:** `ASMR gel brush stroke sound`, `UV lamp cure beep`, `nail file grit sound`, `cap click of gel bottle`

> **Tip:** Lighting is critical. `soft diffused white backlight` softens reflections; `focused spot light from above` makes cat-eye and glitter pop. State light angle in the prompt for more stable hooks.
