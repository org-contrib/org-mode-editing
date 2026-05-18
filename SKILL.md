---
name: org-mode-editing
description: Offline Org Mode manual and guide sources with a chunk index. Use when answering questions about Org Mode syntax, editing workflows, TODOs, tags, dates/deadlines, agenda, capture/refile, tables, Babel/source blocks, export/publishing, markup, links, attachments, LaTeX, and other Emacs Org Mode behavior that should be grounded in the bundled Org documentation.
---

# org-mode-editing

Use this skill to answer Org Mode questions from the bundled Org manual and compact guide without needing network access.

## Bundled resources
- `docs/org-manual.org`: full Org manual source; use for authoritative detail and surrounding context.
- `docs/org-guide.org`: shorter guide; use for introductory explanations and quick workflows.
- `chunks/`: retrieval chunks split mostly at level-3 Org headings.
- `index/index.json`: list of chunks with titles, heading paths, source line ranges, and mirrored URL metadata.
- `index/sources.json`: source file checksums and generation metadata.
- `mirror/map.json` and `mirror/by-url/`: offline mirrors of allowlisted external URLs.

## Retrieval workflow
1. Search `index/index.json` first by `title`, `heading_path`, and likely Org terms.
2. Open the most relevant `chunk_path` files under `chunks/`.
3. If a chunk lacks context, open the corresponding range in `docs/org-manual.org` or `docs/org-guide.org` using `start_line` and `end_line` from the index entry.
4. Prefer `docs/org-manual.org` when the manual and guide differ in detail; use the guide for concise beginner-facing summaries.
5. For external references, consult `mirror/map.json` only when the index entry lists `mirrored_urls`; otherwise keep the original link external.

Useful local search patterns:

```bash
python - <<'PY'
import json
terms = ['agenda', 'deadline']
for item in json.load(open('index/index.json')):
    haystack = ' '.join([item['title'], item['heading_path']]).lower()
    if all(term in haystack for term in terms):
        print(item['chunk_path'], item['heading_path'], item['start_line'], item['end_line'])
PY
rg -n "deadline|agenda" docs/org-manual.org docs/org-guide.org
```

## Answering guidance
- Ground answers in the retrieved manual or guide content, and cite the exact chunk path or source heading/line range when the environment supports citations.
- Include small Org snippets for syntax questions; keep snippets minimal and valid.
- Mention version-sensitive behavior only when it appears in the bundled docs or metadata. The packaged docs are for Org `9.8-pre` as declared in `manifest.toml`.
- If the bundled docs do not cover the question, say so and ask whether to use external sources.
- Do not treat mirrored pages as newer than the bundled documentation; they are offline copies of referenced URLs.

## Index entry fields
- `chunk_path`: focused chunk to open for retrieval.
- `heading_path`: breadcrumb for the Org section.
- `source_file`, `start_line`, `end_line`: location in the source document for broader context.
- `external_urls`: URLs mentioned in the chunk.
- `mirrored_urls`: subset available in `mirror/` when mirroring succeeded.

## Mirror notes
- External URLs are mirrored only for domains allowlisted in `manifest.toml` (`mirror.allowlist`).
- Non-allowlisted URLs remain as external links in chunks.
- Mirroring caps are 8 MB per page and 1 GB total.
- In `mirror/map.json`, entries with `status: "ok"` and a `path` are available offline; `status: "error"` means unavailable offline; `status: "skipped"` means intentionally not mirrored, usually because the URL was outside the allowlist.
- Directives that reference `fdl.org` or `doc-setup.org` are removed from packaged docs.
