---
name: mini-kitchen-video
version: 1.0.0
description: "Generate vertical miniature kitchen cooking shorts (WeryAI): text or mini cookware/ingredient image to motion on a tiny stove. Use when you need miniature cooking ASMR, tiny-kitchen clip, or the user asks for thumb-sized woks, mini egg fry, cozy food-toy vibes. SEO: mini kitchen video; miniature kitchen cooking video generation."

tags: [mini-kitchen, food, healing, handmade, curiosity, short-video, video]

metadata:
  openclaw:
    emoji: "🍳"
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

# Miniature kitchen cooking video generation

Three reasons these finish well:

- **Scale contrast:** Real flame, real oil sparkle—at fingertip size
- **Smaller = sharper:** Mini pan shimmer, mini hotpot bubbles, mini burger cheese pull—every macro frame can poster
- **Cozy ASMR:** Sizzle + boil + chop + warm light + slow motion—often holds attention better than pure VFX

Text or one cookware / ingredient image → clip.

**Dependencies:** `WERYAI_API_KEY` + Node.js 18+. When you run the CLI, **`scripts/video_gen.js`** must exist; **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)** must exist under **`resources/`** (supply both via publish or pre-use assembly). Full commands and JSON fields: see **`resources/WERYAI_VIDEO_API.md`**. No other Cursor skills. **Default parameters** and model tiers are in the tables below; live API limits follow **weryai** models. Before installing or running, review the bundled `video_gen.js` to confirm it meets your requirements. Pay particular attention to how it handles local files if you choose to allow their usage, ensuring this behavior aligns with the skill's intended workflow for image-to-video requests.


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

- **Toy scale:** fingertip/palm/palm-sized props; **real heat physics**—sizzle, bubble, color change, steam.
- **Camera:** extreme macro, top-down or ~45°; **slow motion** for food detail; ASMR if audio on.
- **Image-to-video:** match reference lighting; animate flame, boil, or stir without breaking scale gag.

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

- `Mini cast-iron pan, micro sunny-side egg, oil beads and yolk wobble, vertical food-toy look`
- `Use this tiny stove scene: stir-fry mini vegetables motion`
- `ASMR chop mini ingredients then into the pan, close hands throughout`
- `Miniature kitchen cooking 9:16, tiny realistic steam and sizzle`

---

## Default parameters

| Field | Value |
|-------|-------|
| Model | KLING_V3_0_PRO (strong material read; supports audio) |
| Aspect ratio | 9:16 (fixed vertical) |
| Duration | 5s (short, peak-focused) |
| Style | Extreme macro, warm natural light, top-down or ~45°, slow cooking detail, crafted handmade feel |
| Audio | On (sizzle / boil core satisfaction) |
| Loop seam | No (default; when on, append loop phrases to prompt) |

> **API validity (default `KLING_V3_0_PRO`):** Text-to-video `duration` only **5 / 10 / 15**; `aspect_ratio` only **9:16, 1:1, 16:9**. Image-to-video `aspect_ratio` only **9:16, 16:9, 1:1**. **No `resolution` field—do not send it.** For **fast** tier with VEO: text-to-video **`VEO_3_1_FAST`**, image-to-video **`CHATBOT_VEO_3_1_FAST`**, with `duration` **fixed at 8**, `aspect_ratio` only **9:16** or **16:9**. When switching `model_key`, follow the allowed sets in this section’s model/API constraints and the API validity note above; do not send `resolution` to models that do not support it.

---

## Text-to-video

Say the dish, mini cookware, and the cooking beat (fry / stir / hotpot / plate); **expand** into a full English prompt per `## Prompt expansion (mandatory)` before submit. Good for rapid look tests.

More specific = better detail: mini cast-iron single egg, palm hotpot with mini noodles, thumbnail burger patty flipping on a micro grill.

**Flow:**

1. Collect dish + motion preference.
2. Prompt stresses: real heat response in tiny vessels (oil, bubbles, color change), prop craft, slow-motion rhythm.
3. If unspecified, show and wait:

   > Generating with the parameters below—reply **confirm** or say what to change:
   > - model: KLING_V3_0_PRO
   > - aspect_ratio: 9:16
   > - duration: 5s
   > - generate_audio: true
   > - Loop seam: No (reply **loop** to append `seamless loop` etc. to prompt)

4. After confirmation, run `node scripts/video_gen.js wait --json '…'` (text-to-video, no `image`); match fields; parse stdout.

**Parameter configuration:**

| Field | Value |
|-------|-------|
| aspect_ratio | 9:16 |
| duration | 5 |
| generate_audio | true |

**Expanded prompt:** Compose at generation time per `## Prompt expansion (mandatory)` from the user's actual brief—do not reuse fixed sample paragraphs.

**Expected outcome:** Macro captures believable heat physics at toy scale, warm cozy grade, audio matches motion—strong vertical completion.

---

## Mini cookware image → cooking motion

Public **HTTPS** photo of mini gear / food / plating → motion that matches. Reuse product or unboxing stills.

**Flow:**

1. Image URL + desired motion (start stir / ingredient in / lift plate / broth boil).
2. Match palette and lighting of the reference.
3. Show and wait:

   > Generating with the parameters below—reply **confirm** or say what to change:
   > - model: KLING_V3_0_PRO
   > - aspect_ratio: 9:16
   > - duration: 5s
   > - generate_audio: true
   > - image: (your URL)
   > - Loop seam: No (reply **loop** to enable)

4. Run `node scripts/video_gen.js wait --json '…'` with `image`; parse stdout.

**Parameter configuration:**

| Field | Value |
|-------|-------|
| aspect_ratio | 9:16 |
| duration | 5 |
| generate_audio | true |
| image | User-supplied image URL |

**Expanded prompt:** Compose at generation time per `## Prompt expansion (mandatory)` from the user's actual brief—do not reuse fixed sample paragraphs.

**Expected outcome:** Props from the image move naturally; cooking matches ingredients; color consistent; scale gag preserved.

---

## Tips

### Prompt boosts

- **Scale:** `fits entirely within a fingertip`, `no bigger than a thumbnail`, `palm-sized`—helps proportion.
- **Heat physics:** `egg white sets at the edges`, `cheese stretches in a thin glossy pull`, `meat curls immediately` beats generic “cooking.”
- **Slow motion:** `120fps slow motion` or `60fps slow motion` for clearer food detail—often helps retention.
- **Loop:** append `seamless loop, first and last frame identical, perfectly looping` when loop is on.

### FAQ

- Prefer a public **`https`** image URL. If the assembled `scripts/video_gen.js` supports local file paths, review/verify the script and explicitly consent before local read-and-upload to WeryAI.
- Multi-item mini bento / dessert: describe each item’s size and placement so nothing gets dropped.
- For polish, name the surface (white marble, wood board, dark slate) instead of “nice background.”

> **Note:** Not every model accepts `aspect_ratio`; if the API errors on it, try omitting and retry.
