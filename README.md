# LaTeX-better-practice
To avoid re-searching and re-inventing wheels

*Note that the tips here might be better, but not the best*

## Add source codes

Use `minted`.

```latex
\documentclass{article}
\usepackage[newfloat=true]{minted}
\usemintedstyle{monokai}
% To change listing to Algorithm
\SetupFloatingEnvironment{listing}{name=Algorithm}
% for cleveref
\Crefname{listing}{Algorithm}{Algorithms}

\begin{document}

    % block code
    \begin{listing}
        \centering
        \begin{minted}[OPTIONS]{python}

        for i in range(10):
            print(f'hello {i}')

        \end{minted}
    \end{listing}

    % inline code
    \mintinline[OPTIONS]{python}{[print(i) for _ in range(10)]} 


\end{document}
```

To enable minted with TeXPad

* `pip install Pygments`
* `ln -s $(which pygmentize) /Library/TeX/texbin`
* Enable `--shell-escape` in TeXPad's settings.

You can *freeze* minted environments (especially for arXiv) by

```latex
\usepackage[%
finalizecache=true,
cachedir=.
]{minted}
```

then compiling then

```latex
\usepackage[%
frozencache=true,
cachedir=.
]{minted}
```

### Links

* https://www.overleaf.com/learn/latex/Code_Highlighting_with_minted
* http://tug.ctan.org/macros/latex/contrib/minted/minted.pdf


## Use multiple files

Use `subfiles`.


```latex
% main.tex
\documentclass{article}

% shared preambles

% put this line as last as possible
\usepackage{subfiles}

\begin{document}
    \subfile{section1}
\end{document}
```

```latex
%section1.tex
\documentclass[main.tex]{subfiles}
% main.tex's preambles are automatically loaded
\begin{document}
    % content
\end{document}
```

Of course, `TYPESET main.tex` is ok, as well as `TYPESET section1.tex`

### Links

* https://www.overleaf.com/learn/latex/Multi-file_LaTeX_projects


## BibLaTeX

```latex
\documentclass{article}
\usepackage[sortcites,
backend=biber,
hyperref=true,
style=numeric,
language=auto,
babel=hyphen,
maxbibnames=20,
maxcitenames=2,
eprint=false]{biblatex}
\addbibresource{library.bib}

\begin{document}

\clearpage
\printbibliography

\end{document}
```

## Extract used entries from `.bib` files

Suppose the main file name is `main.tex`.

### BibLaTeX

`biber --output_format=bibtex --output_resolve main.bcf -O extracted.bib`

### BibTeX

`bibexport -o extracted.bib main.aux`

## Booktab for high-quality tables

```latex

% \usepackage{booktabs}
\begin{table}
    \centering
    \bagin{tabular}{ccc}
        \toprule
        Foo    & Bar  & XXX   \\ 
        \midrule
        1      & 2    & 3     \\ 
        \cmidrule(r){1-2}
        4      & 5    & 6     \\ 
        \bottomrule
    \end{tabular}

\end{table}
```

## Multiple lines in a table entry

`p{WIDTH}` fixes the width of columns.

```latex
% \usepackage{array}
\newcolumntype{P}[1]{>{\centering\arraybackslash}p{#1}} % default p is to align left.
\begin{tabular}{p{0.5\linewidth}P{0.4\linewidth}}
...

```

## Smart enumerate/itemize and description

```latex
% \usepackage{enumitem}
\begin{enumerate}[label={\roman*},start=2]
    \item Start from 2
\end{enumerate}

\begin{description}[leftmargin=*,itemsep=0pt,topsep=0pt]
    \item [LaTeX] LaTeX is ...
    \item [BibTeX] BibTeX is ...
\end{description}
```

### Links

* http://konoyonohana.blog.fc2.com/blog-entry-58.html

## Wrap Figure/Table

I don't like to use wrapped figures/tables, but sometimes, I need to.

```latex
%\usepackage{wrapfig}
\begin{wrapfigure}[11]{r}{0.4\linewidth}
    \centering
\end{wrapfigure}
```

Here, `11` corresponds to the height in 11 lines and `0.4\linewidth` determins the width.

### Links

* http://konoyonohana.blog.fc2.com/blog-entry-259.html

## Stop using unnecessary \(re)newcommand

For mathematical operators, use

```
\DeclareMathOperator{\ReLU}{ReLU}
```

```
\DeclareMathOperator*{\argmax}{argmax}
```

For paired operators, e.g., norms, use

```
\DeclarePairedDelimiter{\norm}{\lVert}{\rVert}
```

*For `\ReLU`, `\DeclareMathSymbol` may be a better choice.*

### Links

https://www.ctan.org/pkg/mathtools
https://www.ctan.org/pkg/amsmath

## Control margins around captions

```latex
\usepackage{caption}
\captionsetup[table]{skip=5pt}
\captionsetup[figure]{skip=5pt}
```

### Export a pandas DataFrame as a  LaTeX table

```python
df
# model      |  param_size     |   test_accuracy     |
# ----------------------------------------------------
# resnet     |  3M             |   0.9               |
df.to_latex(index=False, formatters=[lambda x: clean_model_names[x], "{:.2f}".format, "{:.2f}".format], header=["Model", "Num Params", "Test acc"])
```

### Figure A. 1 in Appendices

```latex
\appendix
\counterwithin{figure}{section}
\counterwithin{table}{section}
```
