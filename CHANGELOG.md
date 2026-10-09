# Changelog

Author: TABARC-Code

Notes on what changed, written down so future me doesn't have to read a 700 kB diff to find out. Newest at the top.

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
