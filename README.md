# DHANANJAY — Glass Temple Toolkit (v3.1)

143 free browser-based utilities in a cinematic Hemadpanthi-inspired stone temple experience.

## Smooth + glass update

- Apple-inspired translucent cards, search, panels and segmented category tabs; static gradient lighting instead of animated blur layers.
- Accessible **Glass** and **Dark** switches with spring-style thumb movement and saved preferences. Keyboard activation and reduced-motion CSS supported.
- **Motion / Still view** control on the homepage. Reduced-motion users start with the temple still unless they explicitly saved another preference.
- Smooth profile is now default: single main scene pass, shadows/bloom/cloth/live card renders disabled, fewer particles, lower texture work, static stone geometry batched, bounded pixel density.
- Adaptive resolution now uses elapsed real time rather than the clamped animation clock. Sustained severe GPU pressure at minimum resolution switches to still view with an explanatory notice; it does not overwrite the saved motion preference.
- Scene rendering pauses while tools are open; hidden-page work is suspended. Mobile tool cards avoid backdrop blur.
- All 143 tool engines, PDF compressor, offline use, branding and contact details retained.

Advanced comparison: `?smooth=0` enables the heavier authored effects; `?autostill=0` disables only the severe-performance fallback for diagnostics. These are not recommended on slow phones. Rendering speed still depends on the device/browser; no universal frame-rate promise.

New checks: `tests/glass-ui.mjs` (switches, keyboard, mobile navigation/layout, password), `tests/motion-fallback.mjs` (simulated severe GPU pressure and reduced-motion startup), and `tests/performance-final.mjs` (software-rendering diagnostic). All 143 tool-engine sample checks and offline image/QR checks were rerun successfully. The comparative performance report is a software-WebGL diagnostic, not a physical-phone benchmark; its optimized sample predates the automatic still-view safeguard and has that safeguard disabled when rerun.

## Deploy the ready-made website

Use `dhananjay-group-website.zip` (the deployment ZIP, not the source ZIP).

1. Extract the ZIP.
2. Replace the previously deployed **index.html** with this ZIP's **index.html**.
3. Commit/push to the production branch of your connected repository.
4. Wait for the hosting provider's deployment to succeed. Test in a private tab.

The production index is **self-contained**: React, Three.js, fonts, scene images, toolkit code and PDF worker are embedded. You do not need an API, account system, CDN, or backend. The included `landing-pages/` folder preserves the exact upstream reference paths; the customized experience does not request those assets at runtime. Retain the included license notices when distributing the site.

If your host runs a source-code build, upload the updated **source project**, rather than replacing its development HTML with the compiled index. Run `npm ci` and `npm run build`, with output directory **dist**. Never put the deployable compiled HTML into the source project's index.html and then rebuild it.

For a local preview, open the deployable `index.html` directly in a current Chrome, Edge, Firefox, or Safari browser. Some browser/file-viewer wrappers restrict JavaScript, downloads or cryptography; use the actual browser rather than an in-app document preview. HTTPS hosting is recommended. File-mode cryptographic API availability depends on the browser.

## Run or edit the source

Node.js 20.19+ / 22+ recommended.

```sh
npm ci
npm run dev
```

`npm run dev` first prepares both embedded documents, then starts Vite on 0.0.0.0. After editing tool code or scene content, rerun `npm run prepare:site` to regenerate the embedded documents. `npm run build` always regenerates them automatically.

```sh
npm run build
npm run preview
```

## Design implementation and provenance

The base is the exact ThreeUI Kage source, revision:

`c8e06b90397ac246baf0ab6f32f5f6b570acc6fe03c7009f711b579fb72d9f49`

The linked web-served HTML had a different hash. The matching HTML, fonts, Three.js runtime, and all 14 required images were obtained from the MIT-licensed `@designcodeio/threeui@1.2.0` package and verified byte-for-byte. The registered React/TypeScript sources were fetched from the provided source JSON. `node scripts/verify-source.mjs` verifies all 22 source/asset hashes.

- Original canonical HTML and assets are untouched under `public/landing-pages/`.
- The full registered sources, manifest, hashes and license notices are retained under `vendor/threeui-source/`.
- `src/threeui/` uses the exact registered frame, typography and recipe sources. The Kage export is extracted verbatim from registered LandingPages.tsx; unrelated component exports are omitted. Vite aliases `@designcodeio/threeui` to this exact-source entry so it does not silently use a different packaged frame revision.
- `src/scene.jsx` uses KageLandingPage with the requested Onest fonts, weights 400/300, color #e0231c, heading size 46, body size 17 and spacing -0.012.
- `scripts/build-scene.mjs` derives the customized document, changing site copy, branding, contact details, architectural geometry and scene plates without replacing the authored scroll system, shaders, layout variants or interactions.
- `src/temple-geometry.js.txt` implements the requested Hemadpanthi-inspired basalt hall, shikhara, carved stone gateway and diya pedestals. This is a stylized architectural interpretation, not a historical reconstruction of a particular temple.
- New temple plates in `site-assets/` are AI-generated. The Japanese architectural foreground cutouts are disabled in the customized experience. Natural stone/grass layers remain.
- Local srcdoc frames render the full authored document and the existing toolkit. No documentation page or external website is embedded.
- The main 3-D renderer pauses when the toolkit opens. A persistent Open Tools button also works while the scene is loading. If WebGL is unavailable, a temple-still fallback retains navigation and all tools.

