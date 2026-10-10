# Changelog

Author: TABARC-Code

Notes on what changed, written down so future me doesn't have to read a 700 kB diff to find out. Newest at the top.

## October 2026, part eleven: sliders that keep up

### Better

- **Opacity sliders are fast on big maps.** Every repaint used to re-project every sheet onto the globe: 1,024 strips each, sampled from images that can be 8K wide. That happened even when all you'd touched was an opacity slider, which doesn't change the geometry at all. Now each sheet keeps its projected copy, and an opacity change just blends the copies. The full projection only re-runs when something geometric changes: a new image, the span, the projection, the caps, or a crop or pad. Night lights and relief only repaint on those changes too, since their opacity is just a material setting.
- Measured on an 8K map with four layers: an opacity step went from about 140 ms of script time to under a tenth of a millisecond. A 100-step drag went from 21.3 s to 13.6 s wall clock. That was in a headless browser with software 3D, where uploading the texture dominates what's left, so a real graphics card should do better than that. I can't prove it here, though.
- The trade: a full reprojection is about 30% slower than before (132 ms to 171 ms), because each layer gets projected into its own copy. Opacity is what people drag, so that's the right way round.
- I checked the output too. Rendered frames match the old version to within 3 levels out of 255, on a handful of pixels: rounding, not a visible change. This is the CPU-side version of the "draw layers on the graphics card" idea. It got most of the win without rewriting any shaders.

## October 2026, part ten: layer presets

### New

- **Layer presets.** There's a list under the layers with six built-ins: Everything, Political, Physical, Weather, Night side and Bare map. You can add your own as well. A preset is the six layer opacities plus the Sunlight switch, because a night-lights preset that leaves the sun off just shows you nothing and makes you think the app's broken. Name a mix, press Save or Enter, and it's kept. The same name again updates it.
- Presets travel in project files and the remembered settings, never with images. Loading a project merges its presets into yours and never deletes any. Built-in names are protected, so "political" can't quietly replace Political.
- I threw junk at it on purpose: blank names, a name full of HTML, an opacity of 900, and a sun setting of `"yes"`. Blank names are rejected, the HTML shows up as plain text, 900 becomes 100, and `"yes"` is ignored.

## October 2026, part nine: the PNG you actually saw

### Fixed

- **Saved PNGs were off-centre.** The 3D canvas fills the whole window, and the panel sits over part of it, so the globe gets nudged sideways to stay in view. The PNG grabbed the entire canvas, panel area included. At 1366 px wide that put the planet 181 px off-centre beside a strip of empty stars nobody had seen. It now saves only the visible part. On a phone it stops at the top of the panel. The planet lands where you framed it, give or take a few pixels of honest perspective.

## October 2026, part eight: the keyboard gets the whole app

### Fixed

- **You couldn't load anything from the keyboard.** Every file button is a label for a hidden file input, and labels can't take focus, so Tab skipped all of them: base map, six layers, the moon and project files. Unless you could paste an image, a keyboard user couldn't even start. The buttons now take focus and open the picker on Enter or Space. It's been like that since the first upload.

### Better

- **Keyboard zoom.** `+` and `-` zoom, and `0` resets the view. It's ignored while a slider or other control has focus, same as the arrow keys. The help overlay lists them.
- **Exports are named after your map.** `Aerth.png` gives `Aerth-view.png`, `Aerth-turn-480px-60f.gif`, `Aerth-2x1.png` and so on. Odd characters get tidied into hyphens.

### Tried and dropped

I wanted to smooth out slider dragging on huge maps. On an 8K map each layer repaint costs about 50 ms, so I tried batching repaints into one per screen frame. Then I measured it: 100 drag events still gave 100 repaints, because each repaint is slow enough that only one event ever arrives per frame. It also added a frame of delay every time, and the whole drag took longer. So it's gone. A real fix means drawing the layers on the GPU instead, which is a bigger job and not one for a tidy-up round.

## October 2026, part seven: Tab stays put

### Fixed

