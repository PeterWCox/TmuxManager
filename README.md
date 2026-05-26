# TmuxManager

TmuxManager is an interactive CLI for finding and inspecting tmux project scripts under `~/dev`.

It recursively scans the dev folder for tmux-related scripts, shows whether their inferred session is running, and lets you select one to grep captured pane output, show logs, attach, start, stop, or restart.

Starting, restarting, or attaching opens the tmux session in a new Ghostty window by default.

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

The default interactive view is an arrow-key picker. Running sessions show a green dot; stopped sessions show a red dot.

```text
TmuxManager

❯ ● running  budget-dev       Budget/tmux-dev.sh
  ● stopped  sidequest-dev    SideQuest/tmux-dev.sh

↑/↓ move  Enter select  / grep all  r refresh  q quit
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

TmuxManager reads common `SESSION=...` assignments from each script. If it cannot infer a session name, it falls back to `<project-folder>-dev`.

Scripts that support `--no-attach` work best because TmuxManager can start them without taking over the terminal.
