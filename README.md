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
site.webmanifest           app name, theme colour and icon set
assets/dc-runtime.js       template runtime that renders <x-dc> markup
assets/components.js       page components (image-slot, …)
assets/react*.min.js       React 18.3.1 UMD builds, loaded locally
assets/sora-*.woff2        Sora webfont (latin + latin-ext subsets)
assets/logo/               brand assets (see below)
.nojekyll                  serve files as-is, no Jekyll build
src/mealog-website.bundle.html   original single-file export (provenance only)
```

## Brand assets

```
assets/logo/mealog-logo.svg        wordmark mark, green on transparent — used in the nav and footer
assets/logo/mealog-icon.svg        app icon, white mark on a green rounded square
assets/logo/favicon-32.png         32×32 PNG favicon
assets/logo/favicon-180.png        180×180 apple-touch-icon
assets/logo/mealog-logo-1024.png   1024×1024 raster mark
assets/logo/mealog-icon-1024.png   1024×1024 raster app icon — also the Open Graph/Twitter preview image
```

The Open Graph and Twitter image URLs in `index.html` are absolute (link
unfurlers do not resolve relative paths) and point at
`https://vskromny.github.io/mealog-website/`. Update them if the site moves to a
custom domain.

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
