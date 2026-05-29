# TmuxManager

TmuxManager is an interactive CLI for finding tmux project scripts under `~/dev`.

It recursively scans the dev folder for tmux-related scripts, shows whether their inferred session is running, shows git branch plus line additions/removals and untracked counts, and lets you start/open, stop, or reload it from the main screen. Press `g` to refresh git data.

Opening or starting a project happens in a new Ghostty window by default, leaving TmuxManager open.

## Install

```bash
git clone git@github.com:PeterWCox/TmuxManager.git
cd TmuxManager
chmod +x tmuxmanager
```

Optional shell convenience:

```bash
alias q="$PWD/tmuxmanager"
```

Add that alias to `~/.zshrc` to launch the manager with `q`.

## Usage

```bash
./tmuxmanager
./tmuxmanager list
./tmuxmanager grep budget "error"
./tmuxmanager start budget
./tmuxmanager stop budget
./tmuxmanager restart sidequest
./tmuxmanager attach budget
./tmuxmanager logs budget
./tmuxmanager grep "error"
```

The default interactive view is an arrow-key picker. Running sessions show a green dot; stopped sessions show a red dot. Sessions that are running but have recent failure-looking pane output show a yellow warning state.

```text
TmuxManager

❯ ● running  budget-dev       Budget/tmux-dev.sh
  ● stopped  sidequest-dev    SideQuest/tmux-dev.sh

↑/↓ move  Enter start/open  s stop  r reload  g git  Esc quit
```

Set a different scan root with:

```bash
TMUX_MANAGER_DEV_DIR=/path/to/dev ./tmuxmanager
```

Attach in the current terminal instead of Ghostty with:

```bash
TMUX_MANAGER_GHOSTTY=0 ./tmuxmanager
```

## Session Detection

TmuxManager reads common `SESSION=...`, `SESSION_NAME=...`, `TMUX_SESSION=...`, and `TMUX_SESSION_NAME=...` assignments from each script. If it cannot infer a session name, it falls back to `<project-folder>-dev`.

Scripts that support `--no-attach` work best because TmuxManager can start them without taking over the terminal.
