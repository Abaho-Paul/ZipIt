# ZipIt-web

> Browser-based ZIP compression and extraction. No backend. No installs. No uploads.

![ZipIt-web](https://img.shields.io/badge/client--side-only-brightgreen) ![License](https://img.shields.io/badge/license-MIT-blue) ![No Build](https://img.shields.io/badge/build-none-orange)


**A zero-dependency, zero-server ZIP tool that lives entirely in your browser.**

ZipIt-web does something browsers were never supposed to do well — it compresses and extracts files at native speed, entirely on your device, with no backend, no cloud, and no waiting. Open the page, drop your files, done.

---

## The Idea

Most file tools on the web are a wrapper around a server. You upload, they process, you download. Your files travel the internet twice and sit on someone else's machine in between.

ZipIt-web cuts all of that out. The compression algorithm runs in your browser tab. The ZIP is assembled in memory on your machine. The download is generated locally. Nothing leaves your device — not even a filename.

It is the kind of tool that should have always existed: instant, private, and offline-capable once the page loads.

---

## What It Does

### File Compression

Drop any number of files onto the page — a mix of documents, images, code files, whatever — and ZipIt-web bundles them into a single `.zip` archive. The file is generated in your browser and downloaded automatically. There is no upload progress bar because there is no upload.

### ZIP Preview

Before you extract anything, you can inspect a ZIP file's contents. Drop a `.zip` onto the extract panel and the full file tree renders immediately: every filename, every path, every nested folder. Useful for verifying an archive before you commit to extracting it.

### Extract All

Once you have previewed a ZIP, a single click downloads each file inside it individually. Files are decoded in the browser and handed off to your device one by one, preserving their original names.

### Web Share

On browsers and devices that support the Web Share API — primarily mobile — a share option appears after a ZIP is created. This lets you hand off the generated archive directly to another app without touching the filesystem.

### Light and Dark Mode

The interface ships in dark mode by default and includes a full light theme. Your preference is saved in `localStorage` so the app remembers which mode you were using the next time you open it.

---

## How It Works

ZipIt-web is built on three files and one CDN library.

**JSZip** handles the actual compression and extraction logic. It is loaded from the Cloudflare CDN and provides a clean API for building ZIP archives from raw file data and reading them back out. Everything else in the project is vanilla browser technology.

**`index.html`** defines the page structure: the header, the two-tab layout (Compress and Extract), the drop zones, the file queue, and the action buttons. No templating engine, no component framework — just markup.

**`style.css`** handles theming, layout, and all visual states. CSS custom properties drive the light/dark switch, making theme changes a single attribute toggle on the root element. The layout is fully responsive and adapts from a wide desktop view down to a narrow mobile screen without any JavaScript involvement.

**`app.js`** wires everything together. It manages drag-and-drop events, reads files via the FileReader and File APIs, calls JSZip to build or parse archives, generates object URLs for downloads, and handles the share flow. No build step, no transpilation, no module bundler — the file runs directly in the browser as written.

---

## Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | CSS3 (custom properties, flexbox, grid) |
| Logic | Vanilla JavaScript (ES6+) |
| Compression | JSZip 3.10.1 via CDN |

The absence of a framework is intentional. There is nothing to install, nothing to compile, and no dependency tree to audit. The entire application is four files.

---

## Project Structure

```
ZipIt-web/
├── index.html
├── style.css
├── app.js
└── README.md
```

---

## Running It Locally

Because ZipIt-web is static HTML, running it is as simple as opening a file.

```bash
git clone https://github.com/username/ZipIt-web.git
cd ZipIt-web
open index.html
```

For development with live reload, any static server works:

```bash
# Using Node
npx serve .

# Using Python
python3 -m http.server 8080

# Using VS Code
# Install the Live Server extension and click "Go Live"
```

The app will work identically in all cases. There is no dev server, no hot module replacement, and no build output to worry about.

---

## Deploying to GitHub Pages

ZipIt-web was designed to deploy to GitHub Pages in under two minutes.

```bash
git add .
git commit -m "deploy"
git push origin main
```

Then in your repository on GitHub:

1. Go to **Settings > Pages**
2. Under **Source**, select **Deploy from a branch**
3. Choose **Branch: main** and **Folder: / (root)**
4. Click **Save**

GitHub Pages will publish the site at:

```
https://your-username.github.io/ZipIt-web/
```

No build step. No Actions workflow. The files in the repository root are exactly what gets served.

---

## Browser Support

The core compress and extract functionality works in every modern browser. JSZip has broad compatibility and uses standard typed array APIs available across all current browsers.

The Web Share API is the one exception — it is available on most mobile browsers and Chrome on desktop, but absent in Firefox. ZipIt-web detects support at runtime and only shows the share button when the API is present, so there is no broken UI on unsupported platforms.

Drag-and-drop, `localStorage`, `FileReader`, and `URL.createObjectURL` are universally supported in any browser released in the last several years.

---

## Limitations Worth Knowing

**File size.** Very large files — several hundred megabytes or more — will work but may be slow depending on the device. The browser's JavaScript engine is doing real compression work, constrained by available memory and CPU. For most everyday use cases this is not a concern.

**Password protection.** Encrypted ZIP generation is not part of this release. JSZip has limited and inconsistent support for ZIP encryption across environments, so it was left out of scope for the MVP rather than shipped with unreliable behavior.

**Folder structure.** When compressing files selected via the file picker, nested folder hierarchies are not preserved — the picker API in most browsers returns individual files without their directory context. Files dragged from the OS file manager behave the same way. All selected files land at the root of the generated ZIP.

**Offline use.** Once the page has loaded and JSZip has been fetched from the CDN, the app works without a network connection. But the first load requires the CDN to be reachable. Self-hosting the JSZip file alongside the other project files removes this dependency entirely.

---

## Privacy

ZipIt-web does not collect data, set tracking cookies, or communicate with any server after the initial page load. The files you process never leave your machine. There is no analytics, no telemetry, and no third-party scripts beyond JSZip.

---

<p align="center">
  <strong>ZipIt-web</strong> &mdash; MIT License &mdash; 2024
</p>