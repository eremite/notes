# Notes on vim

## Git grep with word boundaries

(Requires the [fugitive](https://github.com/tpope/vim-fugitive) plugin)

```VimL
:Ggrep -w word app/views/
```

## Pathogen

[Vimcast](http://vimcasts.org/episodes/synchronizing-plugins-with-git-submodules-and-pathogen)

### Install a plugin

```bash
git submodule add git://github.com/user/plugin vim/bundle/plugin
git submodule init
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

