---
name: makeup-transform-video
version: 1.0.0
description: "Generate vertical anthropomorphic animal makeup transformation shorts (WeryAI): bare face to glam in a fast beat—everyday, date night, stage, or cosplay homage. Use when you need a makeup transformation reel, cute animal beauty glow-up, or the user asks for foundation blend, one-stroke liner, or lip-sync finish beats. SEO: makeup transform video; makeup transformation video generation."

tags: [makeup, glow-up, contrast, creative, anthropomorphic, short-video]

metadata:
  openclaw:
    emoji: "💄"
    primaryEnv: "WERYAI_API_KEY"
    paid: true
    network_required: true
    requires:
      env:
        - "WERYAI_API_KEY"
      bins:
        - "node"
      node: ">=18"
user-invocable: true
---

# Makeup transformation video generation

Bare to glam in one vertical clip—contrast and rhythm matter. Fits beauty, creative anthro, and holiday looks. Fox / cat / bunny complete the arc in about five seconds.

**Dependencies:** `WERYAI_API_KEY` + Node.js 18+. When you run the CLI, **`scripts/video_gen.js`** must exist; **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)** must exist under **`resources/`** (supply both via publish or pre-use assembly). Full commands and JSON fields: see **`resources/WERYAI_VIDEO_API.md`**. No other Cursor skills. **Default parameters** and model tiers are in the tables below; live API limits follow **weryai** models. Before installing or running, review the bundled `video_gen.js` to confirm it meets your requirements. Pay particular attention to how it handles local files if you choose to allow their usage, ensuring this behavior aligns with the skill's intended workflow for image-to-video requests.


## Prerequisites

- `WERYAI_API_KEY` **must be set** in the environment before `video_gen.js`.
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

- **Glow-up arc:** bare → steps → **full glam**; ring or beauty light; macro skin/eyes/lips when relevant.
- **Character:** anthropomorphic or human—keep application gestures readable; fast beat optional on final reveal.
- **Contrast:** cool before / warm after lighting shift sells transformation.

**`### Example prompts`** at the top of this file are **short triggers only**—always expand from the user's actual request.

## Workflow

1. Confirm the user request matches this skill's scenario (text-to-video and/or image-to-video as documented).
2. Collect the user's **brief**, optional image URL(s), tier (**best** / **good** / **fast**) or an explicit `model` key.
3. **Expand prompt (mandatory):** Unless the user supplied a finished long prompt and explicitly asked not to rewrite it, expand the brief into a full English production `prompt` using `## Prompt expansion (mandatory)` below. **Do not** call the API with only the user's minimal words.
4. Check the **expanded** `prompt` against the selected model's `prompt_length_limit` in the frozen tables in this document (when present); shorten if needed.
5. Verify `duration`, `aspect_ratio`, `resolution`, `generate_audio`, `negative_prompt`, and other fields against the frozen tables in this document and **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)**.
6. Show the pre-submit parameter table including the **full expanded `prompt`**; wait for **confirm** or edits.
7. After confirmation, run `node scripts/video_gen.js wait --json '...'` with the **expanded** prompt.
8. Parse stdout JSON and return video URLs; on failure, surface `errorCode` / `errorMessage` and suggest parameter fixes.

## CLI reference

```sh
node scripts/video_gen.js wait --json '{"model":"…","prompt":"…","duration":5,"aspect_ratio":"9:16"}'
node scripts/video_gen.js wait --json '…' --dry-run
node scripts/video_gen.js status --task-id <id>
```

**Full reference:** **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)**.

## Definition of done

Done when the user receives at least one playable video URL from the API response, or a clear failure explanation with next steps. All parameters used **must** fall within the selected model's allowed sets in this document. The submitted `prompt` **must** be the **expanded** production prompt unless the user explicitly supplied a finished long prompt and asked not to rewrite it.

## Boundaries (out of scope)

- Does not review platform compliance, copyright, or portrait rights; does not guarantee commercial usability of outputs.
- Does not provide non-WeryAI offline rendering, traditional edit timelines, or API field combinations not documented in this SKILL or **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)**.
- Do not link to `weryai-model-capabilities.md` or shared `../references/` paths; use **`resources/WERYAI_VIDEO_API.md`** for CLI/API details.
- Does not hard-code absolute paths in the skill doc; run `node scripts/...` from the skill package root (the directory containing `SKILL.md`) so `scripts/` and `resources/` paths resolve.

