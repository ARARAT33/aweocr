# AWEOCR

AWEOCR is a static, browser-first document workspace for GitHub Pages. It combines file viewing, lightweight creation tools, OCR, and multilingual translation without an AWEOCR backend or account.

## Workspace

- **View:** open local images, videos, audio, PDFs, plain text, CSV, JSON, HTML and other files. Browser support determines which formats can be previewed. DOCX preview uses Mammoth.js and XLS/XLSX preview uses SheetJS loaded on demand from a CDN; the first worksheet can be exported as CSV. PowerPoint and some proprietary formats can still be downloaded but do not have a full renderer.
- **Create:** write and export text documents, HTML pages, JSON, PNG drawings, and CSV spreadsheets that open in Excel-compatible spreadsheet software.
- **OCR:** recognize images and scanned PDFs with Tesseract.js; edit results; copy text; export TXT, editable DOCX, print/save PDF, and PDF page images as a ZIP. Text-based PDFs use their embedded text layer where available; scanned pages use OCR.
- **Language models:** choose from the Tesseract language-code list. Each selected model is downloaded on demand from the upstream CDN; not every model is preloaded. A manually selected language is recommended for reliable results. Automatic mode only compares Armenian, English, and Russian and is not universal language detection.
- **Recognition profiles:** Fast, Balanced, Best, and Deep image cleanup. Select one language for the final OCR pass. Automatic mode tests Armenian, English, and Russian separately to choose one likely language, then performs the final recognition with that single language. Detection is heuristic and should be reviewed.
- **In-page translation:** recognized or manually edited text can be translated into the available target-language list in the OCR and Translate pages. It uses a public, unofficial Google translation endpoint, requires internet access, and can fail or change without notice.
- **Link:** try to fetch a public image, PDF, HTML, or text URL directly in the browser. The remote host must permit cross-origin requests (CORS). This does not bypass authentication, paywalls, or website restrictions.
- **Document conversion:** PDF pages can be exported as PNG images, and extracted/OCR text can be exported to an editable DOCX file. DOCX export preserves text and line breaks, not the original PDF page layout, fonts, columns, or tables. A static browser app cannot promise perfect conversion for every proprietary file format.
- **Responsive layout:** persistent left navigation on desktop and a bottom navigation bar on mobile, light theme by default, optional dark theme, device-aware recommended file limits, local draft storage, and a PWA app shell.

## Device-aware file limits

The interface recommends **30 MB per file on mobile** and **100 MB on desktop** by default, with an optional manual setting. These are application guardrails, not a guarantee that every file of that size can be processed: actual limits depend on browser memory, file dimensions, PDF page count, and the device. Large PDFs are limited to a batch of pages for memory safety.

## Publish on GitHub Pages

1. Open repository **Settings → Pages**.
2. Choose **GitHub Actions** as the source.
3. Push to `main` or `master`, or run the workflow manually from **Actions**.
4. Wait for a successful deployment and open the URL shown by GitHub Pages.

Expected project URL: `https://ararat33.github.io/aweocr/` (only after Pages is enabled and the deployment workflow succeeds).

## Important limitations

- The app is static and processes selected files in the visitor's browser. When using Google Translate, email, sharing, or another external provider, content is sent to that provider only as part of the chosen action.
- First-time OCR needs internet access to load Tesseract.js, PDF.js, and requested language data. The service worker caches the app shell only; full offline OCR is not guaranteed.
- Tesseract model availability, download size, and recognition quality vary by language. The app does not bundle all trained data up front.
- The language list is a broad list of Tesseract model codes, but availability is determined by the upstream model repository. Automatic detection is heuristic and must be checked by the user.
- Google Translate opens in a separate tab; AWEOCR does not provide an in-page translation engine or guarantee Google Translate availability for every language pair.
- The Word export is Word-compatible HTML with a `.doc` extension, not a true OOXML `.docx`. PDF export uses the browser print dialog; searchable PDF output is not implemented.
- Full rich Office editing and rendering for DOCX, XLSX, and PPTX are not bundled. Create currently exports text, HTML, JSON, PNG, and CSV formats.
- Browser storage can be cleared. Save important work to a downloaded file.

## Architecture

Static HTML/CSS/JavaScript; no build step, backend, database, or public API key required. Tesseract.js and PDF.js are loaded from public CDNs.


## Quality and format notes

OCR quality depends on image clarity, rotation, font, layout, and the selected language model. For multilingual or mixed-language pages, choose the main language manually; a single-language OCR pass may miss words in other languages. Review the output before relying on it. This project has not been verified against a representative test set for every language or file format, so it should not be described as perfectly accurate or error-free.
