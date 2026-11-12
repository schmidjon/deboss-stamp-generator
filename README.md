# Deboss Stamp Generator

## Tool running at GitHub pages here 
[https://schmidjon.github.io/deboss-stamp-generator/deboss-generator.html](https://schmidjon.github.io/deboss-stamp-generator/deboss-generator.html)

A single web page that turns an SVG logo into a 3D-printable **debossing stamp** and a matching **cover template (jig)**, for pressing logos into hardcover notebooks with a manual desktop press.

Built for repeatable volunteer production: every stamp drops into its template only one way. The template locks to the notebook's cover edges, so the logo lands in the same place on every notebook.

## What it makes

**Stamp**
- **Smooth curves:** they follow the SVG's Bézier curves to within 0.003 mm, so they come out smooth instead of jagged.
- **Tapered walls:** the raised logo leans out about 10° toward the base. Fine details are stronger and the stamp lifts out of the cover cleanly.
- **Mirrored:** the stamp is mirrored, so the impression reads correctly. It's exported face-up, ready to print.
- **Key tab:** a tab fits the template's notch only one way round. An arrow is engraved on the back.
- **Outline:** round for round logos, a rounded rectangle otherwise. You can override the choice.

**Cover template**
- A plate with lips that hook over the fore-edge, top and bottom of the closed notebook. It positions the same way whatever the notebook's thickness.
- A keyed window that holds the stamp at the chosen spot, centred by default with adjustable offsets.
- Unused plate area is cut away to save plastic. It prints flat with the lips up, no supports.

**Checks**
- Mesh check: confirms both models are watertight, with no open edges.
- Detail check: flags lines and gaps too thin to press cleanly, in red and orange on the cover preview.
- Fit warnings: covers a stamp too close to the cover edge, lips deeper than the notebook, and the stamp-vs-template height.

**Exports**
- One `.3mf` with both parts side by side (opens in Bambu Studio and other slicers).
- Separate `.stl` files for each part.

## Using it

1. Open `deboss-generator.html` in Chrome, Edge, Safari or Firefox. It needs an internet connection the first time it loads its libraries (three.js, Clipper, earcut, fflate from jsDelivr).
2. Drop in an SVG logo.
3. Pick a notebook preset, or enter your own **measured** cover width, height and closed thickness. Measure real notebooks with calipers rather than trusting the manufacturer's size.
4. Set the logo size and position, then review the warnings and previews.
5. Download the `.3mf` and print.

**SVG tips**
- Filled shapes work best. Strokes are supported.
- Convert live text to outlines before exporting the SVG.
- Shapes are combined in the order they're drawn: dark shapes are raised and light shapes cut back into them. This handles logos with white details knocked out of black areas. Use **Raise light areas instead of dark** to flip it.

**Saving setups**
- **Save as preset…** remembers a measured notebook size in your browser.
- **Save settings file** downloads a `.json` file with every setting, so a stamp can be regenerated exactly or shared with another computer.

## Notebook presets

| Preset | Width × height | Closed thickness |
|---|---|---|
| LYDTICK A5 hardcover (measured) | 146.24 × 212.33 mm | 15.08 mm |
| A5 nominal | 148 × 210 mm | — |
| B5 nominal | 176 × 250 mm | — |
| A6 nominal | 105 × 148 mm | — |
| 5 × 8 in | 127 × 203.2 mm | — |

## Key settings

| Setting | Default | Notes |
|---|---|---|
| Logo size | 100 mm | Width or height |
| Back thickness + relief height | 3.8 + 1.2 mm = **5.0 mm** | Keep the total the same for every stamp, so the press stop never needs re-setting |
| Wall taper | 10° | 0° gives straight walls |
| Taper step height | 0.1 mm | Set it equal to your print layer height |
| Clearance around stamp | 0.25 mm | Raise if the stamp binds in the window, lower if it rattles |
| Clearance to cover edges | 0.3 mm | Template fit on the cover |
| Edge lip depth | 6 mm | Must be less than the notebook thickness |

## Print settings (Bambu Lab H2S, 0.2 mm nozzle)

**Stamp**
- **Layers:** 0.08–0.12 mm, matching the generator's taper step.
- **Strength:** 4–6 walls, 6+ top layers and 40% gyroid infill, so the stamp doesn't flex under the press.
- **Surface:** ironing on the topmost surface only, for an even impression.
- **Material:** PLA. Avoid PETG and TPU, which give under pressure.
- **Orientation:** print as exported, face-up, no supports.

**Template**
- **Layers and fill:** 0.16–0.2 mm layers, 3 walls, 15% infill.
- **First layer:** keep elephant-foot compensation at 0.15 mm. The window's edge is printed on the bed, so a bulging first layer would make the stamp bind.

**Before printing in quantity**
1. Print one of each and check the fit.
2. Do a test press on a spare notebook.
3. Adjust the clearances or **Thicken** if needed, then save the settings file.

## Volunteer steps

1. Close the notebook and place it front cover up on the press bed, on the white die pad.
2. Lay the template on the cover and slide it until the lips touch the **fore-edge**, with the top and bottom lips hugging the cover. The arrow points to the top of the notebook.
3. Turn the stamp logo-side down and drop it into the window, with the key tab in the notch.
4. Centre it under the upper plate and pull the handle down once, firmly, to the stop.
5. Lift the stamp straight up, then the template, and check the impression.

## Repository contents

| File | Purpose |
|---|---|
| `deboss-generator.html` | The generator. Everything is in this one file |
| `samples/MEA_seal_blk.svg` | Sample logo used for testing |
| `.claude/launch.json` | Local preview server config for Claude Code |

Generated `.stl`, `.3mf` and `.gcode` files are git-ignored. Keep any local credentials or machine-specific config in `config.local.*` or `.env`, which are also ignored.
