# issue-evidence

Image evidence attached to GitHub issues across several private projects.

## Why this repo exists

GitHub has **no API for issue attachments** — the `user-attachments` upload is a
browser-only flow (session cookie + CSRF, not a personal access token). Verified
against the live schema: 260 GraphQL mutations, none of them attachment-related;
nothing attachment-shaped in the REST endpoint map.

That leaves exactly one automatable way to put a picture in an issue: host it at
a URL GitHub's image proxy (camo) can fetch anonymously, and reference it as
ordinary markdown. camo cannot authenticate, so the host has to be public —
hence this repo, even though the issues that cite it are private.

## Layout

```
<Project>/Issue-<NNNN>/
    <name>.png          native resolution, never downscaled
    <name>.json         provenance sidecar, when the capture tool wrote one
    _manifest.json      sha256, bytes, dimensions, published URL
```

Images are referenced **pinned to a commit SHA**, not to a branch:

```
https://raw.githubusercontent.com/LilSaints/issue-evidence/<sha>/Boxel/Issue-0371/wg-coast.png
```

A branch-relative URL would silently change meaning if the file were ever
replaced. A SHA-pinned one is the frame that was cited, permanently.

## What belongs here

Screenshots and captures that a GitHub issue makes a claim about. Nothing else —
no source, no logs, no assets, no configuration. **This repository is public.**
Anything committed is world-readable forever, including after deletion, because
git history persists in clones and caches.

Images are stored at native resolution deliberately. A downscaled whole frame
cannot support a fine-detail claim, which is usually the entire reason the image
is in the issue.
