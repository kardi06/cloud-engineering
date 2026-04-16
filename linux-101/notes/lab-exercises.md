# Lab Exercises — Week 1: Linux Basics

> Hands-on practice to reinforce directory navigation, file management, permissions, and text editors.
> Run these on your Ubuntu Server VM.

---

## Lab 1: Directory Navigation & File Management

### Objective

Create a project folder structure, manipulate files, and practice navigation commands.

### Tasks

```bash
# 1. Create a project structure in one command
mkdir -p ~/lab1/project/{src,docs,logs,config}

# 2. Verify the structure
tree ~/lab1       # if tree is installed
ls -R ~/lab1      # alternative

# 3. Create files inside the structure
touch ~/lab1/project/src/app.sh
touch ~/lab1/project/docs/readme.txt
touch ~/lab1/project/config/settings.conf
touch ~/lab1/project/logs/app.log

# 4. Write content into files
echo "#!/bin/bash" > ~/lab1/project/src/app.sh
echo "echo 'Hello from Lab 1'" >> ~/lab1/project/src/app.sh

echo "LOG_LEVEL=info" > ~/lab1/project/config/settings.conf
echo "PORT=8080" >> ~/lab1/project/config/settings.conf

# 5. Copy the entire project as a backup
cp -r ~/lab1/project ~/lab1/project_backup

# 6. Rename the backup
mv ~/lab1/project_backup ~/lab1/project_v1

# 7. Verify both exist
ls ~/lab1/

# 8. Remove the backup
rm -rf ~/lab1/project_v1

# 9. Verify it's gone
ls ~/lab1/
```

### Validation

- [ ] `ls -R ~/lab1/project` shows all 4 subdirectories with files
- [ ] `cat ~/lab1/project/src/app.sh` shows the script content
- [ ] `~/lab1/project_v1` no longer exists

---

## Lab 2: File Permissions & Ownership

### Objective

Practice reading and setting permissions using both symbolic and octal notation.

### Tasks

```bash
# 1. Check the current permissions of your lab files
ls -la ~/lab1/project/src/app.sh
ls -la ~/lab1/project/config/settings.conf

# 2. Make the script executable
chmod u+x ~/lab1/project/src/app.sh

# 3. Run it
~/lab1/project/src/app.sh
# Expected output: Hello from Lab 1

# 4. Restrict the config file — owner read/write only
chmod 600 ~/lab1/project/config/settings.conf

# 5. Verify the change
ls -l ~/lab1/project/config/settings.conf
# Expected: -rw-------

# 6. Set directory permissions for logs (owner: full, group: read, other: none)
chmod 740 ~/lab1/project/logs/

# 7. Verify
ls -ld ~/lab1/project/logs/
# Expected: drwxr-----

# 8. Practice octal — set all files in docs to 644
chmod 644 ~/lab1/project/docs/readme.txt

# 9. Check your umask
umask
# Note the value — it controls default permissions for new files

# 10. Create a file and see what permissions it gets
touch ~/lab1/project/docs/newfile.txt
ls -l ~/lab1/project/docs/newfile.txt
# Compare with your umask value
```

### Validation

- [ ] `ls -l ~/lab1/project/src/app.sh` shows `-rwxr--r--` or similar with `x` for owner
- [ ] Running `~/lab1/project/src/app.sh` prints "Hello from Lab 1"
- [ ] `ls -l ~/lab1/project/config/settings.conf` shows `-rw-------`
- [ ] You can explain why new files get the permissions they do (umask)

---

## Lab 3: Users & Groups

### Objective

Create users and groups, assign ownership, and set up a shared directory.

### Tasks

```bash
# 1. Check your current user and groups
whoami
id
groups

# 2. Create a new group
sudo groupadd lab-team

# 3. Add your user to the group
sudo usermod -aG lab-team $USER

# 4. Verify (you may need to log out and back in)
groups $USER

# 5. Create a shared directory
sudo mkdir -p /opt/lab-shared

# 6. Set ownership — root as owner, lab-team as group
sudo chown root:lab-team /opt/lab-shared

# 7. Set permissions with SGID (new files inherit group)
sudo chmod 2775 /opt/lab-shared

# 8. Verify the SGID bit
ls -ld /opt/lab-shared
# Expected: drwxrwsr-x  (notice the 's')

# 9. Create a file inside (after re-login for group to take effect)
touch /opt/lab-shared/team-notes.txt

# 10. Check that the file inherited the group
ls -l /opt/lab-shared/team-notes.txt
# The group should be lab-team, not your default group
```

