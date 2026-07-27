# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

LaTeX source for Henry Schreiner's CV. Two standalone documents, each compiled on its own:

- `HenrySchreinerCV.tex` — the main CV (education, career, grants, publications, open source, presentations).
- `PublicationList.tex` — an older, shorter publication/presentation list. It still shows the University of Cincinnati address, so it is not kept up to date with the main CV.

There is no build script and no CI. Content updates are the usual work here.

## Build

`.latexmkrc` sets `$pdflatex = 'lualatex'` and PDF mode, so LuaLaTeX is used (needed by `moderncv` with the fonts in use). Both files also carry a `% !TeX program = lualatex` line.

```sh
latexmk HenrySchreinerCV.tex      # -> HenrySchreinerCV.pdf
latexmk -c                        # clean aux files
```

Lint with `prek -a --quiet`

PDFs are gitignored; only the sources are committed.

## Conventions in the source

**Conditional content.** `HenrySchreinerCV.tex` uses the `tagging` package. Tags are switched near the top of the preamble:

```latex
%\usetag{details}
%\usetag{refs}
\usetag{physics}
\usetag{cern}
```

`\begin{taggedblock}{details} ... \end{taggedblock}` wraps the long-form entries (extra publications, the full list of smaller open source contributions, more accomplishments). With `details` commented out, those blocks disappear and the CV stays short. Comment or uncomment `\usetag` lines to retarget the CV at an audience instead of deleting content. Content that is retired rather than conditional goes inside `\begin{comment} ... \end{comment}`.

**Section headings** use `\mysection{...}`, a local wrapper that adds negative vertical space before `\section`. Do not call `\section` directly.

**Two entry layouts:**

- Date/description/institution rows use `tabularx` with a fixed-width first column, an `X` middle column, and a bold right column: `\begin{tabularx}{\textwidth}{p{1.1in}X>{\bfseries}r}`. The first column width varies by section (`.5in`, `.8in`, `1.1in`) — match the surrounding section.
- The peer-reviewed publications section instead uses paired `minipage`s (`.06\textwidth` for the bold year, `.94\textwidth` for the entries, with `%` joining them). Entries are grouped one minipage pair per year, newest first, venue in `\textbf`, title in `\emph`, rows separated by `\\`.

Everything is ordered newest first. Escape `#` and `&` as usual, and use `\@` after an abbreviation ending in a capital before a colon (see the grants section) so spacing stays right.

## Commits

Conventional commits. Routine content refreshes have used `chore: update for <period>`.
