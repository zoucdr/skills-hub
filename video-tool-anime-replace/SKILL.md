---
name: video-tool-anime-replace
description: "Replace or move an element in an existing HTTPS video with WeryAI anime-style processing (video-anime-replace). Use when the user wants anime-style object replace or move on a video URL, not text-to-video generation."
metadata: { "openclaw": { "emoji": "🎨", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
tags: [video, weryai, video-tools]
---

# WeryAI video tool — anime replace

This skill covers **only** **`anime-replace`** on an **existing** video URL. It is **not** text-to-video or image-to-video generation from scratch.

**Dependencies:** `scripts/video_toolkits.js` next to this `SKILL.md`, `WERYAI_API_KEY`, Node.js 18+.

## API surface (this tool only)

- **Required:** `video_url`, `image_url` (public `https://` URLs)
- **Optional:** `type` (`move` | `replace`), `resolution` (`480p` | `580p` | `720p`)
- **Defaults:** `type=replace`, `resolution=720p`

For the full matrix and enums, open the **`weryai-video-toolkits`** package `references/video-tools-matrix.md`, or run `node scripts/video_toolkits.js tools` from this package.

## Pre-submit gate (mandatory)

Do **not** run `submit` / `wait` until the user explicitly confirms URLs and non-default `type` / `resolution`. Paid runs are not idempotent.

## Workflow

Prefer `--dry-run` to validate JSON; default to `submit` then user-driven `status` / `wait` unless they ask to block until done. Share final URLs as Markdown links `[Video](https://…)`.

## CLI (this tool)

From **this skill root**:

```sh
node scripts/video_toolkits.js wait \
  --tool anime-replace \
  --json '{"video_url":"https://example.com/video.mp4","image_url":"https://example.com/ref.jpg"}'

node scripts/video_toolkits.js submit \
  --tool anime-replace \
  --json '{"video_url":"https://example.com/video.mp4","image_url":"https://example.com/ref.jpg","type":"replace","resolution":"720p"}'

node scripts/video_toolkits.js status --task-id <task-id>
```

Dry-run:

```sh
node scripts/video_toolkits.js wait \
  --tool anime-replace \
  --json '{"video_url":"https://example.com/video.mp4","image_url":"https://example.com/ref.jpg"}' \
  --dry-run
```

## Security

Never write `WERYAI_API_KEY` into files; only public `https://` media URLs.

## Out of scope

- Other WeryAI video tools (subtitle, upscale, extend, etc.) — use their dedicated `video-tool-*` skills or **`weryai-video-toolkits`**.
- Text-to-video / image-to-video — **`weryai-video-generator`** or a `-video-gen` skill.

## References

- [WeryAI llms.txt](https://docs.weryai.com/llms.txt)
- [Submit Video Anime Replace Task](https://docs.weryai.com/api-references/video-tools/submit-video-anime-replace-task)