### Validation

- [ ] `groups $USER` includes `lab-team`
- [ ] `ls -ld /opt/lab-shared` shows `drwxrwsr-x` with group `lab-team`
- [ ] Files created inside `/opt/lab-shared` inherit the `lab-team` group

---

## Lab 4: Text Editors (Vim & Nano)

### Objective

Practice creating and editing files with both Vim and Nano.

### Part A — Nano

```bash
# 1. Create a new file with Nano
nano ~/lab1/project/docs/nano-test.txt

# Inside Nano:
#   - Type: "This file was created with Nano"
#   - Press Ctrl+O, then Enter to save
#   - Press Ctrl+X to exit

# 2. Verify
cat ~/lab1/project/docs/nano-test.txt

# 3. Open again and add a second line
nano ~/lab1/project/docs/nano-test.txt

# Inside Nano:
#   - Go to end of file
#   - Add: "Edited on $(date)"
#   - Use Ctrl+W to search for "created"
#   - Save and exit
```

### Part B — Vim

```bash
# 1. Create a new file with Vim
vim ~/lab1/project/docs/vim-test.txt

# Inside Vim:
#   - Press i to enter Insert mode
#   - Type: "This file was created with Vim"
#   - Press Esc to go back to Normal mode
#   - Type :wq and press Enter to save and quit

# 2. Verify
cat ~/lab1/project/docs/vim-test.txt

# 3. Open again and practice editing
vim ~/lab1/project/docs/vim-test.txt

# Inside Vim:
#   - Press o to open a new line below and enter Insert mode
#   - Type: "Second line added in Vim"
#   - Press Esc
#   - Type yy to copy the line
#   - Type p to paste it below
#   - Type dd to delete the duplicate
#   - Type :wq to save and quit

# 4. (Bonus) Run the built-in Vim tutorial
vimtutor
```

### Validation

- [ ] `cat ~/lab1/project/docs/nano-test.txt` shows your text
- [ ] `cat ~/lab1/project/docs/vim-test.txt` shows two lines
- [ ] You can enter and exit both editors without getting stuck

---

## Lab 5: Putting It All Together

### Scenario

You are setting up a simple deployment directory on a server. A deploy script needs to be executable, config files need to be restricted, and logs should be readable by the team.

### Tasks

```bash
# 1. Create the deployment structure
mkdir -p ~/lab5/deploy/{bin,config,logs}

# 2. Create a deploy script
cat > ~/lab5/deploy/bin/deploy.sh << 'EOF'
#!/bin/bash
echo "[$(date)] Deployment started"
echo "Reading config..."
cat ~/lab5/deploy/config/app.env
echo "[$(date)] Deployment complete"
EOF

# 3. Create a config file with sensitive data
cat > ~/lab5/deploy/config/app.env << 'EOF'
DB_HOST=localhost
DB_PORT=5432
DB_PASSWORD=supersecret123
EOF

# 4. Create a sample log file
echo "[$(date)] Server started" > ~/lab5/deploy/logs/deploy.log

# 5. Set permissions:
#    - deploy.sh: owner can execute, group can read, others nothing
chmod 750 ~/lab5/deploy/bin/deploy.sh

#    - app.env: owner only (sensitive!)
chmod 600 ~/lab5/deploy/config/app.env

#    - logs: owner full, group read, others read
chmod 644 ~/lab5/deploy/logs/deploy.log
chmod 755 ~/lab5/deploy/logs/

# 6. Run the deploy script
~/lab5/deploy/bin/deploy.sh

# 7. Verify all permissions are correct
ls -laR ~/lab5/deploy/
```

### Validation

- [ ] Deploy script runs and prints the config values
- [ ] `ls -l ~/lab5/deploy/config/app.env` shows `-rw-------`
- [ ] `ls -l ~/lab5/deploy/bin/deploy.sh` shows `-rwxr-x---`
- [ ] `ls -l ~/lab5/deploy/logs/deploy.log` shows `-rw-r--r--`

---

## Cleanup

When you're done with all labs:

```bash
rm -rf ~/lab1 ~/lab5
sudo rm -rf /opt/lab-shared
sudo groupdel lab-team
```

---

*Completed all labs? You're ready for Week 2: Networking & SSH.*
