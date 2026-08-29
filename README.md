# Globify

Globify turns a flat world map into a rotatable 3D globe in the browser. Drop in an image, add a few optional map layers, adjust the globe, then save a PNG or an animated GIF.

There is no account, server, upload or dependency install. I made it as one portable `index.html` file because a map sketch should not require a small ceremony involving Node, npm and a terminal window.

The interface uses Google’s Atkinson Hyperlegible for the control panel when a connection is available. If it is not, the app falls back to ordinary system sans-serif text and keeps working. Your maps still never leave the browser.

**New to GitHub Pages? Read [the no-nonsense setup guide](SETUP.md).** It assumes you have never deployed anything and explains every click.

## What it does

- Wraps equirectangular and Mercator map sheets around a WebGL globe.
- Warns when the source proportions do not describe a complete world cleanly.
- Handles missing polar areas with average colour, edge colour or custom caps.
- Keeps a simple, fixed layer stack: biomes, borders, labels, clouds, night lights and height relief.
- Adds one separately textured moon, positioned on a simple visual orbit.
- Lets you move the prime meridian, axial tilt, spin, light, graticule, stars and atmosphere.
- Includes scoped resets for the view, display, optional layers and moon.
- Exports the current view as a PNG or a full rotation as a GIF.
- Saves and reloads settings as JSON without quietly packing your artwork into it.
- Runs locally once downloaded. Your maps stay in your browser.

## Start here

### Just use it on your computer

1. Download or unzip this project.
2. Open `index.html` in a current desktop browser.
3. Click **Choose, drop, or paste a map image** and select your base map.
4. Drag the globe, then use **Save this view as PNG** or **Export a full turn as GIF**.

That is the whole installation. The app is already built.

If a browser blocks opening local files, use the GitHub Pages guide instead. It is usually less effort than arguing with browser security settings, which are doing their job even when it feels personal.

## Make a proper globe

The base map is the geography. It sets the colour and any polar-cap treatment. A complete equirectangular world is normally **2:1**: twice as wide as it is high. It covers 360° of longitude and 180° of latitude.

Mercator is different. It stretches high latitudes, so it often stops short of the poles. Choose **Mercator**, then use the latitude span slider or **Use the undistorted span**. Globify will not invent geography for the missing bits. It will cap them in the style you choose.

Use **Download a template for these settings** if you are painting a map from scratch. The guide shows the latitude spacing expected by the selected projection.

## Layers

Every layer must line up with the base map: same projection, latitude span, seam and dimensions. PNG files with transparent backgrounds are the sensible choice for labels, borders and clouds.

| Layer | What it is for | Notes |
| --- | --- | --- |
| Biomes | A translucent climate or terrain tint | Loaded beneath borders and labels. |
| Borders | Political or regional boundaries | A transparent PNG works best. |
| Labels | Place names and annotations | Keep text large enough to survive a spin. |
| Clouds | A cloud sheet | This is a static overlay, not weather simulation. |
| Night lights | A light map | Visible with **Sunlight** on. It is a stylised glow, not a physics model. |
| Height map | Greyscale relief | Visible with **Sunlight** on. Keep the opacity low unless you enjoy planets made of chewing gum. |

The order is fixed on purpose. Reorderable layers, blend modes and per-layer seams sound modest until they turn a compact map tool into a browser GIS project. That is future work, not an invisible promise.

## Moon

**Moon** is a separate small globe, not a paint layer on the planet. Switch it on, then either leave the generated crater texture in place or load a second equirectangular map sheet for it. Size, distance, orbital position and tilt are saved with the project settings.

It is a composition tool, not a gravity simulator. One moon is intentional for now. A useful multi-moon system needs named bodies, drawing order and a less crowded control panel, rather than a duplicated file picker and optimism.

## Controls

| Job | Control |
| --- | --- |
| Rotate | Drag, or use the arrow keys |
| Pan | Shift-drag, right-drag or two-finger drag |
| Zoom | Mouse wheel or pinch |
| Re-centre | Double-click the globe or click **Center the globe** |
| Close export preview | Click **Close** or press Escape |

The GIF exporter offers 320, 480 and 640 pixel frames, plus 36, 60 or 90 frames per turn. GIF is old and limited to 256 colours, but it still travels well. Dither can soften colour bands at the cost of a larger file.

Transparent GIF turns off the starfield and atmosphere halo because GIF transparency is extremely basic. A transparent pixel is either there or it is not.

## Quick resets

The new **Quick resets** section is deliberately specific. It exists to make experimentation safe without offering one large red button that erases a project because someone was curious.

| Button | What it resets | What it keeps |
| --- | --- | --- |
| Reset view | Globe rotation, zoom, pan and drag momentum | Map, layers, moon and display settings |
| Reset display | Graticule, sunlight, starfield, halo and spin to the starting display | Map, layers, moon and camera position |
| Clear layers | Removes the six optional sheets and restores their default opacity values | The base map and all geometry settings |
| Reset moon | Hides the moon, restores its generated crater texture and its default pose | The planet and every map layer |

## Save a settings project

**Save settings as project JSON** keeps the globe settings, layer opacities, display switches and GIF choices. It deliberately does not include the source images.

When you load a settings project, add the map files again. This keeps private artwork out of a JSON blob that might otherwise get copied to places it should not be.

## GitHub Pages, from zero

Everything needed for a simple Pages deployment is already in this folder, including `.nojekyll`. There is no build command and no package manager.

Follow [SETUP.md](SETUP.md) for the complete beginner version. The short form is:

1. Create a new GitHub repository.
2. Upload the contents of this folder so `index.html` sits at the repository root.
3. In **Settings → Pages**, choose **Deploy from a branch**, select `main` and `/(root)`, then save.
4. Wait for GitHub to show the published URL.

GitHub’s own Pages instructions cover the current settings screen: [configuring a publishing source](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).

## Limits worth knowing

- You need a modern browser with WebGL enabled.
- Large source images and long 640 px GIFs can be slow, especially on phones.
- All layers are static images. There is no timeline, animation system or live weather data.
- The self-contained renderer is based on embedded Three.js r128. It is dependable for this tool, but updating it needs proper testing rather than blind version-number worship.

## Future ideas - maybe

Ok o the sensible next additions are a seam checker, simple crop and padding tools, named layer presets, controlled blend modes, PNG sequences or WebM export, and an optional File System Access workflow for reopening a project and its original images.

I would keep the released app as one HTML file. For maintenance, though, it should eventually have modular source files and a small build step that produces that one file. Users get the neat portable thing; maintainers get fewer 700 kB surgery sessions. Everybody wins a little.

---
##Do check out the origional creator - https://github.com/Viruzodro/globify

## Licence

Globify is released under the [MIT Licence](LICENSE). The bundled Three.js code is MIT. Spectral and IBM Plex Mono use the SIL Open Font Licence 1.1. Atkinson Hyperlegible is loaded from Google Fonts when available.
