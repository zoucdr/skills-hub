# WeryAI video CLI (`video_gen.js`)

From the skill package root: **`node scripts/video_gen.js`**. **Node.js 18+**. Per-skill rules (models, prompts, confirmation) stay in each **`SKILL.md`**.

## Commands

| Command | Purpose |
|---------|---------|
| `models` | **Live model registry** (GET same endpoint as below). Prints JSON **to stdout only** — this CLI **does not** write arbitrary files. Optional `--mode text_to_video` \| `image_to_video` \| `multi_image_to_video` (omit for all three buckets). Response includes **`collectedAt`** (ISO timestamp) when successful. **Assembled child skills** should use this to refresh stale **`## Model and API constraints`** in **`SKILL.md`** after platform changes (see **Refreshing model metadata** below). |
| `submit-text` / `submit-image` / `submit-multi-image` | Submit one job (async); routing follows JSON only (see below) |
| `wait` | Same JSON as submit, then **poll until done** |
| `status` | One-shot status: `--task-id <id>` (or `task_id` / `taskId` in `--json`) |

`submit-text`, `submit-image`, and `submit-multi-image` are **aliases** — mode is chosen from `image` / `images[]`, not from the verb.

## Generation mode (from JSON)

| Mode | Condition | API route (under `https://api.weryai.com`) |
|------|-----------|--------------------------------|
| Text → video | No `image`, no non-empty `images[]` | `POST /v1/generation/text-to-video` |
| Image → video | Non-empty `image` string | `POST /v1/generation/image-to-video` |
| Multi-image | Non-empty `images` array | `POST /v1/generation/multi-image-to-video` |

- **`model`**: required for real runs and for `--dry-run` when `prompt` is present. Missing/empty → `MISSING_PARAM` (no default model).
- **`prompt`**: required for `wait` and all `submit-*`.
- **`duration`**: number; script defaults to **5** if missing/invalid — must still be allowed for the chosen model.
- **`aspect_ratio`**, **`resolution`**, **`generate_audio`**, **`negative_prompt`**: optional; only if the model supports them (`models` output / skill table).
- **`images`**: at most **3** entries are sent after preparation (`slice(0, 3)`).

## Images: `https` vs local path

- **Preferred:** public **`https://`** URL in `image` or each `images[]` entry — sent straight to the video API.
- **Not allowed as-is:** `http://` image URLs — script errors.
- **Local path / `file://`:** script reads the file, **POST** multipart to  
  `https://api-growth-agent.weryai.com/growthai/v1/generation/upload-file`  
  (`batch_no`, `fixed`, `file`), parses response for a public **`https`** URL, then substitutes it into the video request. Extensions: **`.jpg` / `.jpeg` / `.png` / `.webp` / `.gif`**.  
- **`--dry-run`:** no upload; response may still list local paths with a note.

Skills should default to **public `https`**; local upload only after reviewing this script and **explicit user consent**.

## Refreshing model metadata (`models`) — assembled child skills

Each WeryAI **child skill** ships a **frozen** **`## Model and API constraints (frozen for this skill)`** table in **`SKILL.md`**. The platform may change **`durations`**, **`aspect_ratios`**, **`resolutions`**, audio flags, or add/remove **`model_key`** entries over time. To avoid outdated limits:

1. From the **skill package root** (where **`scripts/video_gen.js`** lives), set **`WERYAI_API_KEY`** and run:
   - Full registry: `node scripts/video_gen.js models`
   - One channel: `node scripts/video_gen.js models --mode text_to_video` (or `image_to_video` / `multi_image_to_video`)
2. Read the **stdout** JSON. Each listed **`model_key`** includes fields such as **`durations`**, **`aspect_ratios`**, **`resolutions`**, **`has_generate_audio`**, **`has_negative_prompt`**, **`prompt_length_limit`** (exact keys depend on API version).
3. **Update** **`SKILL.md`**: **`## Model and API constraints`**, **`## Recommended models`**, **`## Default parameters`**, and any workflow text so they match the live row for every **`model_key`** this skill documents. Note the refresh date in the frozen-section footnote (e.g. “Re-run `models` after platform upgrades”).
4. **Do not** paste the full registry into **`SKILL.md`**—only the rows this skill actually uses.

