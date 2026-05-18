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

## Notes
- External URLs are only mirrored for domains allowlisted in `manifest.toml` (`mirror.allowlist`).
- Non-allowlisted URLs remain as external links in chunks.
- Mirroring caps: 8 MB per page, 1 GB total.
- Directives that reference `fdl.org` or `doc-setup.org` are removed from packaged docs.
