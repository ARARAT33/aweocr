# AWEOCR

AWEOCR is a static, browser-first OCR workspace for images and scanned PDFs. It is designed for GitHub Pages and does not require an AWEOCR backend or account.

## Current capabilities
- Import image files and PDFs (60 MB per file limit in the current UI).
- Local image preview and PDF page rendering with PDF.js.
- OCR with Tesseract.js using **Fast**, **Best**, and **Deep** profiles.
- Language choices: automatic multilingual best effort, Armenian (`hye`), English (`eng`), Russian (`rus`), and combinations.
- Editable text, word/character counts, clipboard, TXT export, Word-compatible HTML-in-DOC export, JSON project export, print/save PDF, native device share, and email draft.
- Browser-local drafts, responsive UI, customizable accent/font size, PWA shell caching.
- Optional Google Drive upload using the visitor's own OAuth client configuration.

## Publish on GitHub Pages
1. Open repository **Settings → Pages**.
2. Choose **GitHub Actions** as the source (the included workflow publishes the repository root).
3. Push to `main` or `master`, or run the workflow manually from **Actions**.
4. Wait for the deployment and open the URL shown by GitHub Pages.

## Optional Google Drive setup
Drive upload needs a Google OAuth setup owned by the site operator; static hosting alone cannot upload to a user's Drive anonymously.
1. In Google Cloud Console, enable **Google Drive API**.
2. Configure the OAuth consent screen.
3. Create an OAuth **Web application** client ID.
4. Add the exact site origin (for example, `https://OWNER.github.io` or the project's GitHub Pages URL/custom domain) to **Authorized JavaScript origins**.
5. Enter the **Client ID only** in AWEOCR → Export & share. Never expose a client secret in a static website.
6. Users authorize access; the app requests the narrow `drive.file` scope and uploads the recognized text as a .txt file.

A client ID is public configuration, not a secret. Google may restrict access to test users while the consent screen is in testing.

## Important limitations
- AWEOCR does not upload source files to an AWEOCR server. If you choose Drive/share/email, content goes to that provider or recipient by your explicit action.
- First-time OCR requires internet to download Tesseract.js, PDF.js and language data. The service worker caches the app shell only, **not all OCR/PDF libraries or trained models**. Full offline OCR is not guaranteed yet.
- Automatic mode currently uses the English + Armenian + Russian model combination; it is not a true language detector and may use more memory.
- .doc is a Word-compatible HTML document, not a full OOXML DOCX. PDF export uses the browser print dialog. Searchable PDF generation is not implemented yet.
- Deep mode uses basic threshold preprocessing. Handwriting and poor-quality scans may produce weak results.
- Browser storage can be cleared by the browser. Keep backups of important files.
- Imported files are treated as untrusted input; the app accepts images/PDFs only and does not execute imported content.

## Architecture
Static HTML/CSS/JavaScript; no build step, server, database, or public API key required. Third-party OCR/PDF libraries are loaded from public CDNs.
