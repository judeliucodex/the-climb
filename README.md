# THE CLIMB

A pixel-art companion site for a school English reflection on **"The Woman at the
Top of the World"**, a short story by **Matthew J. Kirby**.

**Live site:** _(added after deploy)_

## What it is

The written reflection argues that no single sentence in the story carries its
meaning — that the wisdom is in the *sequence* of Able's climb. Prose struggles
to show that, so this site makes you do it: you scroll upward through thirteen
stages of the journey, from the village to the summit, and each stage surfaces
the quote the essay cites alongside the commentary on it. The final screen
assembles every quote in one view, which is the argument.

Two details in the art carry the thesis without words:

- At **Level 2-3** Able throws away the crutch his father carved for him. The
  sprite never gets it back.
- At **Level 3-2** his withered leg is redrawn healthy, and the body meter in
  the HUD snaps from 6% to full.

## Running it

It's a single self-contained `index.html`. Open it in any browser — no build
step, no dependencies, no server needed.

```bash
open index.html
```

The only external requests are two Google Fonts, and both have fallback stacks,
so it works offline too.

## How it's built

- All artwork is drawn at runtime to `<canvas>` at a logical 192×108 and scaled
  up by whole numbers, so every pixel stays square and hard-edged.
- Sprites are string pixel maps (one character per palette colour) rather than
  image files, so the whole site is one text file.
- Skies use Bayer dithering between a locked palette instead of smooth
  gradients — the thing that separates real pixel art from a blocky CSS theme.
- Nothing is random: all texture uses a deterministic hash, so the art renders
  identically every time.
- The text of every stage is plain HTML. With JavaScript disabled the artwork
  is dropped and the full essay still reads.

## Credit and use

"The Woman at the Top of the World" is a short story by Matthew J. Kirby. All
quoted phrases are his, reproduced as brief excerpts for the purpose of
commentary.

A project by Jude for English SOW.
