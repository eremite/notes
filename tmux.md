# Notes on tmux

## List keybindings
```bash
:list-keys
:list-keys -t vi-copy
```

## Move window left or right
```bash
:swap-window -t -1
:swap-window -t +1
```

## Choose a paste buffer
```bash
=
```

## Kill server

You'll have to stop running processes by hand.

```bash
:kill-server
```

## What does `M` mean in the pane list?

It is marking the current pane for use by future commands. See `select-pane` in the manfile.

You can unmark it with `prefix m`.
