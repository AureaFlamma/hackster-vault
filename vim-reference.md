# VIM REFERENCE #

## Help window

- `:help` or `<F1>` opens the help doc
- `:help [argument]` opens help for a term e.g. `:help w` shows help for the w motion.
- `<C-w>` jumps between help pane and the file pane. 
- `:q` closes the help doc

## Exiting, saving, writing, retriveing

- `:q!` closes nvim and discards changes
- `:w` saves the file
- `:w [filename]` saves as
- `:wq` saves and closes (`:wq!`) does the same thing.
- `:r [filename]` inserts the contents of the file below the cursor line.
` `:r !ls` inserts the output of `ls` below the cursor line.

## commands and motions
`command repeat motion` e.g. `d2w`
- `w` until start of next word
- `W` until start of next WORD
- `iW` whole WORD, cursor anywhere on it.
- `e` until end of current word
- `E` until end of current WORD
- `$` until end of line.
- `0` until beginning of line.
- `gg` until end of first line.
- `G` until end of file.
- `[number]G` moves to specific line

## Moving the cursor
- `<C+g>` displays location and file status
- `repeat motion` without a command moves the cursor (`2w` moves the cursor to the start of the 2nd word forward; `30G` goes to 30th line etc.)

## Replace
- `r<character>` replaces the char under cursor with the character e.g. `rb` replaces the char with `b`
- `R<characters>` replaces multiple chars, starting with the one under cursor.

## Undo
- `u` undoes last command
- `U` undoes all commands on line
- `<C + r>` redoes last undo

## Delete, Yank, Put.
- `x` deletes the character under the cursor.
- `d` deletes until `motion` (`dw` `d2e` `d3$` etc.) 
- `dd` deletes the line (regardless of cursor position). `2dd` etc.
- `y` yanks selection. Can be used with motions + repeats, or with visual selection: `ye`, `y2w`
- `yy` yanks line.

NB: deletion is just yanking + removal.

- `p` puts after the cursor (next line if a line was yanked)
- `P` puts before the cursor (previous line if line was yanked)

## Change
- `c[motion]` deletes to motion, and puts you in insert mode. E.g. `c2e` changes 2 words.  

## Insert mode
- `i` before cursor.
- `a` after cursor.
- `I` beginning of line
- `A` at end of the line.
- `o` inserts line below cursor and starts insert mode.
- `O` inserts line above cursor and starts insert mode.

## Search
- `/` searches forward from cursor e.g. `/[phrase]<Enter>`
- `?` searches backwards from cursor e.g. `/[phrase]<Enter>`
- `n` skips to next hit in the direction of search (forward for `/`, backward for `?`)
- `N` skips to next hit in the opposite direction to search (backwars for `/`, forward for `?`)
- `<C+o>` moves backward in your visited hit history
- `<C+i>` moves forward in your visited hit history  
- `%` moves cursor to matching bracket (cursor must be over a bracket).

## Substitute
substitutes `new` string for `old` string.
- `:s/[old]/[new]` first occurence of `old` in the line.
- `:s/[old]/[new]/g` all occurences of `old` in the line.
- `:#,#s/[old]/[new]/g` all occurences between line #'s.
- `:%s/[old]/[new]` first occurence on all lines in file.
- `:%s/[old]/[new]/g` all occurences in file. (all occurences on a line, for all lines).
- `:%s/[old]/[new]/gc` all occurences in file, asks for confirmation before each subsititution.

## Indenting
- `>>` indents current line
- `<<` unindents current line
- `>ap` indents paragraph
- `>i{` indents inside `{}`
- `>ib` indents inside `()`
- `=ap` auto-indents current paragraph
- `gg=G` auto-indents whole file.

*in VISUAL mode*
- `[repeat]>` indent `repeat` times (e.g. `3>` adds 3 levels of indentation).
- `[repeat]<` unindent

## Comments
- `gcc` toggles current line
- `gcap` toggles current paragraph
- `gci{` toggles inside `{}`
- `gc[repeat][motion]` toggles to motion

*in VISUAL mode*
- `gc` toggles selection

## External commands
- `:![command]` e.g. `:!ls -a`

## Visual mode
- `v` enters the visual mode. You can make selection using cursor keys.
- `v <selection> :w [filename]` writes the selection as file.
- `v <selection> :d` deletes the selection.

## Set options
- `:set [option]` toggles option on. E.g. `:set ic`
- `:set no[option]` toggles option off. E.g. `:set noic`
- `:set inv[option]` toggles option to the opposite to what it was `:set invic`

## Completion
- `<C-d>` shows list of command completions
- `<Tab>` and `<S-Tab>` navigate menu of command completions.

## Settings
- live in `~.config/nvim/init.lua` and can be edited with `:e $MYVIMRC`
