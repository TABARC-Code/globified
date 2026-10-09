# Globify

Globify turns a flat world map into a rotatable 3D globe in the browser. Drop in an image, add a few optional map layers, adjust the globe, then save a PNG, an animated GIF or a video.

There is no account, server, upload or dependency install. I made it as one portable `index.html` file because a map sketch should not require a small ceremony involving Node, npm and a terminal window.

The interface uses Google’s Atkinson Hyperlegible for the control panel when a connection is available. If it is not, the app falls back to ordinary system sans-serif text and keeps working. Your maps still never leave the browser.

**Never run a local HTML app before? Read [the no-nonsense setup guide](setup-guide.md).** It covers Windows, macOS and Linux, assumes nothing and explains every click. Putting it on GitHub Pages is further down this page.

## What it does

- Wraps equirectangular and Mercator map sheets around a WebGL globe.
- Warns when the source proportions do not describe a complete world cleanly, then offers to crop or pad the sheet to 2:1.
- Handles missing polar areas with average colour, edge colour or custom caps.
- Keeps a simple, fixed layer stack: biomes, borders, labels, clouds, night lights and height relief.
- Adds one separately textured moon, positioned on a simple visual orbit.
- Lets you move the prime meridian, axial tilt, spin, light, graticule, stars and atmosphere.
- Includes scoped resets for the view, display, optional layers and moon.
- Checks whether the left and right edges of your map actually meet, before the globe shows you the hard way.
- Exports the current view as a PNG, or a full rotation as a GIF or a WebM/MP4 video.
- Saves and reloads settings as JSON without quietly packing your artwork into it.
- Runs locally once downloaded. Your maps stay in your browser.

## Start here

### Just use it on your computer

1. Download or unzip this project.
2. Open `index.html` in a current desktop browser.
3. Click **Choose, drop, or paste a map image** and select your base map.
4. Drag the globe, then use **Save this view as PNG**, **Export a full turn as GIF** or **Export a full turn as video**.

That is the whole installation. The app is already built.

If a browser blocks opening local files, use the GitHub Pages guide instead. It is usually less effort than arguing with browser security settings, which are doing their job even when it feels personal.

## Make a proper globe

The base map is the geography. It sets the colour and any polar-cap treatment. A complete equirectangular world is normally **2:1**: twice as wide as it is high. It covers 360° of longitude and 180° of latitude.

Mercator is different. It stretches high latitudes, so it often stops short of the poles. Choose **Mercator**, then use the latitude span slider or **Use the undistorted span**. Globify will not invent geography for the missing bits. It will cap them in the style you choose.

### Crop and pad to 2:1

When an equirectangular sheet isn't 2:1, buttons appear under the advice box.

- **Pad to 2:1** is for sheets that are too wide. These are usually maps that stop short of the poles. It adds bands above and below, painted with whatever **Polar caps** treatment you've picked. Change the caps afterwards and the bands follow.
- **Crop to 2:1** is for sheets that are too tall, normally because of a title block, a legend or a margin someone thought was a good idea. It trims rows off the top and bottom, and a slider picks which rows survive.
- **Use original** puts the untouched image back. Nothing is destroyed. Globify always keeps the original and builds the fixed copy from it.
- **Download the 2:1 sheet** saves the fixed version as a PNG, so other tools get a proper sheet too.

There's deliberately no "crop the sides" option. Trimming longitude off a world map breaks the wrap, and I'd rather not offer a button whose only job is ruining things.

Every layer gets the same treatment as the base map, including layers loaded after you press the button. Layer bands are transparent, so your labels don't grow grey hats. The choice is saved in project JSON too.

### The seam check

A world map wraps round on itself, so the far-left column of pixels has to sit happily next to the far-right one. When you load a base map, Globify compares the two edges and tells you whether they match, differ a bit or clearly don't. A clear mismatch turns the advice box red.

It's a blunt instrument. It averages colour differences down the two edges and nothing more clever than that. It won't tell you a coastline is in the wrong place, but it will catch the classic map that was cropped from something bigger and never meant to wrap. If you can't repaint the edges, slide the **Prime meridian** round so the join faces away from your best side. Everyone has one.

### Templates

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
| Re-centre | Double-click the globe or click **Centre the globe** |
| Close export preview | Click **Close** or press Escape |
| Load a layer or moon map | Click its button, or drop the file straight onto it |
| Cancel a GIF or video export | Click **Cancel** on the progress card, or press Escape |
| Show all of this in the app | Press `?`, or **Controls and shortcuts** under View |
| Fold a panel section away | Click its heading, or Tab to it and press Enter |

