---
title: "LaTeX Template"
date: 2026-08-01
tags: [gravitation, lecture notes]
published: true
---


# LaTeX Template

All manuscripts must be submitted using our official LaTeX template. This ensures consistent formatting and simplifies the editorial process.

---

## Download

> **[Download the LaTeX Template](#)** ← replace with actual link

The template archive contains:

| File | Description |
|---|---|
| `main.tex` | Your manuscript — edit this file |
| `phoebe.cls` | Journal class file — **do not modify** |
| `references.bib` | BibTeX reference database — add your references here |
| `example-figure.pdf` | Example figure for reference |
| `README.md` | Quick start guide |

---

## Requirements

- **LaTeX distribution:** TeX Live 2022 or later / MiKTeX 22 or later
- **Compilation engine:** `pdflatex` + `bibtex`, or `lualatex` + `biber`
  - Recommended compile sequence: `pdflatex → bibtex → pdflatex → pdflatex`
- **Editor:** Any LaTeX editor works. We recommend [Overleaf](https://www.overleaf.com/) for collaborative editing without local installation.

---

## Using the Template

### 1. Fill in the Metadata

At the top of `main.tex`, fill in the document metadata. Leave author fields empty for initial submission (blind review):

```latex
\title{Your Article Title}
\shorttitle{Short Title for Header}   % max. ~50 characters

% BLIND REVIEW: leave author fields empty for initial submission
% \author{Jane Doe}
% \affiliation{University of Example}
% \email{j.doe@example.edu}

\submissiontype{original research}   % or: review / short communication / theoretical
\keywords{keyword one, keyword two, keyword three}
\mscnumber{} % Mathematics Subject Classification (optional, math articles)
```

After acceptance, you will be asked to un-comment and fill in the author information.

### 2. Abstract

Write your abstract inside the `abstract` environment:

```latex
\begin{abstract}
  Your abstract text here. Maximum 250 words. No citations, no 
  undefined abbreviations, no references to figures or tables.
\end{abstract}
```

### 3. Body Text

Use standard LaTeX sectioning:

```latex
\section{Introduction}
\section{Methods}
\section{Results}
\section{Discussion}
\section{Conclusion}
```

For review articles and theoretical articles, use section headings appropriate to your content.

### 4. Figures

Include figures with captions using the `figure` environment:

```latex
\begin{figure}[htbp]
  \centering
  \includegraphics[width=0.8\linewidth]{your-figure.pdf}
  \caption{A description of what is shown. Include axis labels, 
           units, and the key observation. The caption must be 
           self-contained.}
  \label{fig:yourlabel}
\end{figure}
```

**Figure guidelines:**
- Preferred formats: **PDF** (vector) or **PNG/TIFF** at ≥ 300 dpi
- All text in figures (axis labels, legends) must be legible at final print size
- Do not use raster screenshots of vector plots if the source can be exported as PDF
- Color figures are allowed; but ensure the figure is also readable in grayscale

### 5. Tables

```latex
\begin{table}[htbp]
  \centering
  \caption{Caption goes above the table.}
  \label{tab:yourlabel}
  \begin{tabular}{lcc}
    \toprule
    Column 1 & Column 2 & Column 3 \\
    \midrule
    Data     & 1.23     & 4.56     \\
    \bottomrule
  \end{tabular}
\end{table}
```

Use the `booktabs` package (already loaded in the template) — avoid vertical rules.

### 6. Equations

Number all equations that are referenced in the text:

```latex
\begin{equation}
  E = mc^2
  \label{eq:einstein}
\end{equation}

As shown in Eq.~\eqref{eq:einstein}, ...
```

Use `\( \)` for inline math, not `$ $` (both work, but the former is preferred in LaTeX).

### 7. References

Add your references to `references.bib` in BibTeX format. Cite them with `\cite{}`:

```latex
\cite{einstein1905}          % single citation
\cite{einstein1905, bohr1913} % multiple citations
```

Use DOI-based BibTeX entries whenever possible. Tools like **Zotero**, **JabRef**, or **doi2bib.org** can generate BibTeX entries automatically.

**Do not** insert references manually as footnotes or formatted text — always use the BibTeX system.

---

## What Not to Change

The following must not be modified:

- `phoebe.cls` — the journal class file
- Page margins, font sizes, line spacing
- The header/footer design
- The reference style (set by the class file)

If you feel the template is missing a LaTeX package you need (e.g., for chemistry structures, circuit diagrams, musical notation), contact the editors before adding it — some packages conflict with the class file.

---

## Common Errors

| Error | Likely cause |
|---|---|
| `File 'phoebe.cls' not found` | `phoebe.cls` must be in the same directory as `main.tex` |
| References show as `[?]` | Run BibTeX after pdflatex, then compile twice more |
| Figures not found | Check filename spelling and file extension (case-sensitive on Linux) |
| Overfull \hbox warnings | Usually fine; fix only if text visibly overflows the margin |
| Unicode characters not rendering | Use `\usepackage[utf8]{inputenc}` (already included) or switch to LuaLaTeX |

---

## Submitting the Files

Submit the following in a single `.zip` archive:

- `main.tex`
- `references.bib`
- All figure files referenced in the document
- The compiled `main.pdf`

Do **not** include `phoebe.cls` — we already have it. Do **not** include auxiliary files (`.aux`, `.log`, `.bbl`, etc.).
