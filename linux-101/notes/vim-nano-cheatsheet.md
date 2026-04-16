# Vim & Nano Cheatsheet

> Essential shortcuts for the two most common terminal text editors on Linux servers.

---

## Vim

Vim operates in **modes** — understanding this is the key to using it effectively.

### Modes

| Mode | How to Enter | Purpose |
|------|-------------|---------|
| Normal | `Esc` | Navigate, delete, copy, paste |
| Insert | `i`, `a`, `o` | Type/edit text |
| Visual | `v`, `V`, `Ctrl+v` | Select text |
| Command | `:` | Save, quit, search-replace, settings |

### Entering Insert Mode

```
i       → Insert before cursor
a       → Insert after cursor
I       → Insert at beginning of line
A       → Insert at end of line
o       → Open new line below
O       → Open new line above
```

### Navigation (Normal Mode)

```
h / j / k / l   → Left / Down / Up / Right
w               → Jump forward one word
b               → Jump backward one word
0               → Go to beginning of line
$               → Go to end of line
gg              → Go to first line of file
G               → Go to last line of file
:42             → Go to line 42
Ctrl+d          → Scroll half page down
Ctrl+u          → Scroll half page up
```

### Editing (Normal Mode)

```
dd      → Delete (cut) current line
5dd     → Delete 5 lines
dw      → Delete word
d$      → Delete from cursor to end of line
yy      → Copy (yank) current line
5yy     → Copy 5 lines
p       → Paste below
P       → Paste above
u       → Undo
Ctrl+r  → Redo
x       → Delete character under cursor
r<c>    → Replace character under cursor with <c>
>>      → Indent line
<<      → Unindent line
```

### Search & Replace

```
/pattern        → Search forward
?pattern        → Search backward
n               → Next match
N               → Previous match
:%s/old/new/g   → Replace all in file
:%s/old/new/gc  → Replace all with confirmation
```

### Save & Quit

```
:w              → Save
:q              → Quit
:wq             → Save and quit (also ZZ in Normal mode)
:q!             → Force quit without saving
:w filename     → Save as new file
```

### Useful Settings

```
:set nu         → Show line numbers
:set nonu       → Hide line numbers
:set paste      → Enable paste mode (preserves formatting)
:set nopaste    → Disable paste mode
:syntax on      → Enable syntax highlighting
```

---

## Nano

Nano is straightforward — no modes. Open a file and start typing.

```bash
nano filename       # Open file
nano +42 filename   # Open file at line 42
```

### Core Shortcuts

> In Nano, `^` means `Ctrl` and `M-` means `Alt`.

```
Ctrl+O      → Save (Write Out), then Enter to confirm
Ctrl+X      → Exit (prompts to save if modified)
Ctrl+S      → Save (quick save, no confirmation)
```

### Navigation

```
Ctrl+A      → Go to beginning of line
Ctrl+E      → Go to end of line
Ctrl+Y      → Page up
Ctrl+V      → Page down
Alt+\       → Go to first line of file
Alt+/       → Go to last line of file
Ctrl+_      → Go to specific line number
```

### Editing

```
Ctrl+K      → Cut current line
Alt+6       → Copy current line
Ctrl+U      → Paste
Ctrl+T      → Check spelling (if available)
Alt+U       → Undo
Alt+E       → Redo
```

### Search & Replace

```
Ctrl+W      → Search
Alt+W       → Repeat last search
Ctrl+\      → Search and replace
```

### Help

```
Ctrl+G      → Show help (lists all shortcuts)
```

---

## When to Use Which?

| Scenario | Recommended |
|----------|-------------|
| Quick config edit on a server | **Nano** — simple, no learning curve |
| Editing multiple files or heavy text work | **Vim** — powerful once you learn the basics |
| The server only has `vi` installed | **Vim** — `vi` is almost always available |
| You just need to append one line | **Nano** or `echo "line" >> file` |

---

## Survival Guide

**Stuck in Vim?** Press `Esc` then type `:q!` and hit `Enter`.

**Stuck in Nano?** Press `Ctrl+X`, then `N` to discard changes.

---

*Practice tip: Run `vimtutor` in your terminal for an interactive Vim tutorial built into most Linux systems.*
