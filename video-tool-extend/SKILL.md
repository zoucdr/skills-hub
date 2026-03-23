---
name: video-tool-extend
description: "Extend or continue an existing HTTPS video with a prompt via WeryAI (video-extend). Use when the user wants to lengthen a clip with style and duration controls, not text-to-video from scratch."
metadata: { "openclaw": { "emoji": "⏩", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
tags: [video, weryai, video-tools]
---

# WeryAI video tool — extend

This skill covers **only** **`extend`** on an **existing** video URL. It is **not** text-to-video or image-to-video generation from scratch.

**Dependencies:** `scripts/video_toolkits.js` next to this `SKILL.md`, `WERYAI_API_KEY`, Node.js 18+.

## API surface (this tool only)

- **Required:** `video_url`, `prompt`
- **Optional:** `style`, `resolution`, `duration` (see `weryai-video-toolkits` matrix)
- **Defaults:** `style=anime`, `resolution=720p`, `duration=5`

Full parameter matrix and enums: For the full matrix, open the **`weryai-video-toolkits`** skill package `references/video-tools-matrix.md`, or run `node scripts/video_toolkits.js tools` from this package.

## Pre-submit gate (mandatory)

Do **not** run `submit` / `wait` until the user explicitly confirms URLs and non-default parameters. Paid runs are not idempotent.

## Workflow

Prefer `--dry-run` to validate JSON; default to `submit` then user-driven `status` / `wait` unless they ask to block until done. Share final URLs as Markdown links `[Video](https://…)`.

## CLI (this tool)

From **this skill root**:

```sh
node scripts/video_toolkits.js wait \
  --tool extend \
  --json '{"video_url":"https://example.com/video.mp4","prompt":"Continue the motion naturally for 5 seconds"}'

node scripts/video_toolkits.js submit \
  --tool extend \
  --json '{"video_url":"https://example.com/video.mp4","prompt":"Continue the motion naturally for 5 seconds"}'

node scripts/video_toolkits.js status --task-id <task-id>
```

Dry-run:

```sh
node scripts/video_toolkits.js wait \
  --tool extend \
  --json '{"video_url":"https://example.com/video.mp4","prompt":"Continue the motion naturally for 5 seconds"}' \
  --dry-run
```

## Security

Never write `WERYAI_API_KEY` into files; only public `https://` media URLs.

## Out of scope

Other tools; pure text-to-video without a source video — use `weryai-video-generator`.

## References

- [WeryAI llms.txt](https://docs.weryai.com/llms.txt)
