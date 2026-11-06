# Curriculum Vitae (one page)

My one-page CV in English and Italian, plus my certificates.
The full version lives in [aleemont1/curriculum-vitae](https://github.com/aleemont1/curriculum-vitae).

**Website:** [aleemont1.github.io/one-page-cv](https://aleemont1.github.io/one-page-cv/)
(language is picked from your browser; switch with the EN/IT toggle)

| | English | Italiano |
|---|---|---|
| PDF | [resume.pdf](https://aleemont1.github.io/one-page-cv/resume.pdf) | [resume-it.pdf](https://aleemont1.github.io/one-page-cv/resume-it.pdf) |
| Short link | [/en/](https://aleemont1.github.io/one-page-cv/en/) | [/it/](https://aleemont1.github.io/one-page-cv/it/) |

## Layout

| File | Purpose |
|---|---|
| `preamble.tex` | Packages, margins and macros shared by every variant |
| `resume.tex` / `resume-it.tex` | Content in English / Italian. **Change both together.** |
| `resume-signed.tex` / `resume-it-signed.tex` | Same content plus the GDPR consent clause, date and signature |
| `signature.tex` | Consent clause and signature block (shared with the long CV) |
| `site/` | Landing page and `/en/`, `/it/` short links deployed to GitHub Pages |

Every variant must fit on **one page**: CI fails the build otherwise.

## Building locally

```sh
xelatex resume.tex          # English
xelatex resume-it.tex       # Italian
xelatex resume-it-signed.tex
```

The signed variants use `signature.png` if it exists in the repo folder and print a blank
signature line otherwise. `signature.png` is git-ignored: never commit it.

## Signed versions

Signed PDFs (GDPR consent clause, date and handwritten signature) are **not** published on the website.
To get them:

1. One-time setup: encrypt your signature scan with `scripts/encrypt-signature.sh path/to/signature.png`,
   commit the resulting `signature.png.gpg`, and add the same passphrase as the repository secret
   `SIGNATURE_PASSPHRASE` (Settings → Secrets and variables → Actions).
2. Run **Actions → Build signed CV → Run workflow**.
3. Download the `signed-cv` artifact (kept for one day) and open `signed-cv.zip` with the same passphrase.
   The zip uses AES-256: use 7-Zip on Windows, Keka or The Unarchiver on macOS, or `7z x` on Linux.

## CI

`.github/workflows/deployLaTeX.yml` compiles all four variants on every push and pull request,
checks the page count, and on `main` deploys the English and Italian PDFs with the landing page
to GitHub Pages. `.github/workflows/signed.yml` builds the signed PDFs on demand (see above);
they are never published to Pages.
