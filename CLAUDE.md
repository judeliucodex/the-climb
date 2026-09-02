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

**The reader scrolls UP to climb, and scroll maps to distance.** `main` is
`flex-direction: column-reverse`, so the title sits at the bottom of the
document and the results/footer at the top. The DOM keeps its natural order
(s0 first) so screen readers and the no-JS view still read the essay forwards;
only the visual flow is reversed. On load the page jumps to the bottom
(`toBase()`), and it re-anchors after web fonts settle unless the reader has
already scrolled.

Each stage is a *station* — a fixed place on the mountain where the altitude
does not change while you read it. Between two stations JS inserts a `.travel`
spacer whose height is `(feet gained x PX_PER_FT)`, so one pixel of scrolling is
the same number of feet everywhere on the page. The village stages gain 0 ft and
get no spacer at all; the granite wall gains 1,400 ft and takes real effort.

Because the page is reversed, **a spacer's TOP edge touches the higher station**.
Segments store `aTop`/`aBot` rather than start/end, altitude ticks are placed at
`(a1 - ft) * PPF` from the spacer top, and `buildMap()` sorts segments by their
rendered `offsetTop`.

- Altitudes live in `data-alt` on each `.stage`. Change one and the page
  re-proportions itself — nothing else needs editing.
- `buildTravel()` creates the spacers, altitude ticks and sticky climbers.
  `buildMap()` then records every stage and spacer as a segment, and `readAt(y)`
  converts a scroll position into feet. The HUD, sky, clouds and route map are
  all driven from that one number.
- **Never use `:nth-child` on `.stage`** — the spacers sit between the stages in
  the DOM. JS adds `.flip` for the left/right alternation instead.
- `yForAlt()` is the inverse lookup: on resize/rotate the page restores the
  reader's **altitude**, not their pixel offset, so rotating an iPad keeps place.
- The HUD is deliberately minimal: one number, one bar, one chapter name. It is
  a floating sidebar above 1280px and a slim top bar below that — under 1280px
  the centred content is wide enough to run beneath a sidebar.
- Sprite art is height-capped as well as width-capped (`byHeight` in
  `sizeScenes`), or a landscape phone makes the title screen three screens tall.
  The TV gets a tighter budget than the story panels so the title fits one view.
- Scanlines (the faint CRT lines, `#crt`) are always on — permanent art
  direction, not a setting. There is no toggle and no `localStorage` for it.

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
- Able climbs on a 4-frame cycle (`climbA` reach-left, `climbB` pull, `climbC`
  reach-right, `climbB`) via `climbFrame()`/`climbLift()`, used both by the
  sticky climbers in the travel spacers and by the granite-wall scene.

## Testing note

In the Claude Code browser pane the tab reports `document.hidden === true`, which
freezes rAF, CSS transitions and the animation timeline, and the pane only
composites a screenshot at scroll 0. Structural checks via `javascript_tool` are
reliable; scrolled screenshots are not. To see a scrolled section, load the page
in an iframe on a scratch page that scrolls it on `load` and clears the frozen
transitions, then screenshot that.
