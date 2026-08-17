# mealog-website

Marketing site for Mealog, served as a static site on GitHub Pages.

Live URL (once Pages is enabled): https://vskromny.github.io/mealog-website/

## Enabling GitHub Pages

Settings → Pages → *Build and deployment*:

- **Source:** Deploy from a branch
- **Branch:** `main` — folder `/ (root)`

The same content also lives on `develop`, so the Pages source can be switched
between the two branches without any other change.

## Layout

```
index.html                 the page itself
assets/dc-runtime.js       template runtime that renders <x-dc> markup
assets/components.js       page components (image-slot, …)
assets/react*.min.js       React 18.3.1 UMD builds, loaded locally
assets/sora-*.woff2        Sora webfont (latin + latin-ext subsets)
.nojekyll                  serve files as-is, no Jekyll build
src/mealog-website.bundle.html   original single-file export (provenance only)
```

Everything is served from this repo — the page makes no third-party requests at
runtime. `index.html` maps the runtime's CDN URLs to the local React copies via
`window.__resources`, so nothing is fetched from unpkg.

## Local preview

```sh
python3 -m http.server 8000
# then open http://127.0.0.1:8000/
```

Opening `index.html` directly over `file://` will not work — the assets are
loaded over HTTP.

## Editing

`index.html` uses `<x-dc>` templates with `{{ }}` bindings; page data (features,
steps, FAQ, App Store URL) lives in the `<script type="text/x-dc">` block at the
bottom of the file. The `<image-slot>` elements in the *See it in action*
section are placeholders — drop in real screenshots there when they are ready.
