# Mikasan Narrator — Free Script-to-Video Web App

100% free, no paid APIs (no Higgsfield). Runs entirely in the browser except the
optional voiceover step, which stays a separate Coqui XTTS-v2 pass (as in your
existing `mikasan-narrator` repo).

## How it works
1. **Script → Scenes**: Groq's free LLM API (`llama-3.3-70b-versatile`) splits
   your script into scenes with narration, a motion type, a camera move, and a
   duration estimate. You paste your own free Groq key in the browser — it's
   never sent anywhere but api.groq.com.
2. **Animation**: each scene is drawn on an HTML `<canvas>` — your locked
   character image over a solid `#F5E6CC` background, with subtle procedural
   motion (bob, sway, head turn, lean, nod, shrug) and simple camera
   pan/zoom, plus a typewriter caption. This replaces per-scene AI image/video
   generation entirely, so there's no usage-based cost.
3. Each scene is captured live via `canvas.captureStream()` +
   `MediaRecorder` into a `.webm` clip, right in the browser.
4. **Voiceover**: generate narration separately with your existing Coqui
   XTTS-v2 pipeline (one file per scene), then upload each file in the app —
   it gets muxed onto the matching scene clip.
5. **Assembly**: `ffmpeg.wasm` (loaded from a public CDN, free, runs
   client-side) muxes audio, transcodes each scene to mp4, and concatenates
   them into one downloadable final video.

## Required asset
Put a **transparent-background PNG cutout** of the character at:
```
assets/character-front.png
```
Crop this from your reference sheet's "Front View" pose using any free tool
(Photopea, GIMP, remove.bg). Until this file exists, scenes render with a
green placeholder box so you can confirm timing/captions/camera work before
the real art is in.

If you later want turn/profile shots for `head_turn_left/right` scenes to
look less like a flat sprite twisting, add:
```
assets/character-profile.png
assets/character-back.png
```
and swap the drawn image based on `scene.motion` — the current version keeps
one flat sprite and fakes the turn with rotation, which matches your
"subtle, natural" requirement without needing extra assets.

## Hosting for free (GitHub Pages)
1. `index.html` lives at the repo root — already pushed.
2. Repo Settings → Pages → Deploy from branch → `main` / root (or done automatically, see below).
3. Your app is live at `https://sabaghanbarlo-hash.github.io/mikasan-webapp/`.

No backend, no server costs, no build step.

## Known limits of this MVP
- `MediaRecorder` output codec support varies slightly by browser — Chrome
  is the most reliable for `video/webm;codecs=vp9`.
- Long scripts (15+ scenes) will make the Groq call slower and increase
  browser memory use during rendering — render scenes one at a time if a
  full run feels heavy, using each scene's individual "Render this scene's
  clip" button before hitting Assemble.
- Blinking isn't implemented yet (no eye-region asset to swap) — motion
  currently relies on bob/sway/turn/lean/nod/shrug, which already satisfies
  "subtle and natural" without risking AI artifacts.

## Next steps I can help with
- Wire in blink frames once you have an eyes-closed variant of the sprite.
- Add a captions-timing sync pass so text pacing matches actual audio
  duration instead of the estimated `duration_seconds`.
- Port the Coqui step into a GitHub Actions workflow so the whole thing
  (scenes → audio → assemble) runs headless with one push, like your
  existing repo's approval-gate flow.
