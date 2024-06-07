# tmux

## Configuration

```conf
## Tmux configuration

# Splitting
unbind %
bind - split-window -v
bind = split-window -h
```

## Key bindings

| Task                | Key             |
| :---                | :---            |
| New window          | `c`             |
| Go to window        | number          |
| New horizontal pane | `-`             |
| New vertical pane   | `=`             |
| Go to other pane    | arrow keys      |
| Toggle full screen  | `z`             |
| Scroll up/down (*)  | `PgUp` / `PgDn` |
| Detach              | `d`             |
| Reload config       | `r`             |

(*) Exit scroll mode with `C-c`

## Command line options


| Task              | Command                    |
| :---              | :---                       |
| List sessions     | `tmux ls`                  |
| Attach to session | `tmux attach -t NAME`       |
| Kill session      | `tmux kill-session -t NAME` |

## References

- Glenn Goodrich, [Tmux: a Simple Start](https://www.sitepoint.com/tmux-a-simple-start/)
- Steven De Coeyer, [Verslag Techtalk - Vim en tmux](https://www.openminds.be/nl/blog/detail/verslag-techtak-vim-en-tmux)
