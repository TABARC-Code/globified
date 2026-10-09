# Changelog

Author: TABARC-Code

Notes on what changed, written down so future me doesn't have to read a 700 kB diff to find out. Newest at the top.

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
