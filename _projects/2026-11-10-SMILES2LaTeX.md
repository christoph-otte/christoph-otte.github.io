---
title: "SMILES to LaTeX: Drawing Molecular Structures"
date: 2026-11-10
tags: [latex, chemistry, structural formula]
published: true
---

Drawing chemical structure formulae with LaTeX can quickly become frustrating. Using [`chemfig`](https://ctan.org/pkg/chemfig) might be a good choice when you want to draw simple molecules. But learning it may not be worthwhile if you're writing about complex structures.

The [PubChem](https://pubchem.ncbi.nlm.nih.gov/) database provides access to millions of chemical structures, but it doesn't provide the corresponding LaTeX code.

<p align="center">
  <img src="{{ '/assets/images/caffeine.svg' | relative_url }}"
       width="350"
       alt="Chemical structure of caffeine">
</p>

That's why I wrote a small program that converts [SMILES](https://en.wikipedia.org/wiki/Simplified_Molecular_Input_Line_Entry_System) representations into usable chemfig code.

You can find the source code and instructions on [GitHub](https://github.com/christoph-otte/smiles2latex).