---
name: video-tool-magic-style
description: "Apply WeryAI magic style transfer to an existing HTTPS video (video-magic). Use when the user wants preset visual restyling of a video URL, not text-to-video."
metadata: { "openclaw": { "emoji": "✨", "primaryEnv": "WERYAI_API_KEY", "paid": true, "network_required": true, "requires": { "env": ["WERYAI_API_KEY"], "bins": ["node"], "node": ">=18" } } }
tags: [video, weryai, video-tools]
---

# WeryAI video tool — magic style

This skill covers **only** **`magic-style`** on an **existing** video URL. It is **not** text-to-video or image-to-video generation from scratch.

**Dependencies:** `scripts/video_magic_style.js` (self-contained CLI), `WERYAI_API_KEY`, Node.js 18+.

## API surface (this tool only)

- **Required:** `video_url`
- **Optional:** `video_style` (`realistic_2` | `anime_style_3` | `anime_style_4`)
- **Default:** `video_style=anime_style_3`

Full parameter matrix and enums: For the full matrix, open the **`weryai-video-toolkits`** skill package `references/video-tools-matrix.md`, or run `node scripts/video_magic_style.js spec` from this package.

## Pre-submit gate (mandatory)

Do **not** run `submit` / `wait` until the user explicitly confirms URLs and non-default parameters. Paid runs are not idempotent.

## Workflow

Prefer `--dry-run` to validate JSON; default to `submit` then user-driven `status` / `wait` unless they ask to block until done. Share final URLs as Markdown links `[Video](https://…)`.

## CLI (this tool)

From **this skill root**:

```sh
node scripts/video_magic_style.js wait \
  --json '{"video_url":"https://example.com/video.mp4"}'

node scripts/video_magic_style.js submit \
  --json '{"video_url":"https://example.com/video.mp4"}'

node scripts/video_magic_style.js status --task-id <task-id>
```

Dry-run:

```sh
node scripts/video_magic_style.js wait \
  --json '{"video_url":"https://example.com/video.mp4"}' \
  --dry-run
```

## Security

Never write `WERYAI_API_KEY` into files; only public `https://` media URLs.

## Out of scope

Anime object replace — `video-tool-anime-replace`. New video from text — `weryai-video-generator`.

## References

- [WeryAI llms.txt](https://docs.weryai.com/llms.txt)
