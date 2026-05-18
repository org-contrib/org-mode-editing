---
name: org-mode-editing
description: This skill packages the Org Mode manual and guide as offline Org sources with a chunk index.
---

# org-mode-editing

This skill packages the Org Mode manual and guide as offline Org sources with a chunk index.

## Contents
- `docs/`: original Org source files copied from the repository
- `chunks/`: level-3 heading chunks for retrieval
- `index/index.json`: chunk index
- `index/sources.json`: source file metadata
- `mirror/`: allowlisted external URLs mirrored for offline use

## How to use this skill
- Search `index/index.json` by `title`, `heading_path`, and relevant terms to find likely matching chunks.
- Open the matching `chunk_path` files under `chunks/` for the focused Org content.
- Use `docs/org-manual.org` or `docs/org-guide.org` for broader surrounding context when a chunk is insufficient.
- Use `mirror/map.json` and `mirror/by-url/` only for offline copies of referenced external URLs.
- Prefer concise answers that cite the relevant Org heading or chunk path.

## Notes
- External URLs are only mirrored for allowlisted domains (orgmode.org, gnu.org, fsf.org).
- Non-allowlisted URLs remain as external links in chunks.
- Mirroring caps: 8 MB per page, 1 GB total.
- Directives that reference `fdl.org` or `doc-setup.org` are removed from packaged docs.
