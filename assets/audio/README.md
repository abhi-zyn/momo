# Momo — Audio assets (optional)

The page ships with a **procedural Web Audio engine** that generates the entire
soundscape on the fly — no external files needed. If you'd like to upgrade any
sound to a real recording, here's a curated CC0 / public-domain shopping list.
Drop the downloaded file into this folder and replace the matching synth call
in `index.html` with an `<audio>` element + `play()`.

Everything below is **CC0 (public domain) or Mixkit-licensed (free for any use)**.

---

## Ambient bed

| Slot                | Suggested source | Search keyword                                    |
|---------------------|------------------|---------------------------------------------------|
| Night hollow loop   | freesound.org    | `forest night cricket loop` (filter: CC0)        |
| Pre-dawn breeze     | freesound.org    | `light forest wind ambience` (CC0)               |
| Dawn chorus loop    | freesound.org    | `dawn chorus birds field recording` (CC0)        |
| Distant owl hoot    | freesound.org    | `owl hoot distant` (CC0)                          |

Recommended specific freesound IDs (verified CC0 at time of writing — check the
license tag on the page before downloading):

- `forest night ambience`  → freesound.org/people/Mings/sounds/176615/
- `gentle wind in trees`   → freesound.org/people/InspectorJ/sounds/352081/
- `morning birds singing`  → freesound.org/people/Mings/sounds/378511/
- `single owl hoot`        → freesound.org/people/InspectorJ/sounds/200401/

## Music score

Mixkit and Pixabay both have free-license tracks that fit the mood:

- **Music box lullaby** — pixabay.com/music/lullabies-music-box-lullaby-165273/
- **Soft piano sunrise** — search `pixabay.com/music/search/soft%20piano/`
- **Studio Ghibli-style morning** — search `pixabay.com/music/search/celesta/`

Pick one ~30–60s loopable clip and save as `music.mp3` here.

## One-shot SFX

| Filename suggested  | What it is                       | Search                                |
|---------------------|----------------------------------|---------------------------------------|
| `yawn.mp3`          | Cute creature yawn               | freesound `kitten yawn` / `puppy yawn`|
| `leaf-rustle.mp3`   | Soft dry-leaf crinkle            | freesound `leaf rustle small`         |
| `footstep.mp3`      | Soft padded footstep on dirt     | mixkit `walking forest ground`        |
| `sunrise-chime.mp3` | Magical sparkle / glockenspiel   | freesound `magic sparkle chime`       |
| `bell.mp3`          | Single warm bell tone            | freesound `tibetan singing bowl short`|

## How to swap a procedural sound for a real file

In `index.html`, find the `Audio` IIFE. To override `playFootstep` with a real
file, for example, replace its body with:

```js
function playFootstep() {
  if (!stepBuffer) return;
  const src = ctx.createBufferSource();
  src.buffer = stepBuffer;
  const g = gain(0.55);
  src.connect(g).connect(sfxBus);
  src.start();
}
```

…and load the buffer once during `init()`:

```js
fetch('assets/audio/footstep.mp3')
  .then(r => r.arrayBuffer())
  .then(b => ctx.decodeAudioData(b))
  .then(buf => { stepBuffer = buf; });
```

The same pattern works for any of the SFX. For ambient/music loops, set
`source.loop = true` and route through `ambBus` or `musicBus`.
