---
name: world-changing-decision-video-gen
version: 1.0.0
description: "Create vertical history-decision hooks: old choice, consequence montage, ‘still today’ closer, timed English captions (WeryAI). Use for archive-flash docs, chain-reaction Shorts, or series hooks. Use when the user asks how one past decision shaped the present. SEO: history decision hook video; consequence montage video."

tags: [history, documentary, montage, hooks, subtitles, short-video, video-gen]

metadata: { "openclaw": { "emoji": "📜", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# One decision changed the world (absurd choice → fallout → today)

**Template (assume `duration` 10 unless scaled):** **0–3s** *one old decision sounded crazy* (user’s **topic**); **3–8s** **rapid consequence flashes** (maps, crowds, documents, industry—**symbolic**, not gruesome war imagery unless user explicitly wants sober history tone and still **non-graphic**); **8–10s** *we still feel it today* (or equivalent **kicker**). **English subtitles** with **exact windows**. Default **`duration`** is **10** when unspecified.

**Dependencies:** `WERYAI_API_KEY` + Node.js 18+. **`{baseDir}/scripts/video_gen.js`** + **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)**. **No other Cursor skills required.**

## Prerequisites

- `WERYAI_API_KEY` **must be set**; Node **18+**. Prefer public **`https`** image URLs; if the assembled `scripts/video_gen.js` supports local file paths, review/verify the script and explicitly consent before local read-and-upload to WeryAI.
- Non-empty `model`.

## Security, secrets, and API hosts

- **`WERYAI_API_KEY`**: Secret; never commit in-package values.
- **API hosts** fixed in `video_gen.js`.
- **Local image handling disclosure**: Prefer public **`https`** image URLs. If the assembled `scripts/video_gen.js` supports local file paths, it may read a local image and upload it to WeryAI to obtain a public URL; require review / verification and explicit consent before using that path.
- **Higher assurance**: Prefer a short-lived or isolated environment; review `scripts/video_gen.js` before production use. Verify whether the runtime can read local image files and upload them to WeryAI, and obtain explicit consent before using that path.

## Prompt expansion (mandatory)

- **Duration:** Default **10**; **5** or **15** when allowed and requested.
- **Timeline scaling:** Rescale the four windows proportionally; preserve **hook → fallout A → fallout B → today** order.

**Locked rhythm (example for duration 10):**

- **0.0–3.0s** — Hook: e.g. *ONE ABSURD OLD DECISION…* (customize to user event).
- **3.0–5.5s** — First fallout beat label: e.g. *IMMEDIATE CHAOS*.
- **5.5–8.0s** — Second fallout: e.g. *RIPPLES EVERYWHERE*.
- **8.0–10.0s** — Closer: e.g. *WE STILL FEEL IT TODAY*.

**Visuals:** Sepia → color punches, **paper texture**, **map redraw**, **whoosh** transitions; **nonstop** motion.

**Typography:** Bold sans, outline, lower third; **English** on screen.

**Audio:** Epic but **generic** sting / rumble if audio on.

**Negatives (Kling):** illegible text, watermark, muddy overlays.

**Confirmation:** Full expanded `prompt`.

### Niche checklist

- Treat controversial history as **hook narration**, not a cited essay.
- Avoid **graphic violence**; imply scale with **crowds, maps, clocks**.

## Workflow

1. Text / image / multi-image (**SEEDANCE_2_0**, ≤3).
2. Collect **which decision or era** (user may give Chinese—translate on-screen lines to **English** unless otherwise requested).
3. Expand → validate → confirm → `wait`.
4. **`[Video](url)`** output.

## CLI reference

~~~sh
node {baseDir}/scripts/video_gen.js wait --json '{"model":"KLING_V3_0_PRO","prompt":"…","duration":10,"aspect_ratio":"9:16","generate_audio":true,"negative_prompt":"illegible text, watermark, garbled captions"}'
node {baseDir}/scripts/video_gen.js wait --json '{"model":"SEEDANCE_2_0","prompt":"…","duration":10,"aspect_ratio":"9:16","resolution":"720p","generate_audio":true}'
~~~

**Full reference:** **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)**.

## Definition of done

**[Video](url)** or error; subtitle windows match the **four-beat structure** with **ranges scaled to `duration`** (or user-approved edits).

## Boundaries (out of scope)

- Not academic sourcing; user owns historical framing.
- No glorification of atrocities; keep **symbolic** montage.
- **`resources/WERYAI_VIDEO_API.md`** for CLI; no `negative_prompt` on Seedance.
- **Multi-image** only **SEEDANCE_2_0**, max **3**.

### Example prompts

- `Treaty border line redrawn—maps and crowds, closer about modern borders`
- `Tech standard picked in the 90s—ripples to today’s gadgets`
- `Vintage document still image URL → parallax and same subtitle rhythm`
- `Standard picked decades ago—ripples into every pocket device today`

---

## Model and API constraints (frozen for this skill)

### Text-to-video

| model_key | durations | aspect_ratios | resolutions | audio | negative_prompt | prompt limit |
|-----------|-----------|---------------|-------------|-------|-----------------|--------------|
| KLING_V3_0_PRO | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit)* | yes | yes | 2000 |
| KLING_V3_0_STA | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit)* | yes | yes | 2000 |
| SEEDANCE_2_0 | 5, 10, 15 | 9:16, 1:1, 16:9 | 480p, 720p | yes | no | 2000 |

### Image-to-video

| model_key | durations | aspect_ratios | resolutions | audio | negative_prompt | image slots |
|-----------|-----------|---------------|-------------|-------|-----------------|-------------|
| KLING_V3_0_PRO | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | yes | yes | single `image` |
| KLING_V3_0_STA | 5, 10, 15 | 9:16, 16:9, 1:1 | *(omit)* | yes | yes | single `image` |
| SEEDANCE_2_0 | 5, 10, 15 | 9:16, 1:1, 16:9 | 480p, 720p | yes | no | up to **3** `images` |

## Recommended models

| Tier | model_key | Default duration |
|------|-----------|------------------|
| ⭐ Best | KLING_V3_0_PRO | 10 |
| 👍 Good | KLING_V3_0_STA | 10 |
| ⚡ Fast | SEEDANCE_2_0 | 10 (`resolution`: **720p**) |

## Default parameters

| Field | Value |
|-------|--------|
| aspect_ratio | 9:16 |
| duration | 10 |
| generate_audio | true |
| resolution | 720p (Seedance) |
| negative_prompt | Kling: `illegible text, garbled captions, watermark` |

## Text-to-video (primary)

User names **event** → you **phrase four beats** → confirm → `wait`.

## Image-to-video (optional)

Archive scan → motion + same beats.

## Tips

- **Series hook:** end on *PART 2?* only if user wants—but default closer is **today** framing.
- Keep proper nouns in captions **short** to survive video text rendering.

> Packaged as **`world-changing-decision-video-gen`**.