- **Tab walked out of overlays.** With the help card, an export result or the progress card open, Tab moved focus into the panel behind, where a keyboard user couldn't see it and could change things they weren't looking at. Tab and Shift+Tab now cycle inside whichever overlay is open, and once it closes, Tab works through the panel as normal. It was left over from the review. Small, but it's the sort of thing that makes a tool quietly unusable for someone.

## October 2026, part six: a proper look round

I reviewed the whole repo before adding anything else, and found three things that were simply wrong.

### Fixed

- **Loading a project told you to re-add a map that was already there.** The message went onto the base map's button, over the map's own name. It now has its own line in the project section, and only asks for the map when there isn't one.
- **A damaged project file could leave "±NaN°" on the panel for good.** A missing `spanDeg` slipped straight through, and remembered settings then saved it, so it came back on every reload. Every value in a project file is now checked as it loads. Missing, mistyped or silly values fall back to the defaults or get clamped. I fed it a deliberately rubbish file, with things like `"capMode": "plaid"` and a moon orbit of a billion degrees, and nothing got through.
- **Picking the same file twice did nothing.** Browsers only react when a file picker's value changes, and none of ours ever cleared it. They clear themselves now, so reloading the same project after fiddling actually reloads it.

### Docs

- The README now says the video button is also greyed out until a map is loaded, lists everything a project file saves, and warns about Safari's canvas size limit on very large maps. That last one is untested on a real Safari, and the README says so.

## October 2026, part five: a shorter panel, and a second chance

### New

- **Collapsible sections.** The panel is now five sections: Map, Layers, Moon, Globe and view, and Save and export. They're plain `<details>` elements, so the keyboard and screen readers handle them without any help from me. Layers and Moon start folded. The Layers heading shows "· 2 loaded" or similar, so folding a section never hides that it has something in it. The browser remembers which ones you leave open. First visit is roughly 2,400 px of panel instead of 3,050. Fold what you don't use and it's a lot less.
- **Undo clear layers.** **Clear layers** now sets the images aside instead of throwing them away, and an **Undo clear layers** button appears. Images, button names and opacities all come back. It forgets once you load another layer or clear again, which is the point where an undo stops meaning anything obvious.

### Fixed

- **Hidden things weren't hidden.** Any panel row with its own `display` style ignored the `hidden` attribute. So the **Custom** cap colour pickers showed whatever cap mode you'd picked, and they always have. The crop position slider showed with no crop active, and that one was mine, from part two. One global rule fixes both. My earlier test checked the attribute rather than what was actually on screen, which is a lesson I'd quite like to only learn once.
- Removed a CSS rule that never matched anything. It would have started matching inside the new sections and pushed each one's first row down by 78 px.

## October 2026, part four: a way out, and a cheat sheet

### New

- **Cancel exports.** The progress card has a **Cancel** button, and Escape does the same. A GIF stops at its next frame. A video stops its recorder. Either way the view goes back to how it was and nothing gets saved. Before this, a 640 px, 90-frame GIF started by accident meant sitting through it or reloading the page and losing your layers. Not ideal.
- **Controls and shortcuts.** Press `?`, or use the button under **View**, for a short list of every mouse, touch and keyboard control. Escape, **Close** or a click outside shuts it, and focus goes back where it was. It ignores `?` while you're typing in a control, so it doesn't pop up mid-edit.

It's eight lines long on purpose. If the controls ever need a scrolling help page, the controls are the problem, not the help.

## October 2026, part three: small things that get in the way

A day of actually using it, not adding to it. Every item here is small. Together they make the thing a lot less irritating.

### Better

