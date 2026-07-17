# assets/

Drop your character cutout PNGs here. The app currently looks for:

## Required
- `character-front.png` — transparent-background cutout of the "Front View"
  pose from your reference sheet. This is the only file the app needs to stop
  showing the green placeholder box.

## Optional (for nicer head-turn scenes later)
- `character-profile.png` — side view
- `character-back.png` — back view

## Specs
- PNG with a transparent background (no beige box, no labels/text from the
  reference sheet — just the character).
- Roughly upright, centered in the frame, generous padding above the head so
  the bob/lean animation doesn't clip it.
- Any resolution works — the app scales it to ~620px tall on a 1280x720
  canvas — but taller than ~500px source height will look sharper.

## How to make the cutout (free tools)
1. Open your reference sheet in Photopea (photopea.com, free, browser-based)
   or GIMP.
2. Crop to just the "Front View" pose.
3. Use the magic wand / select-by-color on the beige background, delete it,
   export as PNG (keeps transparency).
4. Alternatively, remove.bg can strip the background automatically if the
   edges are clean.
5. Save as `character-front.png` and commit it here.

Once it's in, refresh the app — no code changes needed, `index.html` already
points at `assets/character-front.png`.
