# Linux Permissions Guide

> Understanding file permissions, ownership, and access control on Linux systems.

---

## How Permissions Work

Every file and directory in Linux has three layers of access control:

```
Owner (u)  →  The user who owns the file
Group (g)  →  The group assigned to the file
Other (o)  →  Everyone else
```

Each layer can have three types of permission:

| Symbol | Permission | On Files | On Directories |
|--------|-----------|----------|----------------|
| `r` | Read | View contents | List contents (`ls`) |
| `w` | Write | Modify contents | Create/delete files inside |
| `x` | Execute | Run as program | Enter directory (`cd`) |

---

## Reading Permission Strings

```bash
ls -l myfile.txt
# -rw-r--r-- 1 kardi devops 1024 Apr 16 10:00 myfile.txt
```

Breaking down `-rw-r--r--`:

```
-   rw-   r--   r--
│    │     │     └── Other: read only
│    │     └──────── Group: read only
│    └────────────── Owner: read + write
└─────────────────── Type: - = file, d = directory, l = symlink
```

---

## chmod — Change Permissions

### Symbolic Mode

Format: `chmod [who][operator][permission] file`

| Who | Meaning |
|-----|---------|
| `u` | Owner (user) |
| `g` | Group |
| `o` | Other |
| `a` | All (u + g + o) |

| Operator | Meaning |
|----------|---------|
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permission |

```bash
chmod u+x script.sh        # Add execute for owner
chmod g-w file.txt          # Remove write from group
chmod o-rwx secret.conf     # Remove all permissions from others
chmod a+r public.txt        # Add read for everyone
chmod u=rwx,g=rx,o= file   # Owner: full, Group: read+exec, Other: none
```

### Octal (Numeric) Mode

Each permission has a numeric value:

```
r = 4
w = 2
x = 1
```

Add them up for each layer:

| Octal | Binary | Permission | Meaning |
|-------|--------|-----------|---------|
| `7` | `111` | `rwx` | Read + Write + Execute |
| `6` | `110` | `rw-` | Read + Write |
| `5` | `101` | `r-x` | Read + Execute |
| `4` | `100` | `r--` | Read only |
| `3` | `011` | `-wx` | Write + Execute |
| `2` | `010` | `-w-` | Write only |
| `1` | `001` | `--x` | Execute only |
| `0` | `000` | `---` | No permission |

### Common Permission Patterns

```bash
chmod 755 script.sh     # rwxr-xr-x  → Executable scripts, public directories
chmod 644 index.html    # rw-r--r--  → Regular files (web, configs)
chmod 600 id_rsa        # rw-------  → SSH private keys (REQUIRED)
chmod 700 .ssh/         # rwx------  → SSH directory (REQUIRED)
chmod 664 shared.txt    # rw-rw-r-- → Shared file within a group
chmod 777 file          # rwxrwxrwx  → AVOID — everyone can do anything
```

---

## chown — Change Ownership

```bash
chown alice file.txt                # Change owner to alice
chown alice:devops file.txt         # Change owner and group
chown :devops file.txt              # Change group only
chown -R alice:devops project/      # Recursive — apply to all contents
```

### Check Current Ownership

```bash
ls -l file.txt
# -rw-r--r-- 1 alice devops 1024 Apr 16 10:00 file.txt
#              ^^^^^ ^^^^^^
#              owner  group
```

---

## chgrp — Change Group

```bash
chgrp devops file.txt           # Change group to devops
chgrp -R devops project/        # Recursive
```

---

## umask — Default Permission Mask

`umask` defines the permissions that are **removed** from new files and directories.

```bash
umask               # Show current umask
umask 022           # Set umask
```

How it works:

```
Default for files:       666 (rw-rw-rw-)
Default for directories: 777 (rwxrwxrwx)

umask 022:
  Files:       666 - 022 = 644 (rw-r--r--)
  Directories: 777 - 022 = 755 (rwxr-xr-x)

umask 077:
  Files:       666 - 077 = 600 (rw-------)
  Directories: 777 - 077 = 700 (rwx------)
```

> A umask of `077` is recommended for sensitive environments — only the owner gets access by default.

---

## Special Permissions

### SUID (Set User ID) — `4xxx`

When set on an executable, it runs as the **file owner** instead of the user who ran it.

```bash
chmod u+s program       # Symbolic
chmod 4755 program      # Octal

ls -l program
# -rwsr-xr-x  ← notice the 's' in owner execute position
```

Example: `/usr/bin/passwd` has SUID so regular users can change their own password (which writes to `/etc/shadow`, a root-owned file).

### SGID (Set Group ID) — `2xxx`

On files: runs as the **file's group**.
On directories: new files inside inherit the **directory's group** (useful for shared folders).

```bash
chmod g+s shared_dir/       # Symbolic
chmod 2775 shared_dir/      # Octal

ls -ld shared_dir/
# drwxrwsr-x  ← notice the 's' in group execute position
```

### Sticky Bit — `1xxx`

On directories: only the **file owner** can delete their own files (even if others have write access).

```bash
chmod +t /tmp           # Symbolic
chmod 1777 /tmp         # Octal

ls -ld /tmp
# drwxrwxrwt  ← notice the 't' in other execute position
```

Example: `/tmp` has the sticky bit — everyone can create files, but you can only delete your own.

---

## Users & Groups

### Managing Users

```bash
whoami                          # Show current user
id                              # Show user ID, group ID, and groups
sudo useradd -m -s /bin/bash alice   # Create user with home dir and bash shell
sudo passwd alice               # Set password for user
sudo userdel -r alice           # Delete user and their home directory
```

### Managing Groups

```bash
groups                          # Show groups of current user
groups alice                    # Show groups of specific user
sudo groupadd devops            # Create new group
sudo usermod -aG devops alice   # Add alice to devops group (-a = append)
sudo gpasswd -d alice devops    # Remove alice from devops group
```

> **Important:** After adding a user to a group, they need to log out and back in for it to take effect.

### Key Files

| File | Purpose |
|------|---------|
| `/etc/passwd` | User accounts (username, UID, home dir, shell) |
| `/etc/shadow` | Encrypted passwords (root access only) |
| `/etc/group` | Group definitions and memberships |

---

## Cloud / Server Scenarios

### SSH Key Permissions (Mandatory)

```bash
chmod 700 ~/.ssh                # Directory
chmod 600 ~/.ssh/id_rsa         # Private key
chmod 644 ~/.ssh/id_rsa.pub     # Public key
chmod 600 ~/.ssh/authorized_keys # Authorized keys
```

> SSH will **refuse to connect** if these permissions are too open.

### Shared Project Directory

```bash
sudo groupadd project-team
sudo usermod -aG project-team alice
sudo usermod -aG project-team bob

sudo mkdir /opt/project
sudo chown root:project-team /opt/project
sudo chmod 2775 /opt/project
# SGID ensures new files inherit the project-team group
```

### Web Server Files

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html     # Directories
sudo find /var/www/html -type f -exec chmod 644 {} \;  # Files
```

---

## Quick Troubleshooting

| Problem | Likely Cause | Fix |
|---------|-------------|-----|
| `Permission denied` running a script | Missing execute bit | `chmod +x script.sh` |
| SSH key rejected | Permissions too open | `chmod 600 ~/.ssh/id_rsa` |
| Can't `cd` into directory | Missing execute on directory | `chmod +x directory/` |
| Can't create files in shared dir | Missing write on directory | Check group + `chmod g+w dir/` |
| New files in shared dir have wrong group | SGID not set | `chmod g+s dir/` |

---

*Practice: Try creating users, groups, and shared directories on your Ubuntu Server VM to see these in action.*
