# Run Globify on your own PC

This is the complete local setup guide. There's no installation wizard, no account, no server and no reason to put your maps on the internet just to rotate them.

Globify is a self-contained browser application. Once you have extracted the folder, you open `index.html` in a browser. That is the job done.

## What you need

- A current version of Chrome, Edge, Firefox or Safari.
- A computer with WebGL enabled. Most ordinary PCs have this already.
- The Globify ZIP file.

You do not need GitHub Pages, Git, Node, npm, Python, an extension, or administrator rights.

## First-time setup on Windows

1. Download the Globify ZIP file.
2. Open your **Downloads** folder.
3. Right-click the ZIP and choose **Extract All**.
4. Pick a permanent location. `Documents\Globify` is a sensible one. Avoid leaving it buried in Downloads, where good projects go to become archaeological artefacts.
5. Click **Extract**.
6. Open the new Globify folder.
7. Double-click `index.html`.

Globify should open in your default browser. If your default browser is not current, right-click `index.html`, choose **Open with**, then pick Chrome, Edge or Firefox.

## Make a desktop shortcut

If you are going to use it more than once, make the launch less faffy:

1. In the Globify folder, right-click `index.html`.
2. On Windows 11, choose **Show more options** if needed.
3. Choose **Send to â†’ Desktop (create shortcut)**.
4. Rename the shortcut to `Globify` if Windows gives it a needlessly long name.

From then on, double-click the desktop shortcut. The original `index.html` must stay in its Globify folder, so do not drag it onto the desktop and accidentally separate it from the rest of the project notes.

### On macOS

1. Double-click the ZIP file to extract it.
2. Move the extracted Globify folder to **Documents** or somewhere else you will remember.
3. Open the folder and double-click `index.html`.
4. If macOS asks which app to use, choose Safari, Chrome or Firefox.

You can drag `index.html` into the Dock if you want a shortcut. Keep the extracted folder in place afterwards.

## On Linux

1. Extract the ZIP file using your file manager.
2. Open the extracted folder.
3. Open `index.html` in Firefox, Chromium or another current browser.

Most desktop environments will offer **Open with** from the right-click menu. That is enough.

## If it does not open properly

### The browser shows a blank page or a black globe

Try the current version of Chrome, Edge, Firefox or Safari. The globe needs WebGL, which is the part of the browser that talks to your graphics hardware. If hardware acceleration has been disabled in the browser settings, turn it back on and restart the browser.

### Windows says the file is blocked

Right-click the downloaded ZIP file, choose **Properties**, tick **Unblock** if that option appears, then extract it again. Only do this for a file downloaded from the project source you trust.

### It opens as text or in a code editor

Right-click `index.html`, choose **Open with**, then choose a web browser. The file is HTML, so it wants a browser, not Notepad, Word or an IDE having a speculative moment.

### The typeface looks different when I am offline

Thats expected. The control panel requests Atkinson Hyperlegible from Google Fonts when a connection is available. Offline, it uses your computer's default sans-serif font. The application and map processing still work locally.

### I moved the shortcut and it stopped working

The shortcut points to the original `index.html`. Find the Globify folder, open it from there, then create a fresh shortcut. Do not move the actual file out of its folder.

## Keeping Globify updated

When a newer ZIP arrives, extract it into a new folder such as `Documents\Globify 2`. Open the new `index.html` and make a new shortcut if you need one. Keep an older folder until you have checked the new version opens correctly and your map files load as expected.

Your settings project JSON files and artwork are separate from Globify itself. They arn't changed, uploaded or removed by updating the application.

## Privacy, in plain terms

Globify reads map files in the browser on your PC. It does not upload the maps to an application server because there is no application server. The optional Google Font request is only for the interface typeface. It does not include the map you are working on.
