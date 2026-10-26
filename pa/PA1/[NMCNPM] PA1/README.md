# Project Proposal & Plan

LaTeX source export of the PA1 Project Proposal & Plan, split into front matter and sections.

## Build

With a LaTeX distribution and `latexmk` installed, run `latexmk -xelatex main.tex` from this directory (the directory containing `main.tex`).

The submitted PDF is stored at `../T03_PA1.pdf`. It is the previously committed PDF; this source export contains later edits and has not been verified against that PDF by a local build.

## Structure

- `main.tex` loads packages, document-wide styles and each section in order.
- `frontmatter/titlepage.tex` contains the cover page.
- `sections/` contains one file per document section.
- `assets/university-mark.png` is the mark extracted from the provided template.
