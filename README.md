# mealog-website

Marketing site for Mealog, served as a static site on GitHub Pages.

Live at **https://vskromny.github.io/mealog-website/**

## Deployment

Edit on `main`. `.github/workflows/deploy-pages.yml` runs on every push to it,
mirrors the tree onto `gh-pages` (the branch Pages serves), waits until the live
site reports that commit in `build-info.json`, and fails the run if the page or
any of its assets does not come back `200`. So a green run means the change is
actually live.

`gh-pages` is generated — never commit to it by hand. `develop` carries the same
content as `main` but does not publish.

Pages turned itself on in legacy branch mode when `gh-pages` first appeared,
which is why the mirror exists. To serve `main` directly instead, go to
Settings → Pages and set the source to `main` / `/ (root)`, then delete the
mirror step (the verification steps still work as-is).

## Layout

```
index.html                 the page itself
privacy.html               privacy policy
terms.html                 terms of service
site.webmanifest           app name, theme colour and icon set
assets/dc-runtime.js       template runtime that renders <x-dc> markup
assets/react*.min.js       React 18.3.1 UMD builds, loaded locally
assets/sora-*.woff2        Sora webfont (latin + latin-ext subsets)
assets/pages.css           styles for privacy.html and terms.html
assets/logo/               brand assets (see below)
assets/screens/            app screenshots used in "See it in action"
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
bottom of the file.

## Legal pages

`privacy.html` and `terms.html` are plain HTML — no template runtime — sharing
`assets/pages.css`. They are linked from the landing page footer and from each
other, and the deploy workflow checks both return `200`.

They state as fact that meal logs stay on the device, that photos/voice go to
OpenAI through a Vercel pass-through with no retention, and that there is no
analytics. Keep them in step with what the app actually does — if the app gains
an account, sync or an SDK, both pages need updating before that ships.

## Screenshots

`assets/screens/` holds the app screenshots shown in *See it in action*, each as
WebP at 660px and 1320px wide (served via `srcset`, so retina screens get the
larger file):

| Screen     | Where it appears                        |
| ---------- | --------------------------------------- |
| `today`    | main three-up grid                      |
| `trends`   | main three-up grid                      |
| `calendar` | main three-up grid                      |
| `habits`   | secondary row below the grid            |
| `export`   | secondary row below the grid            |

To swap one out, re-export from the same 1320×2868 source frame so the
`aspect-ratio: 1320/2868` containers stay exact:

```sh
python3 -c "
from PIL import Image
im = Image.open('new-shot.png').convert('RGB')
for w in (660, 1320):
    im.resize((w, round(im.height * w / im.width)), Image.LANCZOS).save(
        f'assets/screens/today-{w}.webp', 'webp', quality=82, method=6)
"
```
