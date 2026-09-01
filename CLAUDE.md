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
