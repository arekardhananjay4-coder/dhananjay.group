# dhananjay.group

A responsive, no-login collection of 142 browser-based tools:

- 24 image tools
- 18 PDF tools
- 24 text tools
- 24 developer tools
- 26 calculators
- 17 converters
- 9 generators (including QR codes)

## Run locally

Requires Node.js 20.19+ or 22+.

```sh
npm ci
npm run dev
```

## Production

```sh
npm run build
```

Upload the contents of `dist/` to a static HTTPS host. `dhananjay-group-website.zip` contains an already-built version. Pages use hash routing, so no special SPA rewrites are required. The production build is a self-contained HTML file with embedded styles, scripts and PDF worker. It works at the domain root, in a subdirectory, or opened directly in a modern browser. Configure your domain's DNS and TLS through your chosen hosting provider. The domain has not been purchased or configured by this project.

Use HTTPS: cryptographic generators, hashing and clipboard access require a secure browser context (localhost also works for development). No backend or API keys are needed.

## Privacy

Tool inputs and files are processed in the browser. No file upload endpoint, analytics, account system or external CDN is used. All JS libraries are bundled. Local storage holds only favorite tool IDs, recent tool IDs, and theme preference. Hosting providers may retain normal HTTP access logs.

## Limits

- File picker limit: 50 MB per file; device memory can impose lower practical limits.
- Image output: up to 40 megapixels, 16,000 px per dimension. Animated images use their first frame. Compression outputs JPEG and flattens transparency.
- PDF editing supports unencrypted PDFs. Digital signatures are invalidated by editing.
- PDF text extraction is not OCR. Scanned documents need a separate OCR solution.
- PDF optimization re-saves object streams; it does not downsample images or guarantee a smaller file.
- PDF text creation and overlays use standard Latin fonts; arbitrary Unicode text needs embedded fonts, not included here.
- Metadata clearing removes standard text fields, not all dates, embedded XMP, or identifying content.
- Financial calculators provide estimates, not investment advice. BMI is an adult screening estimate, not a diagnosis.
- This is a broad practical toolkit, not every possible image/PDF operation. Advanced OCR, background removal, office-to-PDF conversion and password protection are not included or advertised.

## Validation

`tests/report.json` records successful sample-input execution of all 142 tools in Chromium, zero uncaught browser errors, and no horizontal mobile overflow at 390px width. End-to-end UI checks cover QR generation and percentage calculation. These are smoke tests, not an exhaustive guarantee for every input or browser.

To repeat with the dev server running:

```sh
npx playwright install --with-deps chromium
node tests/smoke.mjs
```

## Project files

- `src/main.js`: UI, routing, search, favorites, file input and output handling
- `src/tools.js`: 100 non-image/PDF tools
- `src/files.js`: image and PDF tools
- `src/style.css`: responsive light and dark themes
- `tests/smoke.mjs`: browser smoke suite

Built with Vite, Lucide, pdf-lib, PDF.js, QRCode, JSZip, Marked, DOMPurify and js-yaml. Dependencies retain their respective licenses.

## White-screen compatibility update

The standalone `dhananjay-group.html` embeds all assets, adds Promise.withResolvers compatibility, and displays a startup error rather than an empty page. Tested local-file home, QR and PDF generation, plus home rendering in an opaque sandboxed iframe. `node tests/standalone.mjs` repeats these checks.
