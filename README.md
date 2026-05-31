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
./tmuxmanager preview budget
./tmuxmanager logs budget
./tmuxmanager grep "error"
```

The default interactive view is an arrow-key picker. Running sessions show a green dot; stopped sessions show a red dot. Sessions that are running but have recent failure-looking pane output show a yellow warning state. Projects with `tmuxmanager.json` port settings show Storybook, frontend, and backend port badges.

```text
TmuxManager

❯ ● running  budget-dev       fe:5173 be:5050           Budget/tmux-dev.sh
  ● stopped  sidequest-dev    sb:6006 fe:3000 be:5000   SideQuest/tmux-dev.sh

Controls
  ↑/↓       move selection
  Enter     start or open selected project
  s         stop selected session
  r         reload selected session
  p         open configured preview
  g         refresh git status
  Esc       quit
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

## Project Port Config

Add `tmuxmanager.json` next to a project's tmux script to customize displayed ports and preview behavior:

```json
{
  "ports": {
    "storybook": 6006,
    "frontend": 5173,
    "backend": 5050
  },
  "preview": "frontend"
}
```

`preview` can be `frontend`, `storybook`, `backend`, or a full URL. Press `p` in the interactive picker, or run `./tmuxmanager preview <project>`, to open the selected preview.

When TmuxManager starts a project, it passes configured ports to the tmux script as CLI arguments:

```bash
./tmux-dev.sh --no-attach --storybook-port 6006 --frontend-port 5173 --backend-port 5050
```

The tmux script should use those arguments to start React, Storybook, .NET, or other services with the desired ports.
