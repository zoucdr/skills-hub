---
name: video-tool-lips-change
description: "Lip-sync an existing HTTPS video to a separate audio URL via WeryAI (video-lips-change). Use when the user wants lip sync to new audio, not text-to-video."
metadata: { "openclaw": { "emoji": "🎤", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
tags: [video, weryai, video-tools]
---

# WeryAI video tool — lips change (lip sync)

This skill covers **only** **`lips-change`** on an **existing** video URL. It is **not** text-to-video or image-to-video generation from scratch.

**Dependencies:** `scripts/video_lips_change.js` (self-contained CLI), `WERYAI_API_KEY`, Node.js 18+.

## API surface (this tool only)

- **Required:** `video_url`, `audio_url` (`https://` only)

Full parameter matrix and enums: For the full matrix, open the **`weryai-video-toolkits`** skill package `references/video-tools-matrix.md`, or run `node scripts/video_lips_change.js spec` from this package.

## Pre-submit gate (mandatory)

Do **not** run `submit` / `wait` until the user explicitly confirms URLs and non-default parameters. Paid runs are not idempotent.

## Workflow

Prefer `--dry-run` to validate JSON; default to `submit` then user-driven `status` / `wait` unless they ask to block until done. Share final URLs as Markdown links `[Video](https://…)`.

## CLI (this tool)

From **this skill root**:

```sh
node scripts/video_lips_change.js wait \
  --json '{"video_url":"https://example.com/video.mp4","audio_url":"https://example.com/audio.mp3"}'

node scripts/video_lips_change.js submit \
  --json '{"video_url":"https://example.com/video.mp4","audio_url":"https://example.com/audio.mp3"}'

node scripts/video_lips_change.js status --task-id <task-id>
```

Dry-run:

```sh
node scripts/video_lips_change.js wait \
  --json '{"video_url":"https://example.com/video.mp4","audio_url":"https://example.com/audio.mp3"}' \
  --dry-run
```

## Security

Never write `WERYAI_API_KEY` into files; only public `https://` media URLs.

## Out of scope

Face identity swap — `video-tool-face-change`. New video from prompt — `weryai-video-generator`.

## References

- [WeryAI llms.txt](https://docs.weryai.com/llms.txt)
