## Installation

```bash
# Add the marketplace
claude plugin marketplace add halfwhey/skills

# Install a plugin
claude plugin install tmux
```

---

## tmux

![explore-top](https://github.com/halfwhey/skills/releases/download/v1.1.0/explore-top.gif)
*Claude teaches you how to use top*

Gives Claude full control over tmux - opening panes, driving TUI applications, reading output, and debugging problems alongside you.

### Why

Claude Code runs inside your terminal but can only see its own process. This plugin breaks that wall: Claude can split panes, run commands, operate full-screen TUI apps (vim, gdb, top, lazygit), read their output, and react to what it sees - all while you watch in real time.

### What makes the reading efficient

Most approaches to reading terminal output dump the entire scrollback every time, burning context window on thousands of lines Claude has already seen. `read-tmux` is smarter:

- **Delta mode** (default) - tracks a position marker. First read captures the visible buffer; subsequent reads return **only new lines** since the last call. If nothing changed, Claude gets `(no new output)` instead of a wall of text.
- **TUI mode** (`--tui`) - for full-screen apps like vim, htop, or gdb. Snapshots the visible screen and diffs against the previous capture. If less than half the screen changed, Claude gets a **unified diff of just the changed regions**. Heavy redraws fall back to the full screen.
- **Output capping** - all output is hard-capped at 4KB (first 2000 + last 2000 bytes). When truncated, the full output is saved to disk and the path is included so Claude can read or grep it if needed.

The result: Claude spends tokens on what actually changed, not on re-reading static content.

### More demos

#### Quitting vim

Claude opens vim, writes text, saves, and exits - the classic "how do I quit vim" solved by an AI.

![quit-vim](https://github.com/halfwhey/skills/releases/download/v1.1.0/quit-vim.gif)

#### Debugging with gdb

Claude runs a buggy C program, reads the source with `bat`, opens gdb, sets breakpoints, steps through code in TUI layout, prints variables to find an off-by-one bug, fixes the source, and re-runs.

![debug-gdb](https://github.com/halfwhey/skills/releases/download/v1.1.0/debug-gdb.gif)

## License

MIT
