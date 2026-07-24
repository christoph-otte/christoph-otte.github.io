---
title: "LaTeX Template for Academic Theses"
date: 2026-07-23
tags: [latex, thesis, template]
published: true
---

I wrote a LaTeX template for a typical academic thesis and published it on GitHub:

[https://github.com/bernerbruno/latex-template-thesis](https://github.com/bernerbruno/latex-template-thesis)

The template is based on the `scrreprt` class. Packages, styling, and custom commands are defined in separate `preamble.tex` file. Importing the content is splitted among three different files, i.e. `frontmatter`, `mainmatter`, and `backmatter`. The first part `frontmatter` contains the title page, the abstract (in two languages), the table of contents, the list of figures and the list of tables. In `mainmatter`, the authors put the real content of their work, i.e. the chapters and the bibliography. The appendix and the declaration of authorship is located in the last part `backmatter`.

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