# tmux

## Configuration

Your tmux configuration file should be named .tmux.conf and stored in your home directory.

```conf
# switch panes using Alt-arrow without prefix
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# Enable mouse control (clickable windows, panes, resizable panes)
set -g mouse on

# Start with window 1 (instead of 0)
set -g base-index 1

# Don't rename windows automatically
set -g allow-rename off

# Split panes using | and -
bind | split-window -h
bind - split-window -v
unbind '"'
unbind %
```

After modifications, to refresh running tmux: `tmux source-file ~/.tmux.conf`

## Basic Commands

All commands start with the prefix, which by default is CTRL + B. Below shows *default* action keys.

| Task                          | Key             |
| :---                          | :---            |
| New window                    | `c`             |
| Go to window                  | 0-9             |
| New horizontal pane           | `"`             |
| New vertical pane             | `%`             |
| Move to the (n)ext window     | `n`             |
| Move to the (p)revious window | `p`             |
| Reload config                 | `r`             |

## Command line options

| Task              | Command                     |
| :---              | :---                        |
| List sessions     | `tmux new -s NAME`          |
| Attach to session | `tmux attach -t NAME`       |
| Kill session      | `tmux kill-session -t NAME` |

## References

- [Tmux Cheat Sheet & Quick Reference](https://tmuxcheatsheet.com/)