The panel comes in five sections: Map, Layers, Moon, Globe and view, and Save and export. Layers and Moon start folded, because most globes don't need them. The Layers heading tells you how many are loaded, so a folded section can't hide the fact that something is in it. Your browser remembers which sections you leave open.

Drop a file anywhere else and it becomes the base map. On a phone held upright, the globe pulls back so the whole planet fits. Zoom yourself and it stops second-guessing you; **Reset view** hands control back.

The GIF exporter offers 320, 480 and 640 pixel frames, plus 36, 60 or 90 frames per turn. GIF is old and limited to 256 colours, but it still travels well. Dither can soften colour bands at the cost of a larger file.

Transparent GIF turns off the starfield and atmosphere halo because GIF transparency is extremely basic. A transparent pixel is either there or it is not.

### Video

**Export a full turn as video** uses the browser's own recorder, so you get WebM in Chrome, Edge and Firefox, and MP4 where WebM isn't on offer (usually Safari). It borrows the GIF size and the seconds-per-turn slider, then records at 30 fps.

The catch: it records in real time. A 10-second turn takes 10 seconds. Keep the tab in front while it runs, because browsers throttle background tabs and the result gets jerky. No transparency either. Video files are far smaller than GIFs and don't band colours, so for anything going on a website, video is the better bet. GIF is still the one that pastes into everything.

If the button is greyed out, your browser doesn't have MediaRecorder video support. That's rare now, but it happens.

## Quick resets

The new **Quick resets** section is deliberately specific. It exists to make experimentation safe without offering one large red button that erases a project because someone was curious.

| Button | What it resets | What it keeps |
| --- | --- | --- |
| Reset view | Globe rotation, zoom, pan and drag momentum | Map, layers, moon and display settings |
| Reset display | Graticule, sunlight, starfield, halo and spin to the starting display | Map, layers, moon and camera position |
| Clear layers | Removes the six optional sheets and restores their default opacity values. **Undo clear layers** brings them back until you load another layer | The base map and all geometry settings |
| Reset moon | Hides the moon, restores its generated crater texture and its default pose | The planet and every map layer |

## Save a settings project

**Save settings as project JSON** keeps the globe settings, layer opacities, display switches and GIF choices. It deliberately does not include the source images.

When you load a settings project, add the map files again. This keeps private artwork out of a JSON blob that might otherwise get copied to places it should not be.

Your browser also remembers the last settings you used, so closing the tab doesn't throw away an hour of fiddling with tilt and moon orbits. Same rules: settings only, never images, and it lives in that one browser. A private window forgets, as it should. The project file is still the way to keep a setup or pass it on.

## GitHub Pages, from zero

Everything needed for a simple Pages deployment is already in this folder, including `.nojekyll`. There is no build command and no package manager.

If you only want to use it yourself, [setup-guide.md](setup-guide.md) gets you running locally without any of this. For Pages, the short form is:

1. Create a new GitHub repository.
2. Upload the contents of this folder so `index.html` sits at the repository root.
3. In **Settings > Pages**, choose **Deploy from a branch**, select `main` and `/(root)`, then save.
4. Wait for GitHub to show the published URL.

GitHub’s own Pages instructions cover the current settings screen: [configuring a publishing source](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).

## Limits worth knowing

- You need a modern browser with WebGL enabled.
- Large source images and long 640 px GIFs can be slow, especially on phones.
- Video export records in real time and needs the tab to stay in front.
- All layers are static images. There is no timeline, animation system or live weather data.
- The self-contained renderer is based on embedded Three.js r128. It is dependable for this tool, but updating it needs proper testing rather than blind version-number worship.

## Future ideas, maybe

Three items off the old list are done: the seam checker, video export, and crop and pad. What's left, in rough order of usefulness: named layer presets, controlled blend modes, PNG sequences, and an optional File System Access workflow for reopening a project with its original images.

What changed and when lives in [CHANGELOG.md](CHANGELOG.md).

I would keep the released app as one HTML file. For maintenance, though, it should eventually have modular source files and a small build step that produces that one file. Users get the neat portable thing; maintainers get fewer 700 kB surgery sessions. Everybody wins a little.

---
## Credit

This is a fork. Do check out the original creator: [Viruzodro/globify](https://github.com/Viruzodro/globify).

Fork maintained by TABARC-Code.

## Licence

Globify is released under the [MIT Licence](LICENSE). The bundled Three.js code is MIT. Spectral and IBM Plex Mono use the SIL Open Font Licence 1.1. Atkinson Hyperlegible is loaded from Google Fonts when available.
