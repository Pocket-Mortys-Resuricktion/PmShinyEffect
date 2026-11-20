# Shiny Maker

Recreate the **Pocket Mortys shiny effect** on any sprite: the rainbow hue shift, the diagonal shine, the pulsing glow and the overworld sparkles. The defaults match the game frame for frame. Every setting can be changed, and the effects can be stacked.

Shiny Maker is a single HTML file. It runs as a **Photopea plugin**, or on its own in any browser, so you can use the exports in GIMP, Aseprite, Krita, Photoshop or anything else.

---

## Features

- **Game-accurate defaults**: rebuilt from the game's shader and particle settings, with timing measured from recordings of the game.
- **Four effects**: shiny colours, shine, pulsing and sparkles. Each can be switched on or off and stacked with the others.
- **Animated images**: GIF, APNG and animated WebP play normally with the effects on top, and the result loops cleanly.
- **Custom sparkles**: upload your own sparkle images and the tool picks between them at random.
- **Randomiser**: rolls new looks while staying within limits you set.
- **Exports**: GIF, APNG (full transparency), sprite sheet, a ZIP of PNG frames, or a single frame, with automatic trimming.
- **Presets**: built-in game presets, plus your own, saved in the browser or as `.json` files.
- **Self-contained**: no install, no server and no internet connection needed once the page is open.

---

## Files

| File | What it is |
|---|---|
| `shiny-plugin.html` | The whole tool, in one file. The textures and the GIF encoder are built in. |
| `shiny-plugin.json` | The Photopea plugin manifest. |
| `shiny.py` | A command-line batch tool for the shiny colours and shine. Its output matches the plugin to within 1/255. |

---

## Getting started

### In a browser

Open `shiny-plugin.html` in a browser. Then drag in a sprite, paste one, or click **Open image…**.

### As a Photopea plugin

