---
title: "LaTeX Template for Academic Theses"
date: 2026-07-23
tags: [latex, thesis, template]
published: true
last_modified_at: 2026-07-30
---

I wrote a LaTeX template for a typical academic thesis and published it on GitHub:

[https://github.com/christoph-otte/latex-template-thesis](https://github.com/christoph-otte/latex-template-thesis)

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

The template is built upon the KOMA-Script class `scrreprt`. Packages, styling, and custom commands are encapsulated within a separate `preamble.tex` file. The document content is logically split across three main files:

- The `frontmatter` includes preliminary pages such as the title page, bilingual abstracts, table of contents, and lists of figures and tables.
- The `mainmatter` contains the primary body of the work, comprising the main chapters and the bibliography.
- The `backmatter` concludes the document with the appendix and the declaration of authorship.

Every folder includes a file with the same name, e.g. `frontmatter.tex` lies in `frontmatter/` and contains the other files like `titlepage.tex`, `abstract.tex`, and `acknowledgement.tex`. The folders `figures/` and `tables/` contains image files and LaTeX code for tables, respectively.

Please make sure to compile `main.tex` with `LuaLaTeX` and `biber` for the bibliography. Standard `pdfLaTeX` will not work properly.

