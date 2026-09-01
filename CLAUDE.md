# EngSOW — THE CLIMB

Pixel-art companion site for a school English reflection on **"The Woman at the
Top of the World"** by Matthew J. Kirby.

- **Live:** https://the-climb-judeliucodex.vercel.app
- **Repo:** https://github.com/judeliucodex/the-climb (public, SSH remote)
- **Vercel project:** `the-climb` / team `judeliucodex`

## Deploy rule

Push to `main` → Vercel auto-deploys. **Never deploy manually.** After a push,
confirm the deploy actually reached `READY` and compare bytes — a push proves
nothing about a deploy:

```bash
shasum -a 256 index.html
curl -s https://the-climb-judeliucodex.vercel.app | shasum -a 256
```

## How the scroll works (read before changing layout)

**Scroll maps to distance climbed, not to narrative order.** Each stage is a
*station* — a fixed place on the mountain where the altitude does not change
while you read it. Between two stations JS inserts a `.travel` spacer whose
height is `(feet gained x PX_PER_FT)`, so one pixel of scrolling is the same
number of feet everywhere on the page. The village stages gain 0 ft and get no
spacer at all; the granite wall gains 1,400 ft and takes real effort.

- Altitudes live in `data-alt` on each `.stage`. Change one and the page
  re-proportions itself — nothing else needs editing.
- `buildTravel()` creates the spacers, altitude ticks and sticky climbers.
  `buildMap()` then records every stage and spacer as a segment, and `readAt(y)`
  converts a scroll position into feet. The HUD, sky, clouds and route map are
  all driven from that one number.
- **Never use `:nth-child` on `.stage`** — the spacers sit between the stages in
  the DOM. JS adds `.flip` for the left/right alternation instead.

## Rules specific to this project

- **The PDFs are gitignored and must stay that way.** `*.pdf` covers the scanned
  story and the written reflection. Neither is ours to publish; the site quotes
  only brief attributed excerpts for commentary. Verify with `git ls-files`.
- **`index.html` is the whole site** — inline CSS and JS, no build step, no
  dependencies. Do not split it or add a bundler.
- **Pixel-grid discipline.** Art is drawn at a logical 192×108 and scaled by
  whole numbers. No `border-radius`, no blur, no smooth CSS gradients; skies use
  Bayer dithering between a locked palette.
- **No `Math.random()` or `Date.now()` in the art.** All texture comes from the
  deterministic `hsh()` hash so scenes render identically every time.
- **Content must never depend on an animation.** The reveal animates transform
  only, never opacity, so the essay stays readable if animation is throttled,
  reduced, or JS is off. With JS off the canvases are hidden and the text remains.
- Every scene must fully paint its canvas — an unpainted region shows as a black
  hole. `build()` base-fills as a safety net; check with a transparent-pixel scan.
- **Cartoon layer:** sprites get a 4-direction ink line automatically via the
  `OUTLINED` map (8-direction swallows the interior of small sprites and turns
  faces into blobs). Dialogue uses `balloon()`, narration uses `caption()`.

## Testing note

In the Claude Code browser pane the tab reports `document.hidden === true`, which
freezes rAF, CSS transitions and the animation timeline, and the pane only
composites a screenshot at scroll 0. Structural checks via `javascript_tool` are
reliable; scrolled screenshots are not. To see a scrolled section, load the page
in an iframe on a scratch page that scrolls it on `load` and clears the frozen
transitions, then screenshot that.
