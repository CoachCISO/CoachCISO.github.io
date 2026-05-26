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

3. **Relocate the images.** Create `assets/img/<date>/` (`mkdir -p`). `mv` each found source file into it. Determine the alt text for each reference: preserve any alt from a `![[...|alt]]` embed or `![alt](...)`; if there's none, derive it from the figure caption on the next non-empty line (an italic `*Figure N — ...*`, with the `Figure N — ` prefix stripped); otherwise fall back to the basename without extension. Then rewrite each local reference by type:

   - **Excalidraw** (`.excalidraw`) → a placeholder div, on its own line with a blank line after it:

     `<div data-excalidraw="/assets/img/<date>/<basename>" data-alt="<alt>"></div>`

     Do **not** use markdown image syntax for excalidraw. Chirpy's `refactor-content.html` rewrites every `<img>` (src→data-src for lazysizes, plus a lightbox wrapper), which makes the client-side renderer race the lazy-loader and render unpredictably. A `<div data-excalidraw>` is invisible to that pipeline; `_includes/excalidraw.html` selects on `[data-excalidraw]`.

   - **All other images** (png/jpg/svg/gif…) → standard markdown: `![<alt>](/assets/img/<date>/<basename>)`. These are meant to go through Chirpy's normal image handling.

4. **LinkedIn article link.** If the draft body links to a LinkedIn article (typically a closing line like `This article was published on [LinkedIn](https://www.linkedin.com/...)`), prepend a Font Awesome LinkedIn icon — linked to that same article — so the line reads as a LinkedIn-badged callout. Use raw HTML so it opens in a new tab:

   `<a href="<linkedin-url>" target="_blank" rel="noopener" aria-label="Read this article on LinkedIn"><i class="fab fa-linkedin"></i></a> <original line text>`

   The site does **not** render Chirpy's generic "share on LinkedIn" button — it was removed from `_data/share.yml` in favour of this direct article link (the copy-link icon stays). Skip this step if the draft has no LinkedIn article link.

5. **Excalidraw / dark mode.** If any referenced image ends in `.excalidraw`, add `excalidraw: true` to the front-matter. That is the entire dark-mode requirement: `_includes/excalidraw.html` renders the diagram client-side and re-renders it with `exportWithDarkMode` whenever the theme toggles, so no per-file colour edits are needed. Without the flag the diagram silently won't render.

6. **Write & clean up.** Write the transformed post to `_posts/<date>-<slug>.md`, then `rm` the original draft from `raw-posts/`. (Move, not copy.)

## Fail loud (per CLAUDE.md Rule 12)

- If a referenced image's source file is **not found** in `raw-posts/` and is **not** already present at the destination, do not silently drop it. Rewrite the link to the expected `/assets/img/<date>/` path, but list every such missing image prominently in your summary so the user knows to supply it.
- End with a summary: published path, image folder, each image moved, each missing image flagged, and whether `excalidraw: true` was set.
