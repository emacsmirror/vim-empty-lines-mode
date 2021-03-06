vim-empty-lines-mode
====================

**NOTE** (2016-02-19): Maintaining this package turned out to be more of a
pain than expected due to a multitude of edge-cases caused by other
functions and modes that assume that an end-of-file overlay is empty
(or at least constant in size). Sadly, I could never find the time or
motivation to put that much work into getting this package to play
nicely with others, so development has ceased.

Vim-like empty line indicator at end of files for Emacs.
This mode emulates the way that vim indicates the end of a file,
that is by putting a "tilde" character (`~`) on any empty line that
follows the end of file.

Emacs provides a similar functionality that inserts a bitmap in the
fringe on the extraneous lines. By customizing
`indicate-empty-lines` and `fringe-indicator-alist` it is possible
to nearly emulate the vim behaviour.

However, there is a slight difference in what the two editors
consider to be "empty lines".

A line in vim can be considered to contain the newline character
that ended the previous line, as well as the other visible
characters.  Equivalently, a line is empty only if it contains no
text and the previous line has no trailing whitespace.

Example:

    foo     <- not empty
    \nbar   <- not empty
    \n      <- not empty
    ~       <- empty
    ~       <- empty
   
    foo     <- not empty
    \nbar   <- not empty
    ~       <- empty
    ~       <- empty

A line in emacs, on the other hand, will contain the newline
character that breaks it. Thus a line is empty even if the previous
line has a trailing linebreak.

Example:

    foo\n    <- not empty
    bar\n    <- not empty
    ~        <- empty
    ~        <- empty
   
    foo\n    <- not empty
    bar      <- not empty
    ~        <- empty
    ~        <- empty

Note that Emacs displays the two cases identically!

There is currently (as of Emacs 24.4) no way to implement the
vim-like behaviour for `indicate-empty-lines` short of modifying
the Emacs core.

This module emulates the vim-like behaviour using a different
approach, namely by inserting at the end of the buffer a read-only
overlay containing the indicators for the empty lines. This has the
added advantage that it's trivial to customize the indicator to an
arbitrary string, and customize its text properties.

Initialization
==============

To enable `vim-empty-lines-mode' in a buffer, run
```elisp
(vim-empty-lines-mode)
```
To enable it globally, run
```elisp
(global-vim-empty-lines-mode)
```
To automatically enable it only for certain buffers, set it to run
within a hook. For example, to enable it only for programming modes,
run
```elisp
(add-hook 'prog-mode-hook 'vim-empty-lines-mode)
```
You can also choose to selectively disable it for certain modes. If
it is initialized with
```elisp
(global-vim-empty-lines-mode)
(add-hook 'text-mode-hook (lambda () (vim-empty-lines-mode -1)))
```
it will be enabled in all modes except those derived from `text-mode`.

Customization
=============

The string that indicates an empty line can be customized, e.g.

    (setq vim-empty-lines-indicator "**********************")

The face that is used to display the indicators is `vim-empty-lines-face`.
