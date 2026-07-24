---
title: "LaTeX Template for Academic Theses"
date: 2026-07-23
tags: [latex, thesis, template]
published: true
---

I've written a LaTeX template for a typical academic thesis and published it on GitHub:
[https://github.com/bernerbruno/latex-template-thesis](https://github.com/bernerbruno/latex-template-thesis)

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