## Tools (143)

- 24 image tools
- 19 PDF tools
- 24 text tools
- 24 developer tools
- 26 calculators
- 17 converters
- 9 generators, including QR codes and strong passwords

The previous 142 tool implementations are retained. One new **PDF compressor** is added; the existing **Optimize PDF** tool remains.

### PDF compressor

Choose 72, 96, 120 or 150 DPI presets. Pages are rendered sequentially and encoded as JPEGs in a new PDF. This is most useful for scanned/image-heavy documents. Page dimensions and order are preserved, but selectable text, hyperlinks, forms, accessibility tags and signatures are not retained in the compressed result. This warning appears before processing and on the result. Keep the original.

The tool reports original/result sizes. If the new file is larger, it returns the original unchanged. Reduction is not guaranteed. Browser-safety limits: 200 pages per compression operation, up to 12 megapixels per rendered page.

For lossless re-saving, use **Optimize PDF** instead. It compacts object streams but does not downsample images or guarantee a smaller output.

## Preserved fixes and limits

- Password fields use `form.elements.namedItem`, avoiding the Length property collision. Length 8–256, count 1–100, whole numbers only.
- The image loader uses FileReader and image load events, with a createImageBitmap fallback. It no longer relies on Image.decode.
- HEIC/HEIF decoding depends on native browser support. Otherwise, export to JPG/PNG first; renaming the extension is not conversion.
- 50 MB per file; 40 megapixel image output. Phones can hit memory limits below these caps.
- Animated images use the first frame. JPEG output flattens transparency.
- PDFs must be unencrypted. Text extraction is not OCR. PDF text creation and overlays use Latin-script standard fonts.
- Metadata clearing is not a complete document anonymization tool.
- Financial calculators are estimates; BMI is a general adult screening estimate.
- The tools are broad practical utilities, not every possible image/PDF operation.

## Privacy and contact

Tool inputs and files are processed locally in the browser. There is no file upload endpoint, analytics, or account system. Only favorites, recent tool IDs and display preferences (theme, glass and motion) are stored locally. Files and tool inputs are not sent between the scene and toolkit frames; messages carry only navigation metadata.

Hosting providers may keep ordinary web request logs. A live 3-D scene is more demanding than the prior static homepage; Open Tools gives direct access without requiring completion of the animation. Native rendering speed and decoding vary by device.

- Copyright owner: **Dhananjay Arekar** (year updates at build time).
- Contact: **djrts369@gmail.com**
- Instagram: **https://www.instagram.com/_dr.dhananjay/**
- Underlying ThreeUI work remains copyright Meng To and MIT-licensed. Other libraries/fonts retain their own licenses.

## Validation

The v3 Chromium checks include:

- `tests/smoke-v3.mjs`: valid sample execution of all 143 tool engines, desktop/mobile toolkit layout, QR and calculator UI.
- `tests/compression.mjs`: actual image-heavy PDF compression, parseable output, page dimensions, and file-processing UI. The synthetic fixture shrank from 6,306,987 to 144,324 bytes; this is an example, not a promised reduction.
- `tests/temple-preview.mjs`: real WebGL scene, nine-letter wordmark, temple geometry, toolkit navigation and password generation.
- `tests/temple-mobile.mjs`: mobile-emulated WebGL scene, menu, compressor navigation, embedded password generation, no horizontal overflow.
- `tests/deployed-mobile-tools.mjs`: standalone file in offline mode, image loading with Image.decode forced to fail, JPEG download, QR generation, no-WebGL fallback, contact and copyright.

Install Playwright with `npx playwright install --with-deps chromium`. Source tests expect Vite on port 5173 and a production preview on port 4173. The standalone test expects a copy of the deployable HTML at `/home/user/dhananjay-group.html` (adjust the path outside this workspace).

These are sample-input/browser smoke tests, not an exhaustive certification or physical-device iOS/Android testing. The WebGL software-rendering tests use lower-resolution query controls from the authored scene. The default scene uses the lighter smooth profile and wall-clock adaptive quality. Very slow devices fall back to a clearly indicated still view.
