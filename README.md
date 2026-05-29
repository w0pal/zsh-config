# w0pal's zsh config

This is my small, boring-on-purpose zsh setup.

It is not Oh My Zsh, Prezto, Starship, Powerlevel10k, or a plugin-manager setup. I like those projects, but this repo is my attempt to keep the shell understandable: plain files, plain zsh, and only a few external tools when they earn their place.

## What I Care About

- **Vanilla first**: zsh should still work when optional tools are missing.
- **Evidence over magic**: if a behavior exists, I want to know which file owns it.
- **Container-aware shell**: Distrobox/Podman sessions should be visible without hiding the host identity.
- **Small pieces**: each config file has one job.
- **Low ceremony**: no framework bootstrap just to get a prompt and keybindings.

## Layout

`~/.zshrc` loads these modules from `~/.config/zsh`:

| File          | Purpose                                                       |
| ------------- | ------------------------------------------------------------- |
| `environment` | editor, SDK paths, user-local PATH entries                    |
| `tools`       | optional tool setup, fzf/zoxide integration, native fallbacks |
| `completion`  | zsh completion behavior and prediction toggle                 |
| `history`     | shared history behavior and typo filtering                    |
| `alias`       | daily aliases and repo helpers                                |
| `function`    | small helper functions for editing this config                |
| `keybindings` | movement, history search, completion, prediction keys         |
| `external`    | SDKMAN and other outside init scripts                         |
| `theme`       | prompt, git status, elapsed time, container context           |

## Prompt

The prompt is a two-line prompt that shows:

- user and host
- container name when inside Distrobox/Podman, like `[dev-image]`
- command status
- elapsed command time
- current directory
- git branch and staged/unstaged markers

Example:

```text
w0pal@elitebook [dev-image] √ 13.7s ~/.config/zsh [pure-zsh]+
%
```

The theme started from the prompt style by Christian Ditaputratama and was adapted for my own workflow.

Shout out: <https://github.com/ditatompel/ditatompel/blob/main/machines/p50/home/user/.config/zsh/prompt.zsh>

## Tools

Optional tools are used when available:

- `zoxide` replaces `cd` when installed
- `fzf` is the main interactive history picker
- `eza` backs the `ls` aliases
- `yazi` gets a helper that returns to the selected directory
- `nvm` is lazy-loaded only when needed

When `fzf` is not installed, `Alt+r` falls back to a native zsh history picker. It is not as polished as fzf, but it keeps the shell usable without installing anything extra.

The prompt line also has a lightweight native syntax highlighter using zsh's `region_highlight`, so commands, options, paths, strings, and unknown commands get colored while typing.

## Keybindings

| Key                        | Action                                         |
| -------------------------- | ---------------------------------------------- |
| `Alt+r`                    | history picker, fzf first, native zsh fallback |
| `Ctrl+Right` / `Alt+Right` | forward word                                   |
| `Ctrl+Left` / `Alt+Left`   | backward word                                  |
| `Alt+Backspace`            | delete previous word                           |
| `Up` / `Down`              | history search from current prefix             |
| `Shift+Tab`                | reverse completion menu                        |
| `Ctrl+x Ctrl+p`            | toggle zsh native prediction                   |

## Managing This Repo

This repo is stored as a bare git repo:

```sh
alias zshcfg='git --git-dir=$HOME/.zsh-config.git --work-tree=$HOME'
alias zshlg='lazygit --git-dir=$HOME/.zsh-config.git --work-tree=$HOME'
```

Useful commands:

```sh
zshcfg status
zshcfg add ~/.zshrc ~/.config/zsh README.md
zshcfg commit -m "Update zsh config"
zshcfg push
```

Untracked files are hidden on purpose, because the work tree is `$HOME` and I do not want every random file under `~/.config` showing up in lazygit.

To add a new config file, add it explicitly:

```sh
zshcfg add ~/.config/zsh/new-file
```
