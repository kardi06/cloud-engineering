# Linux 101 - Notes & Cheatsheet

> Part of the **Cloud Engineer/Architect** learning roadmap
> Week 1: Linux Foundations for Cloud

---

## Folder Contents

| File | Description |
|------|-------------|
| [command-cheatsheet.md](./command-cheatsheet.md) | Complete Linux command cheatsheet (navigation, permissions, file management) |
| [vim-nano-cheatsheet.md](./vim-nano-cheatsheet.md) | Essential shortcuts for Vim & Nano |
| [permissions-guide.md](./permissions-guide.md) | Guide to chmod, chown, octal notation, users & groups |
| [lab-exercises.md](./lab-exercises.md) | *(coming soon)* Lab practice documentation |

---

## Learning Goals This Week

- [x] Install Ubuntu Server on VirtualBox
- [x] Directory navigation: `ls`, `cd`, `mkdir`, `rm`, `cp`, `mv`
- [x] File permissions: `chmod`, `chown`, user groups
- [x] Text editors: basics of `nano` and `vim`

---

## Quick Reference

### Directory Navigation

```bash
pwd                  # Print current directory
ls -la               # List all files including hidden + details
cd ~                 # Go to home directory
cd -                 # Go back to previous directory
mkdir -p a/b/c       # Create nested folders at once
cp -r src/ dst/      # Copy folder with its contents
mv file.txt /tmp/    # Move / rename file
rm -rf folder/       # Delete folder (CAUTION: cannot be undone!)
```

### File Permissions

```bash
chmod 755 file       # rwxr-xr-x (owner: full, group/other: read+execute)
chmod 644 file       # rw-r--r-- (owner: read+write, group/other: read only)
chmod 600 file       # rw------- (owner only — required for SSH keys!)
chown user:group file        # Change owner and group
chown -R user:group folder/  # Recursive for all folder contents
```

**How to read permissions:** `-rwxr-xr--`

```
- rwx r-x r--
|  |   |   └── other: read only
|  |   └────── group: read + execute
|  └────────── owner: read + write + execute
└────────────── type: - = file, d = directory, l = symlink
```

### Vim — Essential Commands

```
i         → Insert mode (start typing)
Esc       → Return to Normal mode
:w        → Save
:q        → Quit
:wq       → Save and quit
:q!       → Force quit without saving (emergency!)
dd        → Delete 1 line
yy        → Copy 1 line
p         → Paste
u         → Undo
/word     → Search for "word"
n         → Jump to next search result
:set nu   → Show line numbers
```

### Nano — Essential Commands

```
Ctrl+O → Save (then Enter to confirm filename)
Ctrl+X → Exit
Ctrl+W → Search
Ctrl+K → Cut line
Ctrl+U → Paste
Ctrl+G → Help
```

---

## Tips & Gotchas

- `rm -rf` **has no Recycle Bin** — double-check before executing
- SSH keys **must** have permission `600`, otherwise the server will reject them
- Stuck in Vim? Press `Esc` then type `:q!` to force quit
- `sudo` is required for commands that need root access

---

## Roadmap Progress

```
Week 1  ✅  Linux Basics (you are here)
Week 2  ⬜  Networking & SSH
Week 3  ⬜  Bash Scripting
Week 4  ⬜  Docker & Containers
...
```

---

*This repo is a hands-on Cloud Engineering learning journal — built through practice, not just copy-paste.*
