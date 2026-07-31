---
title: "LaTeX Template for Academic Theses"
date: 2026-07-23
tags: [latex, thesis, template]
published: true
last_modified_at: 2026-07-31
---

I wrote a LaTeX template for a typical academic thesis and published it on GitHub:

[https://github.com/christoph-otte/latex-template-thesis](https://github.com/christoph-otte/latex-template-thesis)

The template has the following structure:

```bash
.
├── backmatter/
│   ├── appendix
│   ├── backmatter.tex
│   └── declaration_of_authorship.tex
├── bib/
│   └── references.bib
├── figures/
│   └── simmons-titania-sleeping.jpg
├── frontmatter/
│   ├── abstract.tex
│   ├── acknowledgements.tex
│   ├── frontmatter.tex
│   └── titlepage.tex
├── mainmatter/
│   ├── 01_introduction.tex
│   ├── 02_theory.tex
│   ├── 03_methods.tex
│   ├── 04_results.tex
│   ├── 05_discussion.tex
│   └── mainmatter.tex
├── tables/
│   └── tab_shakespeare-plays.tex
├── main.pdf
├── main.tex          # primary entry point
├── Makefile          # automated build script
├── preamble.tex      # packages, styling & custom commands
└── README.md
```

The entry point is the `main.tex` file:

```latex
% main.tex

\documentclass[
  11pt,          % base font size to 12pt
  a4paper,       % uses DIN A4 format
  twoside,       % double-sided layout
  DIV=calc,      % calculates automatically best type area ("Satzspiegel")
  BCOR=0mm,      % binding correction 
  listof=totoc,  % adds lists (figures, tables, etc.) to the table of contents
  openright      % forces new chapters to start on a right-handed page
]{scrreprt}

\input{preamble}

\begin{document}

\input{frontmatter/frontmatter}
\input{mainmatter/mainmatter}
\input{backmatter/backmatter}

\end{document}
```

Packages, styling, and custom commands are encapsulated within a separate `preamble.tex` file. The document content is logically split across three main files:

- The `frontmatter` includes preliminary pages such as the title page, bilingual abstracts, table of contents, and lists of figures and tables.
- The `mainmatter` contains the primary body of the work, comprising the main chapters and the bibliography.
- The `backmatter` concludes the document with the appendix and the declaration of authorship.

Each folder contains a file of the same name (e.g., `frontmatter/frontmatter.tex`), which includes the folder's other files — such as `titlepage.tex`, `abstract.tex`, and `acknowledgements.tex`. The `figures/` and `tables/` folders are exceptions: they simply hold image files and LaTeX table code, respectively, without such a collector file.

References are stored as a `references.bib` file within the `bib/` folder. Splitting this into multiple `.bib` files (e.g., `bib/primary.bib` and `bib/secondary.bib`) is optional but may help if you work with several categories of sources — as is common in the humanities, where primary and secondary sources are typically kept separate.

Please make sure to compile `main.tex` with `LuaLaTeX` and `biber` for the bibliography. Standard `pdfLaTeX` will not work properly.

If you work on a Linux terminal you can use the Makefile instead:

```bash
make         # compile full document
make clean   # removes temporary files
```
