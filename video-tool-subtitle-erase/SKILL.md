---
name: video-tool-subtitle-erase
description: "Remove burnt-in subtitles or on-screen text from an HTTPS video via WeryAI (video-subtitle-erase). Use when the user wants subtitle or text removal, not translation or text-to-video."
metadata: { "openclaw": { "emoji": "🧹", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
tags: [video, weryai, video-tools]
---

# WeryAI video tool — subtitle erase

This skill covers **only** **`subtitle-erase`** on an **existing** video URL. It is **not** text-to-video or image-to-video generation from scratch.

**Dependencies:** `scripts/video_subtitle_erase.js` (self-contained CLI), `WERYAI_API_KEY`, Node.js 18+.

## API surface (this tool only)

- **Required:** `video_url`
- **Optional:** `rect_vo_list` (normalized `0~1` boxes); omit for auto-detect

Full parameter matrix and enums: For the full matrix, open the **`weryai-video-toolkits`** skill package `references/video-tools-matrix.md`, or run `node scripts/video_subtitle_erase.js spec` from this package.

## Pre-submit gate (mandatory)

Do **not** run `submit` / `wait` until the user explicitly confirms URLs and non-default parameters. Paid runs are not idempotent.

## Workflow

Prefer `--dry-run` to validate JSON; default to `submit` then user-driven `status` / `wait` unless they ask to block until done. Share final URLs as Markdown links `[Video](https://…)`.

## CLI (this tool)

From **this skill root**:

```sh
node scripts/video_subtitle_erase.js wait \
  --json '{"video_url":"https://example.com/video.mp4"}'

node scripts/video_subtitle_erase.js submit \
  --json '{"video_url":"https://example.com/video.mp4"}'

node scripts/video_subtitle_erase.js status --task-id <task-id>
```

Dry-run:

```sh
node scripts/video_subtitle_erase.js wait \
  --json '{"video_url":"https://example.com/video.mp4"}' \
  --dry-run
```

## Security

Never write `WERYAI_API_KEY` into files; only public `https://` media URLs.

## Out of scope

Subtitle **translation** — `video-tool-subtitle-translate`. Watermark — `video-tool-watermark-remove`.

## References

- [WeryAI llms.txt](https://docs.weryai.com/llms.txt)
