## How to compile this CV (lualatex + latexmk)

Prerequisites: MiKTeX with `lualatex`, `biber`, and `latexmk` installed
(verified with LuaHBTeX 1.24 / MiKTeX 26.2, Biber 2.21, Latexmk 4.88).
Make sure the `lato`, `fontawesome`, and `biblatex` packages are installed
via MiKTeX Console.

Build only the root file `main.tex` — never compile `altacv.cls`,
`left.tex`, `page*sidebar.tex`, or `sections/*.tex` standalone.
Use `lualatex`, not plain `luatex`/`luahbtex` (plain TeX fails with
`! Undefined control sequence` on `\newcommand` / `\documentclass`).

Single-command build from the project root:

```powershell
latexmk -lualatex -bibtex -f -interaction=nonstopmode -synctex=1 main.tex
```

Notes:

* `-bibtex` (not `-biber`) — this Latexmk build rejects `-biber`, but
  `-bibtex` still auto-runs `biber` via `main.bcf`.
* `-f` (force) is required: `altacv.cls` v1.1.4 (2018) reports two
  non-fatal errors on current LaTeX (`\maketitle already defined`,
  `There's no line here to end` at `\makecvheader`), yet still produces
  a correct `main.pdf` (1 page) in `-interaction=nonstopmode`.
* Output is `main.pdf`. Re-running the same command is safe
  (`All targets up-to-date` when nothing changed).
* Clean aux files with `latexmk -c main.tex`.

In Zed, use `task: spawn` and pick `CV: build with lualatex (latexmk)`
(defined in `.zed/tasks.json`).