- **Fits on a phone.** The camera distance was fixed for a landscape screen, so on a phone held upright you only ever saw the middle of the planet. It now pulls back until the globe fits. Desktop is unchanged. Zoom yourself and it leaves you alone. Exports ignore the auto-fit, since their frame is square.
- **Panel in working order.** Map sheet and how it's read come first, then layers and moon, then the globe and display, and finally every save and export together. Moon used to sit between the layers and the projection settings for no reason I can reconstruct.
- **Readable buttons.** The panel text moved to Atkinson Hyperlegible a while back, but the buttons stayed in 10.5 px letter-spaced mono capitals. They match now.
- **Drop onto a layer.** Drop a file on a layer's button, or the moon's, and it loads there. Previously every drop became the base map, so dropping clouds on the clouds button replaced the world.
- **Settings are remembered** between visits, in this browser only. It's the same data as project JSON. Images are never stored.
- **No exporting nothing.** GIF and video now wait for a map, like PNG always did. A hint says so.

### Fixed

- The **Save settings as project JSON** button had no styling at all. It was missing from a CSS selector list and showed up as a grey browser default.
- Loading a layer wrote "Reading labels.png…" into the *base map's* button and left it there. Decode errors for layers landed there too. Messages now go to the button you used.

## October 2026, part two: crop and pad

### New

**Crop and pad to 2:1.** Equirectangular sheets that aren't 2:1 now get **Pad to 2:1** (too wide) or **Crop to 2:1** (too tall) right under the advice box. Padding paints the new bands with your polar cap treatment. Cropping has a slider for which rows to keep. **Use original** undoes it, and **Download the 2:1 sheet** saves the result.

Under the bonnet, Globify now keeps every original image and derives the fixed one from it. Layers get exactly the same crop or pad, so they still line up. They get it even when they're loaded after the fix, which is the bit I'd have forgotten. The setting goes into project JSON as a `fix` field. Older Globify ignores it, so project files stay at version 4.

The advice for odd proportions is also more honest now. A wide equirectangular sheet was already being wrapped correctly with polar caps, so warning that geometry was "doing its best" undersold it a bit.

### Fixed

- **Polar caps: Edge colour and Custom never worked.** The segmented-button helper turned every value into a number, so `'custom'` became `NaN`. The button lit up, the colour pickers never appeared, and every globe quietly got Average caps. It's been like that since the first upload. One `+` in the wrong place. Now fixed; the GIF size and frame buttons do their own number conversion instead.

## October 2026: seams, video and a few things that were simply wrong

### New

**Seam check.** Load a base map and Globify now compares the far-left and far-right columns. It reports a clean match, a slight difference (you might spot a faint line) or a proper mismatch. A mismatch turns the advice box red. It's crude on purpose: it averages colour differences down both edges and that's it. It caught every badly cropped test map I threw at it, and I didn't fancy spending a week on something cleverer that catches maybe two more.

**Video export.** There's a new **Export a full turn as video** button. It uses the browser's built-in MediaRecorder, so it's WebM in Chrome, Edge and Firefox and MP4 where that's all the browser offers. It uses the GIF size and seconds-per-turn settings and records at 30 fps.

It records in real time, though, which is the price of not bundling an encoder. Six seconds of turn means six seconds of waiting. Keep the tab in front. Browsers throttle background tabs without asking.

The GIF and video exporters now share one capture rig instead of each keeping its own copy. Two copies of the same camera-juggling code would have drifted apart within a month. They always do.

### Fixed

- Arrow keys on a focused slider moved the slider *and* spun the planet. Now they only move the slider. Arrow keys still rotate the globe when nothing in the panel has focus.
- Saving a settings project showed a broken-image icon in the download sheet, because everything was previewed as an `<img>`. JSON now gets a plain file note, and video gets a looping player.
- Both README links pointed at `SETUP.md`, which never existed. They now go to `setup-guide.md`.
- The README promised a `.nojekyll` file, but there wasn't one. There is now.
- `setup-guide.md` had a mangled arrow (`â†’`) left over from an encoding accident. Replaced with plain text, along with a couple of typos.
- "Center the globe" is now "Centre the globe". This is a British fork.
- The README's credit to Viruzodro was a broken heading with a typo in it. Fixed, and given its own section, as it should have been.

### Not changed

Settings projects are still version 4. Nothing new needed saving, so old project files load exactly as before.

Still one HTML file, still no build step. I keep saying it'll need modular source eventually. That's still true. It's still not today.
