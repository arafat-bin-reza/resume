# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Arafat Bin Reza's personal resume, kept in two parallel hand-maintained versions of the same content:

- `index.html` — the web version, published via GitHub Pages at https://arafat-bin-reza.github.io/resume/. A single static page with no build step; styling comes from Tailwind CSS 2.2.19 (jsDelivr CDN) utility classes and icons from Font Awesome 6.6.0 (cdnjs). The profile photo is loaded from a Cloudinary URL.
- `resume.tex` → `resume.pdf` — the printable version. `resume.pdf` is committed, so rebuild it and commit it along with any `.tex` change.

**Keep the two in sync.** A content change (job bullets, skills, dates, projects, and so on) normally has to go into both files. They have the same sections in the same order: Career Summary, Experience, Education, Skills, Certifications, Projects, Training, Languages, Reference. Languages is commented out in both files.

`index.html` is usually the file that gets edited first, and an editor formatter sometimes rewraps its lines. To see only the content that changed, run `git diff -w --word-diff index.html`.

Work is committed straight to `main`, and pushing to `main` updates the live GitHub Pages site.

## LaTeX structure

`resume.tex` is written to look like the HTML page. Its colours are named after the Tailwind classes they copy (`linkblue` = `text-blue-500`, `bodygray` = `text-gray-700`, and so on), and it uses a sans-serif font to match. Content goes through custom macros defined at the top of the file. Use them instead of formatting entries inline:

- `\cvjob{title}{dates}` then an `itemize` list and `\cvskills{...}`: an Experience entry
- `\cventry{title}{body}`: Projects and similar entries
- `\cvref{...}` (6 args): a Reference block
- `\profilepic`: uses `profile.jpg` through `\IfFileExists`, so the build still works if the image is missing

Characters that LaTeX treats as special (`&`, `%`, `#`, `_`) must be escaped in `.tex` content even though they appear unescaped in `index.html`.

## Building the PDF

MiKTeX is installed but **not on PATH**, so call it by its full path. Run it twice, because hyperref needs a second pass:

```
"$LOCALAPPDATA/Programs/MiKTeX/miktex/bin/x64/pdflatex.exe" -enable-installer -interaction=nonstopmode -halt-on-error resume.tex
```

The same directory has `pdftoppm.exe` and `pdftotext.exe`. Use them to render pages to PNG and check the layout, for example to catch overflow onto an extra page. Build artifacts (`*.aux`, `*.log`, `*.out`, …) are gitignored.

## Previewing the HTML

Open `index.html` directly in a browser. There is nothing to build, lint or test.

## Gotchas

- `python` on this machine is the Microsoft Store stub and doesn't work. Don't rely on it for scripting.
- Use the Edit tool for `.tex` changes, not `sed`/`awk`. Backslash escaping in shell edits has silently truncated the file before.
- `plan.md` is a gitignored local notes file. Don't commit it.