1. Host `shiny-plugin.html` at an HTTPS URL. GitHub Pages works: push the repo, enable Pages, and the file will be at `https://<user>.github.io/<repo>/shiny-plugin.html`.
2. Edit `shiny-plugin.json` and set `url` to that address. Set `icon` to an image URL as well.
3. Add the plugin through Photopea's Plugins panel. See [Photopea's plugin documentation](https://www.photopea.com/api/plugins) for the current steps.
4. In the plugin, use:
   - **Get from Photopea** to import the visible layers of the active document as a still image.
   - **Get animation** to import an animated document as a GIF.
   - **GIF → Photopea** to send the result back as a new document, with one layer per frame.

> Tip: hide any layers you don't want before using **Get from Photopea**. Photopea sends only what's visible.

---

## Usage

1. **Load the normal (non-shiny) sprite.** A transparent background works best. On a solid background, the effects cover the background too.
2. **Pick a preset** or keep the default (battle shiny).
3. **Adjust the effects.** Each section has an on/off switch and a 🎲 button that randomises just that section. Settings marked **•** differ from the game.
4. **Preview.** Press Space to play or pause, and drag the timeline to scrub. Hold **Hold: original** to compare with the original sprite.
5. **Export** from the panel at the top.

---

## Effects

| Effect | What the game does | Measured from footage |
|---|---|---|
| **Shiny colours** | A hue rotation around the grey axis (AllIn1SpriteShader `HSV_ON`). This is not a plain HSV hue shift. | 360° every 2.2 s, linear |
| **Shine** | A diagonal band added on top (`SHINE_ON`): half-width 0.084, linear falloff, white | Bottom-right to top-left. It starts 0.18 s into the cycle and takes 1.25 s. |
| **Pulsing** | A green-tinted copy of the sprite with a glow texture and an outline (`PulsingMortyMat`), faded in and out over the sprite | Linear ping-pong, about every 2.0 s |
| **Sparkles** | Overworld particles: one at a time, a new one every second, random rotation, cream to white | All spawn about 27% up the body, within ±30% sideways. Size is 28–62% of body height and varies per sparkle. Some drift up by up to 6% of body height per second; others stay still. |

In the game, shiny and pulsing never appear together, and sparkles only appear in the overworld. The game presets keep them separate. The combo presets and the section switches let you stack them.

### Customisation highlights

- **Shiny colours:**
  - Spin, ping-pong between two hues, or a fixed shift.
  - The game's colour maths, or a plain HSV rotation.
  - Saturation and brightness.
- **Shine:**
  - A drag-to-set direction dial.
  - Width, glow, falloff shape and blend mode.
  - Several bands, and several shines per loop.
  - Solid colour or a gradient: along the band, across it, or over the sweep.
  - **Continuous** and **Fit inside loop** buttons, and an option to always finish before the loop ends.
- **Pulsing:**
  - Wave shape, strength, phase, tint, glow and outline.
  - Have the shiny hue shift the pulse glow too.
  - Put the shine above or below the pulse.
- **Sparkles:**
  - Game placement, or randomised placement within a set distance of the body.
  - Size, rate, lifetime and how many can show at once.
  - Rise speed, spin, size over life (steady, pop, twinkle), fades and colour gradient.
  - Optionally apply the shiny colours, shine or pulse to the sparkles.
- **Colour picker:** swatches, a hex box, and a colour square with OK and Cancel.

### Custom sparkles

Under **Sparkles → Sparkle images**, add as many images as you like, or drop them on that box.

- **Picking:** each sparkle picks one image at random. The number on each tile is its weight, so 2 comes up twice as often as 1.
- **Game sparkle:** it stays in the mix unless you untick it.
- **Size:** large images are scaled down to 256 px on their longest side and keep their aspect ratio.
- **Colours:** your images keep their own colours unless **Tint my images** is on.
- **Saved:** the set is remembered in your browser.

---

## Animated images

Load a GIF, APNG or animated WebP, or use **Get animation** in Photopea, and an **Animation** section appears.

- **The original animation is preserved.** It keeps the same first and last frame, and every frame is held for its original time.
- **The effects follow each frame.** The shine, hue shift and pulse outline follow each frame's shape.
- **Keep in sync by:**
  - **Fitting the shiny effect to the animation** (default). The animation is untouched, and the shiny speed changes by no more than **Max shiny speed change**. Short animations repeat until a whole number of shiny cycles fits.
  - **Repeating the animation.** The shiny effect runs at exact game speed.
  - **Changing the animation's speed.** The shiny effect stays at game speed. The animation repeats as needed and changes speed by up to about 10%.
  - **Manual.** You set the number of shiny cycles and animation repeats yourself.
- **Tweaks:**
  - Animation speed and playback direction (forward, reverse or ping-pong).
  - Start frame and shiny offset.
  - Whether sparkles differ in each shiny cycle.
  - Output frames: **smooth** adds in-between frames so the shine moves smoothly; **original only** gives a smaller file.
- **Summary:** the top of the section shows exactly what the export will do: how many times the animation plays, at what speed, and how close the shiny effect is to game speed.

---

## Randomise

**Randomise**, or the 🎲 on a section, rolls new settings.

- **Wildness:** pulls values toward the game look. At 0% you get the game values; at 100% the full allowed range is used.
- **Limits:** every setting has a min and max you can edit, so you never get absurd results such as a sparkle 30× its normal size. Untick a setting to leave it alone.
- **Effects stay as they are:** by default, randomising doesn't switch effects on or off. A checkbox allows it.
- **Undo:** reverts the last roll.

---

## Presets

| Preset | Effects |
|---|---|
| Shiny: battle *(default)* | Shiny colours + shine |
| Shiny: overworld + sparkles | Shiny + sparkles, alpha cutoff 0.1, a 4.4 s loop (two cycles, so you get more variety in sparkles) |
| Shiny: icon | Shiny with the icon material's alpha cutoff of 0.25 |
| Pulsing | The game's pulse only |
| Combo presets | Shiny + pulsing, with or without sparkles |
| Extras | Sparkles only, randomised sparkles, a still frame using the shiny material's saved hue setting (244.83°), rainbow shine, gold flash, double sweep |

Save your own with **Save as…**. Share them with **Export .json** and **Import**.

---

## Supported formats

| Format | Support |
|---|---|
| PNG, JPG, WebP, AVIF, BMP, ICO, SVG | Yes |
| Animated GIF | Yes, in every browser (built-in decoder) |
| APNG | Yes, in every browser (built-in decoder) |
| Animated WebP | Chrome and Edge. Other browsers use the first frame. |
| PSD, TIFF and other editor formats | Open them in Photopea, then use **Get from Photopea** or **Get animation**. |

---

## Exports

| Export | Notes |
|---|---|
| **GIF** | Universal. Only fully on or off transparency, so soft edges get hard; use a matte colour for clean edges on a known background. |
| **APNG** | Full transparency and exact colours. |
| **Sprite sheet** | A PNG grid, read left to right, top to bottom. Columns are adjustable. |
| **Frames .zip** | One PNG per frame, plus `timing.txt` with each frame's delay. |
| **This frame** | The frame currently shown, as a PNG. |
| **GIF → Photopea** | Opens the result in Photopea, one layer per frame (plugin mode only). |

Other export options:
- **Trim empty space** (on by default): crops every export to the area the animation actually uses.
- **Upscale**: pixel-perfect enlargement for pixel art.
- **Canvas growth**: the canvas grows automatically so sparkles and outlines never get cut off.
- **Seamless loops**: every export loops seamlessly, with frame delays that add up exactly to the loop length.

---

## Command-line tool

`shiny.py` batch-applies the shiny colours and shine, with the same maths and defaults as the plugin. It doesn't do pulsing, sparkles or animated input.

```bash
pip install pillow numpy
python shiny.py morty.png                 # -> morty_shiny.gif + morty_shiny.png (APNG)
python shiny.py sprites/*.png --scale 4   # batch, 4x pixel-art upscale
python shiny.py morty.png --icon          # icon material (alpha cutoff 0.25)
python shiny.py morty.png --still 244.83  # one still frame with a fixed hue shift
python shiny.py --help                    # every option
```

---

## How accuracy was checked

- **Shiny colours:** the game's colour-rotation maths fits a 30 fps recording of the game smoothly. A plain HSV hue shift fits worse and jumps around.
- **Shine:** the band's width and falloff come from the material values. Its direction, start time and duration were measured from footage, accurate to about one frame (±0.03 s).
- **Pulsing:** the colours and texture come from the material. The timing was fitted to battle footage.
- **Sparkles:** each sparkle was isolated by subtracting the matching frame from the next 2.2 s cycle, where Morty and the background are identical. Their positions, sizes and movement were measured from that.
- **Decoders and output:** the GIF and APNG decoders match Pillow pixel for pixel. With every effect off, exported frames match the source frames exactly.

---

## Known limitations

- The Photopea bridge (**Get from Photopea**, **Get animation**, **GIF → Photopea**) has been tested in a browser but not yet inside Photopea itself.
- Sparkle sizes are measured as a percentage of the image's height. If your sprite has a lot of empty space around it, the sparkles will look smaller than in the game. Crop the sprite, or raise **Size min / max**.
- The pulse outline is 9.5 px wide in the game, which suits full-size Morty art. Turn **Outline width** down for small or pixel-art sprites.
- Very large animations are capped at about 600 MB of decoded frames. Each frame is also limited to 4096 × 4096.

---

## Credits and disclaimer

- A fan-made tool. It is not affiliated with or endorsed by the makers or publishers of Pocket Mortys or *Rick and Morty*. *Pocket Mortys* and *Rick and Morty* belong to their respective owners.
- The built-in sparkle and pulse textures come from the game's files. They are included only so the effect can be reproduced accurately.
- GIF encoding uses [gifenc](https://github.com/mattdesl/gifenc) by Matt DesLauriers (MIT licence), built into the HTML file.
- Effect maths follows the AllIn1SpriteShader shader keywords used by the game's materials.