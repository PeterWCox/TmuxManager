# TmuxManager

TmuxManager is an interactive CLI for finding and managing tmux project scripts under `~/dev`.

It recursively scans the dev folder for tmux-related scripts, shows whether their inferred session is running, and lets you start, stop, restart, attach, inspect logs, or grep captured pane output.

## Install

```bash
git clone git@github.com:PeterWCox/TmuxManager.git
cd TmuxManager
chmod +x tmuxmanager
```

Optional shell convenience:

```bash
ln -s "$PWD/tmuxmanager" /usr/local/bin/tmuxmanager
```

If `/usr/local/bin` is not writable or not on your `PATH`, use any directory that is.

## Usage

```bash
./tmuxmanager
./tmuxmanager list
./tmuxmanager start budget
./tmuxmanager stop budget
./tmuxmanager restart sidequest
./tmuxmanager attach budget
./tmuxmanager logs budget
./tmuxmanager grep "error"
```

Set a different scan root with:

```bash
TMUX_MANAGER_DEV_DIR=/path/to/dev ./tmuxmanager
```

## Session Detection

TmuxManager reads common `SESSION=...` assignments from each script. If it cannot infer a session name, it falls back to `<project-folder>-dev`.

Scripts that support `--no-attach` work best because TmuxManager can start them without taking over the terminal.
