Copyundefined cut and paste | Vim Tips Wiki | Fandom

**[Tip 312](https://vim.fandom.com/wiki/Copy,_cut_and_paste)**  [Printable](https://vim.fandom.com/wiki/Copy,_cut_and_paste?printable=yes)  [Monobook](https://vim.fandom.com/wiki/Copy,_cut_and_paste?useskin=monobook)  [Previous](https://vim.fandom.com/wiki/VimTip311)  [Next](https://vim.fandom.com/wiki/VimTip313)

**created** August 13, 2002 · **complexity** intermediate · **version** 6.0

* * *

Here is how to cut-and-paste or copy-and-paste text using a visual selection in Vim. See [Cut/copy and paste using visual selection](https://vim.fandom.com/wiki/Cut/copy_and_paste_using_visual_selection) for the main article.

**Cut and paste:**
1. Position the cursor where you want to begin cutting.

2. Press `v` to select characters, or uppercase `V` to select whole lines, or `Ctrl-v` to select rectangular blocks (use `Ctrl-q` if `Ctrl-v` is mapped to paste).

3. Move the cursor to the end of what you want to cut.
4. Press `d` to cut (or `y` to copy).
5. Move to where you would like to paste.
6. Press `P` to paste before the cursor, or `p` to paste after.

**Copy and paste** is performed with the same steps except for step 4 where you would press `y` instead of `d`:

- `d` stands for *delete* in Vim, which in other editors is usually called *cut*
- `y` stands for *yank* in Vim, which in other editors is usually called *copy*

## Contents

[[show](https://vim.fandom.com/wiki/Copy,_cut_and_paste#)]

## Copying and cutting in normal mode[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=1)

In normal mode, one can copy (yank) with `y{motion}`, where `{motion}` is a Vim motion. For example, `yw` copies to the beginning of the next word. Other helpful yanking commands include:

- `yy` or `Y` – yank the current line, including the newline character at the end of the line
- `y$` – yank to the end of the current line (but don't yank the newline character); note that many people like to remap `Y` to `y$` in line with `C` and `D`
- `yiw` – yank the current word (excluding surrounding whitespace)
- `yaw` – yank the current word (including leading or trailing whitespace)
- `ytx` – yank from the current cursor position up to and before the character (til `x`)
- `yfx` – yank from the current cursor position up to and including the character (find `x`)

Cutting can be done using `d{motion}`, including:

- `dd` - cut the current line, including the newline character at the end of the line

To copy into a register, one can use `"{register}` immediately before one of the above commands to copy into the register `{register}`. See [pasting registers](https://vim.fandom.com/wiki/Pasting_registers) for more information on register syntax.

## Pasting in normal mode[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=2)

In normal mode, one can use `p` to paste after the cursor, or `P` to paste before the cursor.

The variants `gp` and `gP` move the cursor after the pasted text, instead of leaving the cursor stationary.

To select a register from which to paste, one can use `"{register}p` to paste from the register `{register}`. See [pasting registers](https://vim.fandom.com/wiki/Pasting_registers).

## Pasting in insert mode[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=3)

The contents of a register can be pasted while in insert mode: type Ctrl-r then a character that identifies the register. For example, Ctrl-r then `"` pastes from the default register, and Ctrl-r then `0` pastes from register zero which holds the text that was most recently yanked (copied). See [pasting registers](https://vim.fandom.com/wiki/Pasting_registers).

## Copying and cutting in command-line mode[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=4)

Command-line mode occurs after typing `:` to enter a command. By default, while in the command line, typing Ctrl-f opens the command-line window where commands can be edited using normal mode. For example, part of one command can be copied then pasted into another command. See [using command-line history](https://vim.fandom.com/wiki/Using_command-line_history).

## Pasting in command-line mode[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=5)

There are two approaches to pasting in command-line mode. The first is to open the command-line window with Ctrl-f, then use normal-mode commands to paste. See the [previous section](https://vim.fandom.com/wiki/Copy,_cut_and_paste#Copying_and_cutting_in_command-line_mode).

The second approach is to type Ctrl-r then a character to paste the contents of the register identified by the character. See [Pasting in insert mode](https://vim.fandom.com/wiki/Copy,_cut_and_paste#Pasting_in_insert_mode) above.

## Copy, cut, and paste from the system clipboard[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=6)

*Main article: [Accessing the system clipboard](https://vim.fandom.com/wiki/Accessing_the_system_clipboard)*

Unlike most text editors, Vim distinguishes between its own registers and the system clipboard. By default, Vim copies to, cuts to, and pastes from its own default register, called the *unnamed register* (`""`, also called [quotequote](http://vimdoc.sourceforge.net/htmldoc/change.html#quote_quote)) instead of the system clipboard.

Assuming Vim was compiled with clipboard access, it is possible to access the `"+` or `"*` registers, which can modify the system clipboard. In this case, one can copy with e.g. `"+y` in visual mode, or `"+y{motion}` in normal mode, and paste with e.g. `"+p`.

If your installation of Vim was not compiled with clipboard support, you must either install a package that has clipboard support, or use an external command such as xclip as an intermediary. See [Accessing the system clipboard](https://vim.fandom.com/wiki/Accessing_the_system_clipboard) for detailed information.

## Multiple copying[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=7)

*Main article: [Pasting registers](https://vim.fandom.com/wiki/Pasting_registers).*

Deleted or copied text is placed in the unnamed register. If wanted, a register can be specified so the text is also copied to the named register. A register is a location in Vim's memory identified with a single letter. A double quote character is used to specify that the next letter typed is the name of a register.

For example, you could select the text `hello` then type `"ay` to copy "hello" to the `a` register. Then you could select the text `world` and type `"by` to copy "world" to the `b` register. After moving the cursor to another location, the text could be pasted: type `"ap` to paste "hello" or `"bp` to paste "world". These commands paste the text after the cursor. Alternatively, type `"aP` or `"bP` to paste before the cursor.

### Windows clipboard[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=8)

When using Vim under Windows, the clipboard can be accessed with the following:

- In step 4, press Shift+Delete to cut or Ctrl+Insert to copy.
- In step 6, press Shift+Insert to paste.

### Different instances[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=9)

How does one copy and paste between two instances of Vim on different Linux consoles?

After copying text, open a new buffer for a new file:
:e ~/dummy

- Paste the text to the new buffer.
- Write the new buffer `:w`.
- Switch to the previous buffer `:bp` to release *.swp.
- Now switch to the other console.
- Put the cursor at the desired place.
- Read the dummy file `:r ~/dummy`

## Increasing the buffer size[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=10)

By default, only the first 50 lines in a register are saved, and a register is not saved if it contains more than 10 kilobytes. [:help 'viminfo'](http://vimdoc.sourceforge.net/cgi-bin/help?tag=%27viminfo%27)

In the example below, the first line displays the current settings, while the second line sets:

- `'100`  [Marks](https://vim.fandom.com/wiki/Using_marks) will be remembered for the last 100 edited files.
- `<100` Limits the number of lines saved for each register to 100 lines; if a register contains more than 100 lines, only the first 100 lines are saved.
- `s20` Limits the maximum size of each item to 20 kilobytes; if a register contains more than 20 kilobytes, the register is not saved.
- `h` Disables [search highlighting](https://vim.fandom.com/wiki/Highlight_all_search_pattern_matches) when Vim starts.

:set viminfo?
:set viminfo='100,<100,s20,h

## See also[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=11)

- [Quick yank and paste](https://vim.fandom.com/wiki/VimTip356)
- [Cut/copy and paste using visual selection](https://vim.fandom.com/wiki/Cut/copy_and_paste_using_visual_selection)
- [In line copy and paste to system clipboard](https://vim.fandom.com/wiki/In_line_copy_and_paste_to_system_clipboard)
- [Accessing the system clipboard](https://vim.fandom.com/wiki/Accessing_the_system_clipboard)

## Comments[![](../_resources/592ebefc7104d681d57852665e9ad514.gif)Edit](https://vim.fandom.com/wiki/Copy,_cut_and_paste?action=edit&section=12)