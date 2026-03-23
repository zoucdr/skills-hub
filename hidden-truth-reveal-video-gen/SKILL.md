---
name: hidden-truth-reveal-video-gen
version: 1.0.0
description: "Create vertical reveal shorts: polished product vs harsh origin, one-line verdict, timed English captions (WeryAI). Use for supply-chain shock, contrast edits, or ‘99% don’t know’ hooks. Use when the user asks where everyday goods come from. SEO: hidden truth video; contrast reveal video."

tags: [reveal, contrast, supply-chain, consumer, hooks, subtitles, short-video, video-gen]

metadata: { "openclaw": { "emoji": "👁️", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
user-invocable: true
---

# Hidden truth reveal (polished → brutal → one line)

**Template (seconds below assume `duration` 10; rescale for 5 or 15):** **0–3s** hook: *what you use daily comes from here* (user’s **X**); **3–8s** **stark contrast** montage (sleek product ↔ labor, factory, waste, or source environment—**non-gore**, suggestive not graphic); **8–10s** **one-line summary** that lands the thesis. **English subtitles** with **explicit second windows**. Default **`duration`** is **10** when unspecified.

**Dependencies:** `WERYAI_API_KEY` + Node.js 18+. **`scripts/video_gen.js`** + **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)** under **`resources/`**. **No other Cursor skills required.**

## Prerequisites

- `WERYAI_API_KEY` **must be set**; Node **18+**. Prefer public **`https`** image URLs; if the assembled `scripts/video_gen.js` supports local file paths, review/verify the script and explicitly consent before local read-and-upload to WeryAI.
- Non-empty `model` in JSON; pick from **Recommended models** or frozen tables.
- Each `wait` consumes credits.

## Security, secrets, and API hosts

- **`WERYAI_API_KEY`**: Secret; **`requires.env`** / **`primaryEnv`**—never commit values.
- **API hosts:** `video_gen.js` uses **`https://api.weryai.com`** (tasks) and **`https://api-growth-agent.weryai.com`** (models list).
- **Local image handling disclosure**: Prefer public **`https`** image URLs. If the assembled `scripts/video_gen.js` supports local file paths, it may read a local image and upload it to WeryAI to obtain a public URL; require review / verification and explicit consent before using that path.
- **Higher assurance**: Prefer a short-lived or isolated environment when possible; review `scripts/video_gen.js` before production use. Verify whether the runtime can read local image files and upload them to WeryAI, and obtain explicit consent before using that path.

## Prompt expansion (mandatory)
**Audio (default-on):** Default **`generate_audio`: `true`** for models that support audio. Always add an **`Audio:`** subsection to the expanded **`prompt`** (whooshes, ticks, sub-bass, risers, room tone—**generic**, non-copyrighted)—**even if the user never mentioned sound**. Use **`generate_audio`: `false`** and omit **`Audio:`** only when the user explicitly wants **silent** output.

- **Duration:** Default **10**; **5** or **15** when the user asks and the model allows.
- **Timeline scaling:** Rescale **0–3 / 3–5 / 5–7 / 7–10** proportionally when **`duration` ≠ 10**; keep the **four-beat order**.

**Locked caption rhythm (example for duration 10):**

- **0.0–3.0s** — Hook naming the **daily X** (user-supplied): e.g. *YOUR DAILY X COMES FROM HERE* (keep **≤6 words** per line).
- **3.0–5.0s** — **Polished** beat label: e.g. *SHELF PERFECT*.
- **5.0–7.0s** — **Harsh** beat label: e.g. *HIDDEN COST*.
- **7.0–10.0s** — **One-line verdict** (user-themed): e.g. *THAT’S THE TRUTH* (customize to topic).

**Visuals:** Hard cuts; **high contrast** lighting; **hands / machines / environments**—avoid **graphic gore** or identifiable real people in exploitative frames. **Motion** every beat.

**Typography:** Bold sans, white + black outline, lower third, fades; **English only** on screen unless user opts out.

**Audio:** Tension bed / impact whoosh if `generate_audio` true—no named tracks.

**Negatives (Kling):** illegible captions, watermark, muddy text.

**Exception:** User supplies finished long prompt + no rewrite—still show full text in confirmation.

**Confirmation:** Full expanded `prompt` in the table.

### Niche checklist

- Contrast must read in **silent skim**—image does half the work.
- Middle beats are **two emotional temperatures**, not a lecture.

**Examples below** are targets only.

## Workflow

1. **Text** and/or **image-to-video** (multi-image only **SEEDANCE_2_0**, ≤3 URLs).
2. Collect **product or topic X**, optional reference `image`, tier.
3. **Expand** with locked windows + visuals; **translate** user Chinese brief to **English** on-screen lines unless they request another language.
4. Validate fields vs frozen tables.
5. Confirmation table + full `prompt` → user **confirm**.
6. `node scripts/video_gen.js wait --json '…'`.
7. Return **`[Video](url)`**; no code fences around links.

## CLI reference

~~~sh
node scripts/video_gen.js wait --json '{"model":"KLING_V3_0_PRO","prompt":"…","duration":10,"aspect_ratio":"9:16","generate_audio":true,"negative_prompt":"illegible text, watermark, garbled captions"}'
node scripts/video_gen.js wait --json '{"model":"SEEDANCE_2_0","prompt":"…","duration":10,"aspect_ratio":"9:16","resolution":"720p","generate_audio":true}'
~~~

**Full reference:** **[`WERYAI_VIDEO_API.md`](resources/WERYAI_VIDEO_API.md)**.

## Definition of done

**[Video](url)** or clear error. `prompt` contains **timed English** subtitle block matching the **four-beat order**, with **second ranges scaled to `duration`** (or user’s approved variant).

## Boundaries (out of scope)

- Not a journalism fact-check; **opinion / hook** tone—user responsible for claims.
- No gore; no harassment of real individuals.
- Captions only via **prompt**; no NLE export spec.
- Use **`resources/WERYAI_VIDEO_API.md`** for CLI; no `negative_prompt` on Seedance.
- **Multi-image** only **SEEDANCE_2_0**, max **3**.

### Example prompts

- `Coffee cup: shelf glamour vs harvest-labor montage, verdict line about price to growers`
- `Smartphone: polished device vs mine/e-waste suggestion, one-line kicker`
- `Image URL of sneakers → same four-window subtitle schedule`
- `Fast fashion haul vs factory floor suggestion—verdict about true cost`

---

## Model and API constraints (frozen for this skill)

### Text-to-video

| model_key | durations | aspect_ratios | resolutions | audio | negative_prompt | prompt limit |
|-----------|-----------|---------------|-------------|-------|-----------------|--------------|
| KLING_V3_0_PRO | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit `resolution`)* | yes | yes | 2000 |
| KLING_V3_0_STA | 5, 10, 15 | 9:16, 1:1, 16:9 | *(omit `resolution`)* | yes | yes | 2000 |
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
| resolution | 720p for Seedance only |
| negative_prompt | Kling: `illegible text, garbled captions, watermark` |

## Text-to-video (primary)

Collect **X** → expand **four-window** captions → confirm → `wait`.

## Image-to-video (optional)

Product still → **motion + same schedule**.

## Tips

- If claims are sensitive, soften to **“what they don’t show”** rhetoric.
- **Verdict line** should fit **one breath**.

> Packaged as **`hidden-truth-reveal-video-gen`**.
