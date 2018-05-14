# Overview

We try to learn LaTeX from [TeX by Topic](https://bitbucket.org/VictorEijkhout/tex-by-topic/downloads/TeXbyTopic.pdf). Start from page 23.

*[TeXBook](http://www.ctex.org/documents/shredder/src/texbook.pdf) is too old.*

For LaTeX commands (non-primitive) refer to the [official LaTeX kernel documentation](http://mirror.kku.ac.th/CTAN/macros/latex/base/source2e.pdf).

For TeX (primitive) commands, refer to the official [TeX Primitive Control Sequences Reference](https://www.tug.org/utilities/plain/cseq.html), in case the "*TeX by Topic" book refers to them.

**NB**: Non-primitive commands are built up with primitive and non-primitive commands.

Never mind the details above. As we learn LaTeX, we'll dive into the details. First, install TeX Live.

# Installing TeX Live (partially)

[TeX Live](https://www.tug.org/texlive/quickinstall.html) is needed to compile LaTeX, **but you do not need the full package** (2GB). On the Mac, you only need [BasicTeX](http://www.tug.org/mactex/morepackages.html) (72MB), and some additional TeX Live packages installed via `sudo tlmgr install <whatever-package>`. On Ubuntu, you only need [texlive-base](https://launchpad.net/ubuntu/+source/texlive-base), and additional packages installed via `tlmgr install <whatever-package>`. Actual examples follow.

## Ubuntu Install

If you're using Ubuntu, do:

    sudo apt-get -y update
    sudo apt-get -y install texlive-base
    sudo apt-get -y install xzdec
    tlmgr init-usertree # Error message here is normal.
    tlmgr update --self # Updates TeX Live Manager itself
    sudo apt-get -y install latexmk
    tlmgr install koma-script

(**NB**: For Ubuntu, your `tlmgr` is running in [usermode](http://www.tug.org/texlive/doc/tlmgr.html#USER-MODE). DO NOT do `sudo`; just do `tlmgr update ...`, `tlmgr install ...`.)

The error message at `tlmgr init-usertree` is normal:

    (running on Debian, switching to user mode!)
    Cannot determine type of tlpdb from /home/<username>/texmf!

## Mac OS Install

If you're using Mac OS, it's best you use [Homebrew](https://brew.sh) and [Homebrew Cask](https://caskroom.github.io) (equivalent to Ubuntu's `apt-get`), in which case you'll be doing:

    brew cask install basictex
    sudo tlmgr update --self # Updates TeX Live Manager itself
    sudo tlmgr install latexmk
    sudo tlmgr install koma-script

**NB**: We'll be using KOMA-script. And we need [`latexmk`](https://ctan.org/pkg/latexmk) to build LaTeX documents (it's a `make` utility for LaTeX).

## Installing TeX Live packages

Each TeX Live package can be installed via `sudo tlmgr install <package-name>`.

If on Ubuntu, omit the `sudo`.

# LaTeX Editor

Atom can be used with these packages:

* [latex](https://atom.io/packages/latex)
* [language-latex](https://atom.io/packages/language-latex)

Atom package *latex* will use `latexmk` to build your latex documents. So you'll need to do `sudo tlmgr install latexmk` to install `latexmk`.

Go to *Settings* for Atom package *latex*, and ensure your "*Output Directory" is `output`.

## Enable Soft Wrap (by default)

In `~/.atom/config.cson`, ensure key `editor` has this property:

```coffeescript
editor: {
  "softWrap": true
}
```

## Shortcuts

In `~/.atom/init.coffee`, add these codes:

```coffeescript
latex_ToggleFontStyle = (style) ->
  return unless editor = atom.workspace.getActiveTextEditor()
  selection = editor.getLastSelection()
  label = switch style
    when 'bold' then 'textbf'
    when 'italic' then 'textit'
    when 'teletype' then 'texttt'
  if selection.isEmpty()
    selection.insertText("\\#{label}{}")
    editor.moveLeft()
  else
    selection.insertText("\\#{label}{#{selection.getText()}}")

atom.commands.add 'atom-text-editor', 'latex:toggle-bold', ->
  latex_ToggleFontStyle('bold');

atom.commands.add 'atom-text-editor', 'latex:toggle-italic', ->
  latex_ToggleFontStyle('italic');

atom.commands.add 'atom-text-editor', 'latex:toggle-teletype', ->
  latex_ToggleFontStyle('teletype');
```

And in `~/.atom/keymap.cson`, add these keybinds:

```coffeescript
"atom-text-editor[data-grammar~=\"latex\"]":
  "ctrl-x f b": "latex:toggle-bold"
  "ctrl-x f i": "latex:toggle-italic"
  "ctrl-x f t": "latex:toggle-teletype"
```

You'll need to restart Atom for changes to `~/.atom/init.coffee` to take effect.
