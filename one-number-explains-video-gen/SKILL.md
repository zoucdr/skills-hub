---
name: one-number-explains-video-gen
version: 1.0.0
description: "Create vertical data-stat hooks: hero number, ramping counter or graph, closing meaning line, timed English captions (WeryAI). Use for dataviz Shorts, macro explainers, or ticker TikToks. Use when the user wants one metric to explain a trend. SEO: one number explains video; data hook video."

tags: [data, statistics, motion-graphics, hooks, subtitles, short-video, video-gen]

metadata: { "openclaw": { "emoji": "🔢", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# One number explains everything (hero digit → ramp → meaning)

**Template (assume `duration` 10 unless scaled):** **0–3s** *this number explains it all* (user names **which metric** conceptually—caption stays **short**); **3–8s** **number grows / graph ramps**—**motion must not stop** (tickers, bars, globe wireframe); **8–10s** **one conclusion line** (user-themed). **English subtitles** with **exact windows**. Default **`duration`** is **10** when unspecified.

**Dependencies:** `WERYAI_API_KEY` + Node.js 18+. **`{baseDir}/scripts/video_gen.js`** + **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)**. **No other Cursor skills required.**

## Prerequisites

- `WERYAI_API_KEY` **must be set**; Node **18+**; `https` images only.
- Non-empty `model`.

## Security, secrets, and API hosts

- **`WERYAI_API_KEY`**: Secret; never commit.
- **API hosts** fixed in `video_gen.js`.

## Prompt expansion (mandatory)

- **Duration:** Default **10**; **5** or **15** when allowed and requested.
- **Timeline scaling:** Rescale **0–3 / 3–8 / 8–10** proportionally; keep **hook → ramp → meaning** order.

**Locked rhythm (example for duration 10):**

- **0.0–3.0s** — Hook: e.g. *THIS NUMBER EXPLAINS EVERYTHING* (or embed the **name** of the metric in **≤6 words**).
- **3.0–8.0s** — **Ramp beat**—split into **two on-screen lines** if needed (**3–5.5** *WATCH IT CLIMB*, **5.5–8.0** *FASTER THAN YOU THINK*)—customize to user tone.
- **8.0–10.0s** — Meaning: e.g. *HERE’S WHAT IT MEANS* (user supplies **specific kicker** in brief—you compress to **≤6 words**).

**Visuals:** **Giant numeral**, **neon graph**, **motion blur** streaks, **particles**; **refresh chart** at least every ~1s.

**Typography:** **Huge** readable words + numeric; outline; lower third for sentences, **center** for hero digit moments if described.

**Negatives (Kling):** garbled digits, illegible captions, watermark.

**Confirmation:** Full `prompt`.

### Niche checklist

- If real stats matter to the user, they must **approve** final wording—you still **compress** for on-screen limits.
- Avoid **fake precision**—prefer **“illustrative counter”** language in prompt if not citing sources.

## Workflow

1. Text / image / multi-image (**SEEDANCE_2_0**, ≤3).
2. Collect **which metric / metaphor**.
3. Expand → validate → confirm → `wait`.
4. **`[Video](url)`**.

## CLI reference

~~~sh
node {baseDir}/scripts/video_gen.js wait --json '{"model":"KLING_V3_0_PRO","prompt":"…","duration":10,"aspect_ratio":"9:16","generate_audio":true,"negative_prompt":"illegible text, garbled numbers, watermark"}'
node {baseDir}/scripts/video_gen.js wait --json '{"model":"SEEDANCE_2_0","prompt":"…","duration":10,"aspect_ratio":"9:16","resolution":"720p","generate_audio":true}'
~~~

**Full reference:** **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)**.

## Definition of done

**[Video](url)** or error; **hook / ramp / meaning** subtitle structure explicit in `prompt` with **second ranges scaled to `duration`** (or user-approved subdivision).

## Boundaries (out of scope)

- Not a data journalism vetting pipeline.
- **`resources/WERYAI_VIDEO_API.md`** for CLI; no `negative_prompt` on Seedance.
- **Multi-image** only **SEEDANCE_2_0**, max **3**.

### Example prompts

- `Metric: compound growth—counter ramp, meaning about time`
- `Metric: attention minutes per day—punchy closer`
- `Chart image URL → animate growth with same windows`
- `Inflation or CPI vibe—counter climbs, meaning line about purchasing power`

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
| negative_prompt | Kling: `illegible text, garbled numbers, watermark` |

## Text-to-video (primary)

User gives **topic for the number** → you **craft three beats** → confirm.

## Image-to-video (optional)

Static chart → **ramp** animation + captions.

## Tips

- **Sound:** describe **tick + whoosh**—helps perceived pacing.
- **Translate** user Chinese brief to **English** on-screen lines unless specified.

> Packaged as **`one-number-explains-video-gen`**.
