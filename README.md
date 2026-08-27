# Globify v2

Globify wraps a flat world map around a 3D globe, entirely in the browser. Add a base map, put labels or borders on an overlay, spin it about, and export a still or a looping GIF. No account. No upload. No little data siphon hiding in the flowerbed.

Open `index.html` directly. It works offline.

## What changed

- Checks map proportions and explains when an equirectangular sheet cannot honestly be a full world.
- Accepts modern browser-decodable image formats instead of rejecting HEIC by superstition before trying.
- Adds one transparent overlay layer for labels, borders, clouds, political divisions, or whatever is currently keeping the lore coherent.
- Adds average, edge-smear, and custom-colour polar caps.
- Adds project settings JSON. This stores settings, not artwork: browsers should not quietly bundle private maps into random data files.
- Lets GIF export retain the current framing rather than always snapping back to the central showroom pose.
- Releases Blob URLs and old `ImageBitmap` resources when maps are replaced.
- Improves preview-dialog keyboard behaviour: focus goes to Close, and Escape closes it.

## Projection, briefly

### Equirectangular

A complete equirectangular planet is **2:1**: twice as wide as it is high. It represents 360° longitude across the width and 180° latitude down the height. Anything else is either a partial-latitude sheet, a crop, or a map telling fibs through its aspect ratio.

### Mercator

Mercator maps stretch the high latitudes. That is their job. A 16:9 Mercator sheet naturally covers about ±70.6° and leaves polar territory to the cap setting. The globe is not repairing the source map. It is interpreting it correctly.

## Controls

| Action | Mouse / touch | Keyboard |
| --- | --- | --- |
| Rotate | Drag | Arrow keys |
| Pan | Shift-drag, right-drag, or two-finger drag | Centre button |
| Zoom | Scroll or pinch | — |
| Re-centre | Double-click | Centre button |
| Close an export preview | Click Close | Escape |

## Layers

The base map determines colour and polar caps. The overlay is composited over it using the same projection, latitude span, and prime meridian. It should therefore be made to the same dimensions as the base sheet. Transparent PNGs are ideal for labels and borders.

This is intentionally one overlay, not a premature GIS workstation. More layers need sensible ordering, blend modes, layer-specific seams, project references, and a less chaotic interface. Those are future work, not a checkbox parade.

## Export notes

GIF is old, limited to 256 colours, and still rather useful. Dither hides banding at the cost of size. A shared palette is built across the entire rotation, preventing the shimmer that makes so many GIFs look as though their continents are having a mild panic attack.

Transparent GIF disables the starfield and atmosphere halo because GIF transparency has the emotional range of a light switch.

## Project settings

**Save settings as project JSON** preserves projection, cap treatment, view settings, display toggles, and GIF choices. It intentionally does not include source images. Reload the maps after loading the project.

## Future ideas, considered rather than promised

The next sensible tier would be:

- seam assistant, comparing the map edges and suggesting quiet oceans or regions where the mismatch is least visible;
- crop and pad tools for repairing near-equirectangular maps without leaving the app;
- several named layers with opacity, order, and blend modes;
- a real polar extension method, such as mirrored terrain or painted polar continuation, rather than increasingly elaborate excuses;
- APNG, PNG sequence, and WebM export where the browser can encode them reliably;
- saved project references using the File System Access API as an optional convenience, never a requirement;
- a modular source build that generates the portable single HTML release;
- a controlled upgrade from the embedded Three.js r128 renderer.

The last two matter. The current single-file approach is excellent for users, but not a brilliant place to perform surgery. Maintain source modules, test them, then produce `index.html` as the offline artefact.

## Licence

MIT. Three.js is MIT. Spectral and IBM Plex Mono are licensed under SIL Open Font License 1.1.
