# tmux

## Configuration

Your tmux configuration file should be named .tmux.conf and stored in your home directory.
```conf
# Bind OPTION + Arrow Keys to switch between tmux panes
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D
```
After modifications, to refresh running tmux: `tmux source-file .tmux.conf`

## Basic Commands
All commands start with the prefix, which by default is CTRL + B

| Task                          | Key             |
| :---                          | :---            |
| New window                    | `c`             |
| Go to window                  | number          |
| New horizontal pane           | `"`             |
| New vertical pane             | `%`             |
| Move to the (n)ext window     | n               |
| Move to the (p)revious window | p               |
| Reload config                 | `r`             |

(*) Exit scroll mode with `C-c`

## Command line options

| Task              | Command                     |
| :---              | :---                        |
| List sessions     | `tmux new -s NAME`          |
| Attach to session | `tmux attach -t NAME`       |
| Kill session      | `tmux kill-session -t NAME` |

## References
- [Tmux Cheat Sheet & Quick Reference](https://tmuxcheatsheet.com/)
