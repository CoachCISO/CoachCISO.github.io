---
description: Publish a raw draft — add front-matter (author Simon Goldsmith), move it to _posts, relocate referenced images into assets/img, fix the links, and wire up excalidraw dark mode.
argument-hint: "[draft file in raw-posts] [--date YYYY-MM-DD]"
allowed-tools: Bash(date:*), Bash(ls:*), Bash(mkdir:*), Bash(mv:*), Bash(git mv:*), Bash(rm:*), Read, Write, Edit, Glob, Grep, AskUserQuestion
---

Publish the draft `$ARGUMENTS` from `raw-posts/` into `_posts/`.

If no draft is named, list `raw-posts/*.md`; use it if there is exactly one, otherwise ask which.
An optional `--date YYYY-MM-DD` overrides the publish date (default: today, `!date +%F`).

## What "published" looks like

Match the existing posts exactly. Front-matter template (from `_posts/2026-04-16-the-wrong-race.md`):

```yaml
---
id: <next>
title: '<Title>'
date: '<YYYY-MM-DD>T00:00:00+00:00'
author: 'Simon Goldsmith'
layout: post
excalidraw: true          # include this line ONLY if the post references a .excalidraw image
categories:
    - <one comma-separated string, e.g. "Articles, Cybersecurity, Software">
---
```

- `author` is **always** `'Simon Goldsmith'`. Never change this.
- `id` = (highest `id:` across all files in `_posts/`) + 1, zero-padded to two digits.
- Body must start with an `# <Title>` H1. If the draft already has one, keep it; if not, add one matching the front-matter title.
- Published filename: `_posts/<date>-<slug>.md`, where `<slug>` is the title lower-cased, non-alphanumerics collapsed to single hyphens, trimmed.

## Steps

1. **Read the draft.** Determine the title (an existing first-line `# H1`, else derive Title Case from the filename) and the category. **Ask the user to confirm/override the title and categories** with AskUserQuestion before writing — offer the derived title and a category guess as the recommended options. Do not invent categories silently.

2. **Find image references** in the draft body. Handle all three forms:
   - Obsidian embeds: `![[vault/path/name.ext]]` or `![[vault/path/name.ext|alt text]]`
   - Markdown images: `![alt](path)`
   - HTML: `<img src="path" ...>`

   For each, take the **basename** of the target. Skip remote URLs (`http://`, `https://`) — leave those untouched. Locate the source file by basename: look in `raw-posts/` first (`Glob raw-posts/**/<basename>`), then relative to the draft.

3. **Relocate the images.** Create `assets/img/<date>/` (`mkdir -p`). `mv` each found source file into it. Rewrite every local reference — whatever its original form — to standard markdown:

   `![<alt>](/assets/img/<date>/<basename>)`

   - Preserve any alt text from a `![[...|alt]]` embed or `![alt](...)`. If there's none, derive alt from the figure caption on the next non-empty line (an italic `*Figure N — ...*`, with the `Figure N — ` prefix stripped); otherwise fall back to the basename without extension. Excalidraw diagrams **must** become standard `![](...)` `<img>` tags — the renderer only swaps `img[src$=".excalidraw"]`, so a raw `![[...]]` embed would never render.

4. **Excalidraw / dark mode.** If any referenced image ends in `.excalidraw`, add `excalidraw: true` to the front-matter. That is the entire dark-mode requirement: `_includes/excalidraw.html` renders the diagram client-side and re-renders it with `exportWithDarkMode` whenever the theme toggles, so no per-file colour edits are needed. Without the flag the diagram silently won't render.

5. **Write & clean up.** Write the transformed post to `_posts/<date>-<slug>.md`, then `rm` the original draft from `raw-posts/`. (Move, not copy.)

## Fail loud (per CLAUDE.md Rule 12)

- If a referenced image's source file is **not found** in `raw-posts/` and is **not** already present at the destination, do not silently drop it. Rewrite the link to the expected `/assets/img/<date>/` path, but list every such missing image prominently in your summary so the user knows to supply it.
- End with a summary: published path, image folder, each image moved, each missing image flagged, and whether `excalidraw: true` was set.