**Creator repo only:** the **`weryai-video-skill-direct-creator`** package also ships **`weryai-model-capabilities-collect.js`** for maintainer workflows (`--markdown`, **`--patch-md`** on internal `weryai-model-capabilities.md`). **Assembled child skill packages** normally have only **`video_gen.js`**; use **`models`** (stdout only) there.

## Environment & hosts

- **`WERYAI_API_KEY`**: required for `models`, generation, `status`, and local-image upload — not for `--dry-run` alone. No other env vars are read.

| Use | Base / path |
|-----|-------------|
| Video + task status | `https://api.weryai.com` |
| Model list + upload | `https://api-growth-agent.weryai.com` — paths `/growthai/v1/video/models`, `/growthai/v1/generation/upload-file` |

## `wait` polling (built-in)

After submit, **`wait`** polls **`GET /v1/generation/{taskId}/status`** every **6 seconds** until the task is **completed** or **failed**, or until **600s** total — **fixed interval**, not exponential backoff. External wrappers that call `status` themselves may use any backoff they want.

Per-HTTP `fetch` timeout: **30s** (generation/upload); upload multipart **60s**.

## Examples

```sh
node scripts/video_gen.js models
node scripts/video_gen.js models --mode text_to_video
node scripts/video_gen.js submit-text  --json '{"model":"KLING_V3_0_PRO","prompt":"…","duration":10,"aspect_ratio":"9:16"}'
node scripts/video_gen.js status --task-id "<TASK_ID>"
node scripts/video_gen.js wait --json '{"model":"…","prompt":"…","image":"https://…","duration":5}'
node scripts/video_gen.js wait --json '{"model":"…","prompt":"…","image":"./shot.png","duration":5}'
node scripts/video_gen.js wait --json '…' --dry-run
```

## JSON fields (generation)

| Field | Notes |
|-------|--------|
| `model` | Required (non-empty). |
| `prompt` | Required. |
| `duration` | Number; default 5 in script if absent/invalid. |
| `aspect_ratio`, `resolution`, `generate_audio`, `negative_prompt` | Optional, model-dependent. |
| `image` | Single image-to-video; `https` or local path (upload). |
| `images` | Multi-image; same rules per element; max 3 forwarded. |

## Success / errors (stdout JSON)

- **OK:** HTTP 2xx and business `status` **0** or **200** (this script treats both as success for that hop).
- **Failure:** `ok: false`, `phase: 'failed'`, `errorCode`, `errorMessage`, often `errorTitle`, `retryable`, optional `hint`, `field`.
- **Submitted:** `phase: 'submitted'`, `taskId`, `taskIds`, optional `batchId`.
- **Completed:** `phase: 'completed'`, `videos`: `[{ url, cover_url }, …]`.

There is **no** `batch-status` subcommand; poll each `taskId` with `status` or use `wait`.

## Agent checklist (async flow)

1. After **`submit-*`**, tell the user **`taskId`** (and `batchId` / `taskIds` if present).
2. Either **`wait`** (blocking, fixed 6s polls inside script) or loop **`status`** with your own spacing.
3. On success, show playable URLs as normal Markdown links **`[label](url)`** (not code-fenced). On failure, surface **`errorCode`** / **`errorMessage`**.
4. When the user or maintainer suspects **stale model limits**, run **`models`** from the skill root and use the **stdout** JSON to align **`SKILL.md`** frozen tables with the live registry—see **Refreshing model metadata**.

**Reminder:** Skills usually require explicit user confirmation before any paid `submit-*` / `wait`. Follow the active **`SKILL.md`**.
