# Notes on vim

## Git grep with word boundaries

(Requires the [fugitive](https://github.com/tpope/vim-fugitive) plugin)

```VimL
:Ggrep -w word app/views/
```

## Edit a macro

http://byron.theclarkfamily.name/blog/archive/2007/03/1/

```
Paste it on a blank line: "ap
Edit it
Copy it back: 0"ay$ 
```

## Show full path of current file

```VimL
:echo expand('%:p')
```

## Close all buffers except the current one

```VimL
:bd|b#
```

## Netrw (vim's built-in file browsing) 

`:help netrw-quickmap`

* `%` - New file
* `d` - New directory
* `R` - Rename

## Load all files with a given extension into the quickfix list

http://vim.1045645.n5.nabble.com/quickfix-list-with-files-matching-only-filename-patterns-td1193082.html

```VimL
:cexpr substitute(glob("**/*.csv.haml"), '\n', ':1: &', 'g')
```
