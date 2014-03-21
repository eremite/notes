# Notes on vim

## Git grep with word boundaries

(Requires the [fugitive](https://github.com/tpope/vim-fugitive) plugin)

```VimL
:Ggrep -w word app/views/
```

### Install a Pathogen plugin

```bash
git submodule add git://github.com/user/plugin vim/bundle/plugin
git submodule init
vim .gitmodules #add `ignore = untracked` to ignore doc/tags
```

### Update all plugins

```bash
git submodule update --init
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
