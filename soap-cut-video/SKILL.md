---
name: soap-cut-video
version: 1.0.0
description: "Generate vertical ASMR soap and wax cutting shorts (WeryAI): text-to-video or product photo to slices, cross-section color reveal, and crumb ASMR. Use when you need soap cutting ASMR, wax slice video, satisfying cross-section, or users ask for one clean cut, rainbow layers, crunchy sound."

tags: [asmr, satisfying, handmade, short-video, video]

metadata: { "openclaw": { "emoji": "🧼", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Soap & wax cutting videos

Three hooks that stop the scroll:

- **Cross-section color:** gradient wax, layered soap, marble swirl—the cut face is prettier than the block.
- **Clean geometry:** wavy blade, grid cuts, one decisive slice—every frame is composition.
- **Sound:** dense wax drag, brittle soap crunch—ASMR completion.

From one text prompt or one product photo.

**Dependencies:** `scripts/video_gen.js` + `WERYAI_API_KEY` + Node.js 18+. No other Cursor skills.

## Prerequisites

- `WERYAI_API_KEY` **must be set** before running `video_gen.js`.
- Node.js **18+**. Prefer public **`https`** image URLs. If the assembled `scripts/video_gen.js` supports local file paths, review and verify the script first, then get **explicit consent** before it reads a local image and uploads it to WeryAI to obtain a public URL.
- Each `wait` run uses credits; repeats create new paid tasks.

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

- **Cut grammar:** blade path, **cross-section color**, lattice or layers; crumb or wax drag ASMR.
- **Macro:** top-down or side; one decisive slice or grid; material reads (soap vs wax).
- **Image-to-video:** preserve product colors; animate knife entry and separation.

**`### Example prompts`** at the top of this file are **short triggers only**—always expand from the user's actual request.

## Workflow

1. Confirm the request matches this skill (text and/or image-to-video).
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

Done when the user gets playable video URL(s) or a clear failure with next steps. Parameters **must** match the chosen model’s allowed set. The submitted `prompt` **must** be the **expanded** production prompt unless the user explicitly supplied a finished long prompt and asked not to rewrite it.

## Boundaries (out of scope)

- No compliance/copyright/likeness review; no commercial warranty.
- No offline NLE or undocumented API fields.
- No hard-coded absolute paths; `{baseDir}` = this skill root.

### Example prompts

- `Vertical ASMR: rainbow layered soap, blade down, cross-section color pop`
- `This photo is wax—wavy blade multi-cut, powder falling`
- `Satisfying square soap one-cut-through, sound must be crunchy`
- `Soap cutting satisfying loop 9:16, cross-section color reveal, ASMR crunch`

---

## Default parameters

| Field | Value |
|-------|--------|
| Aspect | 9:16 (fixed) |
| Duration | 5 s |
| Audio | On (cutting ASMR is core) |
| Look | Macro top-down, cold white seamless bg, slow slices, sharp cross-section |
| Loop | Off by default; append loop keywords in prompt if user wants seamless loop |

---

## Text-to-video: cutting

Describe material and blade style; build prompt and run. Good for batch look tests.

Explain object (color soap / gradient wax / layered bar / marble wax) and technique (wavy knife / grid / diagonal single cut / fine strips). Emphasize cross-section reveal, powder arcs, blade pressure deformation.

**Before run—confirm with user:**

> About to generate with: model (you or user pick), aspect_ratio 9:16, duration 5, generate_audio true, loop off (reply “loop” to add seamless loop keywords).

**Parameters:**

| Field | Value |
|-------|--------|
| aspect_ratio | 9:16 |
| duration | 5 |
| generate_audio | true |

**Sample prompts** (already English in body—reuse style):

> Extreme close-up macro shot, top-down overhead angle, a chunky pastel gradient wax block layered in soft pink-lilac-sky-blue sitting on a cold white marble slab, a wavy-edge palette knife presses slowly downward in 120fps slow motion, the blade splits the wax revealing a breathtaking swirled cross-section of pastel gradient colors, thin wavy shavings curl and tumble away from the perfect cut edge, cold diffused studio light makes the wax surface slightly translucent and glowing, fine wax dust floats in the air backlit by the white light, ASMR dense satisfying crunch-and-squeak of wavy blade through dense wax, minimal white seamless background, no distractions

**Expected result:** Cross-section and color read clearly in slow motion; sound matches motion; hook in the first 1–2 s of the push.

---

## Image-to-video: cut motion on photo

Public `https` image + desired cut style (single / multi / wavy / strips). Match colors and texture from the photo.

**Confirm before run:** same as above + `image` URL.

**Parameters:** add `image` row with URL.

**Sample prompt:**
> Close-up macro shot matching the angle of the provided image, a sharp knife blade enters from the top and presses through the soap or wax block in a single slow deliberate cut at 120fps, slicing cleanly from one edge to the other, the two halves slowly separate revealing the internal color layers and texture of the block matching the original object, fine powder particles float upward backlit by cold white studio light, the cut surface catches a clean highlight along the freshly exposed edge, deep satisfying ASMR crunch of blade through dense material, white seamless studio background

**Expected result:** Motion consistent with the photo; cross-section matches material; sound + slow motion amplify satisfaction.

---

## Tips

**Cross-section:** `the cut face reveals`, `cross-section exposed`, `interior color layers emerge`, `split apart revealing`

**Texture:** wax `dense satisfying crunch`; soap `crumbly chalky squeak`; hard wax `deep low thud`

**Debris:** `fine dust floats in backlit air`, `thin shavings curl away`, `micro-particles scatter`

**Loop:** append `seamless loop, first and last frame identical, perfectly looping`

**FAQ:** URLs must be HTTPS public. For color pop, state background (dark vs white). For grid cuts, state slice count (e.g. `6 successive slices`). If API 1002 on `aspect_ratio`, retry without it for models that omit the field.

> **Note:** Not every model supports `aspect_ratio`; on 1002, drop it and retry.