### Example prompts

- `Cat bare face to stage makeup: beat per step, glitter finish close-up, vertical beauty`
- `Use this bare-face image: blush and eyeliner in accelerated motion`
- `Disney-villain homage: contour step exaggerated but still cute`
- `Cute animal makeup transformation 9:16, fast beat cuts`

---

## Default parameters

| Field | Value |
|-------|-------|
| Model | KLING_V3_0_PRO |
| Aspect ratio | 9:16 (fixed) |
| Duration | 5 seconds (`duration`: 5—fast beat) |
| Audio | Off |
| Visual style | Front close-up / macro; soft beauty light; cooler natural “before,” warmer “after” with stronger highlights; quick cuts |

> **API validity (default `KLING_V3_0_PRO`):** Text-to-video `duration` only **5 / 10 / 15**; `aspect_ratio` only **9:16, 1:1, 16:9**. Image-to-video `aspect_ratio` only **9:16, 16:9, 1:1**. **No `resolution` field—do not send it.** For **fast** tier with VEO: text-to-video **`VEO_3_1_FAST`**, image-to-video **`CHATBOT_VEO_3_1_FAST`**, with `duration` **fixed at 8**, `aspect_ratio` only **9:16** or **16:9**. When switching `model_key`, follow the allowed sets in this section’s model/API constraints and the API validity note above; do not send `resolution` to models that do not support it.

---

## Bare face to polished glam

**Before you start, confirm:**
- Character (fox / cat / bunny / bear / any animal)
- Look (everyday / date / stage / holiday / character homage / exaggerated stage)
- Contrast strength (subtle lift / big gap / cartoon-level transformation)

Build a full arc: **bare → application → final close-up** with eye/cheek/lip detail and a clear vibe shift. Show parameter confirmation before submit:

**You must show all parameters in a table and wait for explicit user confirmation before submitting:**

   > 📋 **Ready to generate—please confirm:**
   >
   > | Parameter | This run | Notes |
   > |-----------|----------|-------|
   > | `model` | `KLING_V3_0_PRO` | Best default; **fast**: text `VEO_3_1_FAST`, image `CHATBOT_VEO_3_1_FAST` (`duration` 8); **good** → `KLING_V3_0_STA`; or name a model |
   > | `aspect_ratio` | `9:16` | Default KLING: 9:16, 1:1, 16:9 |
   > | `duration` | `5s` | KLING: 5 / 10 / 15; VEO fast: 8 only |
   > | `generate_audio` | `false` | Whether to auto-generate audio |
   > | `prompt` | **Full expanded English prompt** (entire text for this run) | Revise before confirm |
   > | `Loop seam` | No | Reply “loop” for seamless loop |
   >
   > Reply **“confirm”** to start, or list what to change.

**Expanded prompt:** Compose at generation time per `## Prompt expansion (mandatory)` from the user's actual brief—do not reuse fixed sample paragraphs.

**Expected outcome:** Clear step read, visible shadow and highlight, strong contrast bare vs final—good for beauty / creative edit beats.

---

## Single-step makeup hero

One move in macro—tutorial energy: product texture, hand motion, instant change—not the full glow-up arc.

**Before you start:** Name the step (base skin finish / winged liner / lip swipe / mascara layers / contour sculpt) and character preference.

Flow: collect step → prompt for gesture + material → confirm → `node scripts/video_gen.js wait --json '…'` (fields match table)

> Parameters are shown before generation; wait for confirmation.

**Expanded prompt:** Compose at generation time per `## Prompt expansion (mandatory)` from the user's actual brief—do not reuse fixed sample paragraphs.

**Parameter configuration:**

| Field | Value |
|-------|-------|
| model | KLING_V3_0_PRO |
| aspect_ratio | 9:16 |
| duration | 5 |
| generate_audio | false |

---

## Notes

> **Layering:** List 2–3 steps in order, e.g. `applies foundation → adds eyeshadow → finishes with lip`, instead of a vague “does makeup”—richer layering.

> **Contrast:** Use `bare-faced start` + `jaw-dropping transformation` + `before-after contrast` so the model treats it as a dramatic arc, not a static beauty tableau.

> **Light shift:** `cool natural window light` before, `warm ring light catches highlighter` after—temperature change sells the flip.
