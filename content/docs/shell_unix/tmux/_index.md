---
title: "tmux"
weight: 1
# bookFlatSection: true
# bookToc: false
# bookHidden: true
# bookCollapseSection: true
# bookComments: false
# bookSearchExclude: false
---
# tmux

Terminal Multiplexer.

## Managing sessions
| Command                             |                                                     |
|-------------------------------------|-----------------------------------------------------|
| `tmux new -s session_name`          | Create a new session with the name `session_name`   |
| `C-b d`                             | Detach from the current session                     |   
| `tmux ls`                           | List all sessions                                   |
| `tmux attach -t session_name`       | Attach to a session with the name `session_name`    |
| `tmux kill-session -t session_name` | Kill a session with the name `session_name`         |
| `tmux kill-server`                  | Kill all the server and all tmux sessions           |

## Managing windows
| Command                                         |                                                   |
|-------------------------------------------------|---------------------------------------------------|
| `tmux new-window -n window_name`                | Create a new window with the name `window_name`   |
| `C-b c`                                         | Create a new window                               |
| `C-b w`                                         | List all windows                                  |
| `C-b 0`                                         | Switch to window 0                                |
| `C-b n`                                         | Switch to the next window                         |
| `C-b p`                                         | Switch to the previous window                     |
| `tmux select-window -t window_name`             | Select a window with the name `window_name`       |
| `C-b ,` or `tmux rename-window new_window_name` | Rename the current window to `new_window_name`    |
| `tmux kill-window -t window_name`               | Kill a window with the name `window_name`         |

## Managing panes
| Shortcut |                                                            |
|----------|------------------------------------------------------------|
| `C-b %`  | Split the current pane vertically                          |
| `C-b "`  | Split the current pane horizontally                        |
| `C-b q`  | Show pane numbers. Type the number to switch to the pane.  |
| `C-b o`  | Switch to the next pane                                    |
| `C-b x`  | Kill the current pane                                      |
| `C-b z`  | Toggle pane zoom                                           |
| `C-b {`  | Move the current pane to the left                          |
| `C-b }`  | Move the current pane to the right                         |
| `C-b !`  | Break the current pane into a new window                   |


##