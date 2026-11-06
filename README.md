# Kitaab Saathi (کتاب ساتھی)

Ask questions of your own textbook and get answers with page numbers. It runs fully offline: no account, no server, no API, no `fetch()` calls. Built for **Hackyard Yard #6: Offline**.

Load-shedding or no signal before an exam? Open the app, pick your PDF, ask.

## What it does

- Load any **text-based PDF** textbook. Text is extracted on-device with a bundled copy of pdf.js.
- Ask a question in English, Urdu or Roman Urdu. Retrieval is **BM25** over 3-sentence passages (Unicode-aware tokenizer, so Urdu script works).
- You get the best matching sentence, the top 3 passages with matched words highlighted, and **the page number**.
- **Page N kholo** renders the real PDF page so you can read it in context.
- Books are stored in **IndexedDB** and survive restarts. Install it as a PWA; a cache-first service worker holds the app shell.
- The header badge shows `offline ✓` when the device has no connection.

## Run it

Service workers need `http://localhost` or HTTPS, so serve the folder once:

```bash
python3 -m http.server 8080      # or: npx serve .
```

Open `http://localhost:8080`, let it load once (this caches the shell), then turn on airplane mode. Everything keeps working. Add it to your home screen to use it as an app.

## The airplane-mode test

1. Open the app once while online (caches the shell).
2. Turn off Wi-Fi and data.
3. Reload, load a PDF, ask questions. Check DevTools > Network: zero requests.

## Project layout

```
index.html        UI + indexing + BM25 search (single file, no build step)
sw.js             cache-first service worker
manifest.json     PWA manifest
vendor/           pdf.js 4.10.38 (Apache-2.0), bundled so nothing loads from a CDN
```

## Limits (honest)

- Scanned/image-only PDFs have no text layer; the app tells you and does not OCR.
- Answers are **extractive** (quoted from the book), not generated. That keeps it fast, tiny and always faithful to the source.
- Search is keyword based, so rephrase if nothing is found.

## Roadmap

- Optional local LLM (WebLLM) to rewrite the retrieved passages into a short explanation.
- Offline OCR (Tesseract) for scanned books.
- Quiz mode generated from a chapter.

## Publish to GitHub

```bash
git init && git add . && git commit -m "Kitaab Saathi: offline textbook Q&A"
git branch -M main
git remote add origin https://github.com/<you>/kitaab-saathi.git
git push -u origin main
```

Optional live demo: repo Settings > Pages > Deploy from branch `main` / root.

## License

MIT (app code). pdf.js is Apache-2.0, see `vendor/PDFJS-LICENSE`.
