# Woteq CodePicker — Local Runnability Fix

This is a local, runnable copy of the **Woteq CodePicker** web app that you
downloaded from the server.

## What was wrong / fixed

1. **Anti-tamper guards blocked local usage** (in `woteq-picker.js` and an
   inline script in `index.html`):
   - The app refused to run unless the hostname was
     `www.codepicker.woteq.com` / `codepicker.woteq.com`.
   - It refused to run on `file://` (offline) protocol.
   - It wiped the page (`document.body.innerHTML = ''`) and threw an error.
2. **Obfuscated source**: the whole `woteq-picker.js` was minified/obfuscated
   (shuffled string array + hex-key lookups). It has been fully de-obfuscated
   into plain, readable JavaScript (`woteq-picker.js`).
3. **Third-party CDN dependencies** (CodeMirror + tracking script) removed in
   favour of locally hosted assets in `lib/codemirror/`.
4. **Live-traffic-tracking script** (`cdn.livetrafficfeed.com`) removed.

## How to run

Any static file server works. Easiest (Python 3):

```bash
cd /home/sajjad/Downloads/woteq-www-codepicker/runnable
python3 -m http.server 8080
```

Then open:

- http://localhost:8080/ — CodePicker
- http://localhost:8080/how_to_use_codepicker_guide.html — Guide

(Or open `index.html` directly via `file://` — the anti-tamper `file:`
restriction was removed, so it now also works from disk.)

## Browser requirements

- Chrome / Edge (required for the File System Access API used to browse the
  project directory: `window.showDirectoryPicker`).
- The page uses `navigator.clipboard` (available on localhost / HTTPS).

## Project structure

```
runnable/
├── index.html
├── how_to_use_codepicker_guide.html
├── woteq-picker.js          (de-obfuscated, clean logic)
└── lib/codemirror/          (CodeMirror 5.65.15 local assets)
    ├── codemirror.min.css
    ├── codemirror.min.js
    ├── theme/material.min.css
    └── mode/{xml,javascript,css,htmlmixed,python,markdown,clike}/*.min.js
```
