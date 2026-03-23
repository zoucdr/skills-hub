---
name: video-tool-background-remove
description: "Remove or replace a video background with a solid color via WeryAI (video-background-remove). Use when the user wants background removal or a flat-color backdrop on an HTTPS video, not text-to-video."
metadata: { "openclaw": { "emoji": "🟩", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
tags: [video, weryai, video-tools]
---

# WeryAI video tool — background remove

This skill covers **only** **`background-remove`** on an **existing** video URL. It is **not** text-to-video or image-to-video generation from scratch.

**Dependencies:** `scripts/video_toolkits.js` next to this `SKILL.md`, `WERYAI_API_KEY`, Node.js 18+.

## API surface (this tool only)

- **Required:** `video_url`
- **Optional:** `background_color` (`TRANSPARENT`, `BLACK`, `WHITE`, …)
- **Default:** `background_color=BLACK`

Full parameter matrix and enums: For the full matrix, open the **`weryai-video-toolkits`** skill package `references/video-tools-matrix.md`, or run `node scripts/video_toolkits.js tools` from this package.

## Pre-submit gate (mandatory)

Do **not** run `submit` / `wait` until the user explicitly confirms URLs and non-default parameters. Paid runs are not idempotent.

## Workflow

Prefer `--dry-run` to validate JSON; default to `submit` then user-driven `status` / `wait` unless they ask to block until done. Share final URLs as Markdown links `[Video](https://…)`.

## CLI (this tool)

From **this skill root**:

```sh
node scripts/video_toolkits.js wait \
  --tool background-remove \
  --json '{"video_url":"https://example.com/video.mp4"}'

node scripts/video_toolkits.js submit \
  --tool background-remove \
  --json '{"video_url":"https://example.com/video.mp4"}'

node scripts/video_toolkits.js status --task-id <task-id>
```

Dry-run:

```sh
node scripts/video_toolkits.js wait \
  --tool background-remove \
  --json '{"video_url":"https://example.com/video.mp4"}' \
  --dry-run
```

## Security

Never write `WERYAI_API_KEY` into files; only public `https://` media URLs.

## Out of scope

Other tools; text-to-video — use `weryai-video-generator` or a `-video-gen` skill.

## References

- [WeryAI llms.txt](https://docs.weryai.com/llms.txt)
