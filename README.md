# Globify

Wrap a flat map onto a 3D globe in your browser, then export a looping GIF of it turning.

Built for worldbuilders who have a map and want to see what their world actually looks like as a planet.

**[Try it in your browser](https://viruzodro.github.io/globify/)**  ·  **[Download it to keep](https://github.com/viruzodro/globify/releases/latest)**

![A world map turning as a globe](demo.gif)

## What it does

- Drop in a JPG or PNG and see it wrapped on a sphere immediately
- Reads your map as either **equirectangular** or **Mercator**, which is what determines whether your continents keep their shapes
- Adjustable latitude span, so your art can stop short of the poles instead of pinching to a point
- Movable prime meridian for hiding the seam behind an ocean
- Axial tilt, spin rate, graticule overlay, sunlight shading, starfield, atmosphere halo
- Downloadable drawing templates with distortion marks, sized for whatever projection you pick
- Exports a still PNG or a seamless looping GIF of a full 360° turn

## Nothing is uploaded

Your map never leaves your computer. There is no server, no account, no analytics. The page reads the file locally and does all rendering and encoding in your browser.

## Works offline

`index.html` is completely self-contained. Three.js and both typefaces are embedded in the file. Download it, open it, and it works with your wifi off, forever.

## What aspect ratio should my map be?

For **equirectangular**, exactly 2:1. Width must be double the height, because the projection spreads the full width across 360° of longitude and the full height across 180° of latitude. Good sizes are 2048×1024 or 4096×2048.

For **Mercator**, any aspect ratio works. A 16:9 map covers about ±70.6° of latitude, and the poles above and below get filled with a cap color sampled from your map's edges.

If your flat map looks correct to your eye, it is probably closer to Mercator than equirectangular. A true equirectangular map has to be drawn pre-stretched, with the polar regions smeared absurdly wide, so that the stretching cancels out once it wraps. That is why Antarctica looks enormous on a world map.

Use the **Download template** button to get a guide sheet at the right dimensions. Every circle on it becomes a true circle on the globe, so round marks mean your shapes will survive.

One more thing: make the left and right edges of your map match, since they meet at the seam.

## Controls

| Action | Mouse | Touch |
|---|---|---|
| Spin the globe | Drag | Drag |
| Move the globe | Shift-drag or right-drag | Two-finger drag |
| Zoom | Scroll | Pinch |
| Re-center | Double-click | The Center button |

## GIF export notes

- **Transparent** removes the background so the globe can sit on any page. It turns off the starfield and halo, because GIF transparency is strictly on or off per pixel and soft glows come out with ragged edges.
- **Dither** hides color banding in the shading. GIF holds only 256 colors, so smooth gradients would otherwise break into stripes. Costs roughly double the file size.
- A single palette is built across the whole rotation rather than per frame, which is what keeps the loop from shimmering.

If the save button does nothing, press and hold the preview image and choose Save image. Some browsers block downloads from embedded frames.

## Running it yourself

Download `globify.html` from the [latest release](https://github.com/viruzodro/globify/releases/latest) and double-click it. That's the whole install. No dependencies, no build step, no internet needed after the download.

## Built with

[Three.js](https://threejs.org) (MIT). Typefaces are [Spectral](https://fonts.google.com/specimen/Spectral) and [IBM Plex Mono](https://fonts.google.com/specimen/IBM+Plex+Mono), both SIL Open Font License 1.1. The GIF encoder is written from scratch in this file: median-cut quantization, Floyd-Steinberg dithering, and LZW compression, with no dependencies.

## License

MIT. See [LICENSE](LICENSE).
