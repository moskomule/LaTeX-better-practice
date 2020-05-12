# LaTeX-better-practice
To avoid re-searching and re-inventing wheels

*Note that the tips here might be better, but not best*

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
    % contens
\end{document}
```

Of course `TYPESET main.tex` is ok, as well as `TYPESET section1.tex`

[More details](https://www.overleaf.com/learn/latex/Multi-file_LaTeX_projects)
