# Testing LaTeX

On Bash, do `latex` to open an interactive session.

In the session, do:

```latex
** \show\thinspace

  > \thinspace=macro:
  ->\kern .16667em .

? <Press Enter>

* \show\TeX

  > \TeX=macro:
  ->T\kern -.1667em\lower .5ex\hbox {E}\kern -.125emX\@.

? <Press Enter>

* <Press Ctrl-D>
```

This is how you discover the inner workings of any *control sequence*.

The above control sequences are non-primitive.

# Primitives Control Sequences

TeX primitives don't show any inner workings.

```latex
** \show\kern

  <*> \show\thinspace

? <Press Enter>

* \show\hbox

  > \hbox=\hbox.

? <Press Enter>

* <Press Ctrl-D>
```

# Digging into LaTeX source

In general, `\show\<command-name>` will do the job.

However, some commands are protected. You'll have to use `\expandafter` to show it.

```latex
** \show\textsuperscript

  > \textsuperscript=macro:
  ->\protect \textsuperscript  .

? <Press Enter>

* \expandafter\show\csname textsuperscript \endcsname

  > \textsuperscript =macro:
  #1->\@textsuperscript {\selectfont #1}.

? <Press Enter>

* <Press Ctrl-D>
```

To show commands that contain the `@` symbol, you need to escape the symbol. LaTeX uses that symbol for special purposes. Here's how you can escape the symbol:

```latex
** \makeatletter\show\textsuperscript\makeatother

  > \@textsuperscript=macro:
  #1->{\m@th \ensuremath {^{\mbox {\fontsize \sf@size \z@ #1}}}}.

? <Press Enter>

* <Press Ctrl-D>
```

## Source Code

The best way is to look for the LaTeX command definition at the [LaTeX kernel documentation](http://mirror.kku.ac.th/CTAN/macros/latex/base/source2e.pdf).

If you really want to look at the source code file, you'll have to find it.

`kpsewhich <filename>` will let you find the source code file.

Eg, `kpsewhich latex.ltx` will let you find the definition of the `\textsuperscript` macro:

```tex
\DeclareRobustCommand*\textsuperscript[1]{%
  \@textsuperscript{\selectfont#1}}
\def\@textsuperscript#1{%
  {\m@th\ensuremath{^{\mbox{\fontsize\sf@size\z@#1}}}}}
```
