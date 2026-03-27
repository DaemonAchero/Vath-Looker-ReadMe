# Vath-Looker

Vath Observer watches a project folder and writes and records short notes, human-readable change notes for coding work.
It is built to stay low-CPU while idle, capture file changes clearly, and help with commit messages, context bundles, and quick project reports.

## Commands

- `vath init` configures the observer and can install shell aliases.
- `vath start` launches the observer in a detached background process.
- `vath stop` stops the observer.
- `vath note "refactoring the database connection"` records a short note in `vath_journal.txt`.
- `vath context --last 10m` builds a Markdown context bundle and copies it to the clipboard when possible.
- `vath structure --style visual` prints the project tree.
- `vath report commit` drafts a commit message from recent journal and file changes.
- `vath publish` runs a git preset from `git_config.json`.
- `vath status` shows whether the observer is running and which folder it is watching.
- `vath --debug` reveals the hidden debug note.

## Project files

The observer keeps its main files beside the folder you are observing:

- `.vath/vath_config.json`
- `.vath/vath_journal.txt`
- `.vath/vath_state.json`
- `.vath/git_config.json`
- `.vath/vath.pid`


The config stores the project root and setup preferences. The journal stores concise human-readable summaries plus structured details for each batch. The state file stores low-CPU tracking data for the hybrid watcher. The git config stores optional batch command presets.

## Watcher behavior

When a file changes:

1. The event is staged in memory.
2. The debounce timer resets to 3 seconds.
3. If more events arrive before the timer expires, the staging window stays open.
4. Once the tree is quiet for 3 seconds, the staged batch is flushed to `vath_journal.txt`.

That keeps rapid save bursts from flooding the log.

## Install

```bash
pip install -U vath-observer
```

## Quick start

```bash
vath init
vath start
vath note "refactoring database connection"
vath context --last 10m
vath structure --style directory
vath report commit
vath publish
```

## Notes

- Clipboard support is best-effort. If it cannot access the clipboard, the context bundle is still printed to the terminal.
- Shell aliases are opt-in and can be written for PowerShell, Bash, or Zsh.
- Git presets are opt-in and stored in `git_config.json`.

