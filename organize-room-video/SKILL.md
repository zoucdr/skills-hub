---
name: organize-room-video
version: 1.0.0
description: "Generate vertical shorts of organizing from chaos to order (WeryAI): closets, fridges, vanities, desks, luggage. Use when you need organizing satisfying video, declutter ASMR, before/after tidy clips, or users ask for rainbow-fold stacks and the snap of a clear lid. SEO: organize room video; organizing video generation."

tags: [organizing, satisfying, healing, neat-freak, short-video, lifestyle]

metadata: { "openclaw": { "emoji": "📦", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Organizing & declutter video generation

Picture this: a messy pile of clothes, sorted by a bear’s paws into neat rows, colors shifting into a gradient rainbow—the moment the clear box lid clicks, the frame goes still in the best way. That’s what this skill delivers; describe the scene in one line and generate.

**Dependencies:** `scripts/video_gen.js` in this folder + `WERYAI_API_KEY` + Node.js 18+. No other Cursor skills required.


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

- **Chaos → order:** zones, containers, **color unity**, satisfying snap/align; overhead or 45° for layout reads.
- **Character optional:** anthropomorphic tidy arc or hands-only; strong **before/after** density change.
- **Motion:** time-lapse stack, fold, or slide-in—one clear payoff beat.

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

- `Exploded closet, bear folds clothes into a color gradient, door close one-take`
- `From this messy desk: sort into boxes, labels aligned`
- `Fridge before/after, bottles turned uniform—OCD comfort`
- `Organizing satisfying 9:16, messy to perfectly aligned containers`

---

## Default parameters

| Field | Value |
|-------|--------|
| Model | KLING_V3_0_PRO |
| Aspect | 9:16 (fixed, vertical short) |
| Duration | 5 s (`duration: 5`, peak moments first) |
| Audio | Off (pair with BGM; beat cuts work better) |
| Look | Overhead or ~45° close, soft diffuse light, strong color unity, extreme before/after contrast |

> **API validity (default `KLING_V3_0_PRO`):** Text-to-video: `duration` only **5 / 10 / 15**, `aspect_ratio` only **9:16, 1:1, 16:9**; image-to-video: `aspect_ratio` only **9:16, 16:9, 1:1**; **no `resolution` field—do not send.** Fast VEO tier: text **`VEO_3_1_FAST`**, image **`CHATBOT_VEO_3_1_FAST`**, `duration` **fixed 8**, `aspect_ratio` only **9:16** or **16:9**. For other `model_key` values, follow the allowed sets in this document and the API validity notes above; do not send unsupported fields such as `resolution`.

---

## Anthropomorphic organizing

Algorithm-friendly organizing: cute animals (bear / bunny / cat) take a space from total mess to obsessively neat—clear bins, unified palette, drawer zones are visual peaks.

User gives character type + space; the rest is prompt-filled.

**Flow:**

Collect character + scene → build a prompt with full chaos→order arc and key satisfying beats → show parameter confirmation → run `node {baseDir}/scripts/video_gen.js wait --json '…'` (fields match the confirmation table)

> Full parameters are shown before generate; wait for confirmation:
> - model: KLING_V3_0_PRO
> - aspect_ratio: 9:16
> - duration: 5
> - generate_audio: false
> - seamless loop: off (reply "loop" to enable—append `seamless loop` to prompt)

**Expanded prompt:** Compose at generation time per `## Prompt expansion (mandatory)` from the user's actual brief—do not reuse fixed sample paragraphs.

**Expected outcome:** Natural cute motions, clear satisfying beats on placement, unified palette visible in the final frame—high completion, strong “neat freak” appeal.

---

## Space before/after (no character)

Single organizing moves at peak satisfaction: drawer jam-packed → zoned; suitcase chaos → Tetris-perfect pack; empty clear box → filled in one beat—show the change; no character needed.

**Prep:** Name the space (closet / fridge / vanity / toolbox / desk / luggage) and the hook (unified color sort / clear bins / drawer zones / dense pack feel).

**Flow:**
1. Confirm space type and hook direction
2. Build prompt emphasizing motion paths, color unity, final visual density
3. **Show all parameters in a table and wait for explicit user confirmation before submitting:**

   > 📋 **Ready to generate—please confirm:**
   >
   > | Field | This run | Notes |
   > |-------|----------|-------|
   > | `model` | `KLING_V3_0_PRO` | Best tier default; fast: text `VEO_3_1_FAST`, image `CHATBOT_VEO_3_1_FAST` (`duration` fixed 8); good → `KLING_V3_0_STA`; or specify a model name |
   > | `aspect_ratio` | `9:16` | Default KLING: 9:16, 1:1, 16:9 only; if you switch model, check that row’s `aspect_ratios` etc. |
   > | `duration` | `5s` | KLING family: 5 / 10 / 15; VEO fast: duration 8 only |
   > | `generate_audio` | `false` | Auto-generate audio or not |
   > | `prompt` | **Full expanded English prompt** (entire text for this run) | Revise before confirm |
   > | `seamless loop` | off | Reply "loop" to add seamless loop |
   >
   > Reply **"confirm"** to start, or list what to change.
4. After confirmation, in the terminal (`{baseDir}` = skill root):

   ```sh
   node {baseDir}/scripts/video_gen.js wait --json '{"model":"(confirmed model)","prompt":"(full English prompt)","aspect_ratio":"9:16","duration":5,"generate_audio":false}'
   ```

   `aspect_ratio`, `duration`, `generate_audio`, `model` must match the table; add `resolution` only if the model supports it. Parse `videos` from stdout.

**Expanded prompt:** Compose at generation time per `## Prompt expansion (mandatory)` from the user's actual brief—do not reuse fixed sample paragraphs.

---

## Hook keyword cheat sheet

**Chaos→order:** `chaotic transforms to pristine`, `disorder gives way to perfect symmetry`, `before-after single frame`, `satisfying before/after reveal`

**Color unity:** `rainbow color gradient sorted by hue`, `uniform palette emerges`, `color-matched arrangement`, `chromatic order from chaos`

**Clear storage:** `clear acrylic containers`, `transparent labeled storage boxes`, `stackable organizer bins`, `see-through compartments reveal contents`

**Dense pack:** `zero wasted space`, `maximum density perfect fit`, `Tetris-perfect packing`, `compression reveals hidden capacity`

> **Tip:** Rhythm matters—add `time-lapse with satisfying snap moments` or `speed ramp at key placement moments` so placements feel on-beat for edited music.
