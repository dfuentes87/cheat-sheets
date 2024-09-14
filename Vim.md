# Vim Cheat sheet

Commands work in normal mode, unless specified explicitly.

## General

| Key        | Action                                                                        |
| :---       | :-----                                                                        |
| `C-r`      | Redo (undo undo)                                                              |
| `:!CMD`    | Shell command                                                                 |

## Movement

| Key          | Action                                             |
| :---         | :---                                               |
| `0` / `$`    | Begin/end of line                                  |
| `w`,  `W`    | start of next word (`W` ignores punctuation)       |
| `e`,  `E`    | end of next word (`E` ignores punctuation)         |
| `b`,  `B`    | backwards by word (`B` ignores punctuation)        |

## Editing tricks

| Key               | Action                                      |
| :-----------      | :-----------------------                    |
| `%s/OLD/NEW/gc`   | Interactive search/replace                  |
| `%g/PATTERN/d`    | Delete lines matching PATTERN               |

## Split screen

| Key           | Action                   |
| :-----------  | :----------------------- |
| `C-w s`       | horizontal split         |
| `C-w v`       | vertical split           |
| `C-w q`       | close split              |
| `C-w up/down` | switch split             |
| `C-w C-w`     | cycle splits             |
