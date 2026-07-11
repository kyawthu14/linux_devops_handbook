# Phase 1: Linux Basics (Foundations)

This file contains the commands, explanations, warnings, and verification questions we covered during Phase 1.

---

## 1.1 The Filesystem Hierarchy & Navigation

In Linux, everything is a file. The directory structure is organized as an inverted tree starting from the root `/`.

### Key Directories
* `/` (Root): The top-level directory.
* `/bin` & `/usr/bin`: Essential user executable command binaries (e.g., `ls`, `cd`).
* `/sbin` & `/usr/sbin`: System binaries for administration (e.g., `reboot`, `fdisk`).
* `/etc`: System-wide configuration files (e.g., SSH config, network interfaces).
* `/home`: User home directories (e.g., `/home/kt`).
* `/root`: Home directory for the superuser (root).
* `/var`: Variable data files (e.g., system logs under `/var/log`).
* `/tmp`: Temporary files (often cleared on boot).
* `/proc` & `/sys`: Virtual filesystems representing kernel state and hardware.

### Commands
* `pwd` (Print Working Directory): Shows your current absolute directory path.
* `ls` (List): Lists files and directories.
  * `ls -l`: Long listing format (permissions, owner, size, date).
  * `ls -a`: Lists all files, including hidden files (starting with `.`).
  * `ls -lh`: Long listing with human-readable file sizes (e.g., 4.0K, 24M).
* `cd` (Change Directory): Moves around the filesystem.
  * `cd ..`: Go up one level (parent directory).
  * `cd ~` or `cd`: Go to home directory (`/home/kt`).
  * `cd -`: Go back to the previous working directory.

### Links (Soft vs Hard)
* **Symbolic Links (Soft Links/Symlinks):** Act like Windows shortcuts. They store the text path to the target. 
  * Creation: `ln -s target link_name`
  * If the target is deleted, the soft link breaks and cannot read the content.
* **Hard Links:** Act as another name for the exact same physical data on the disk (inode). 
  * Creation: `ln target link_name`
  * If the original file is deleted, the hard link still retains access to the data.

---

## 1.2 File and Directory Operations

### Commands
* `mkdir folder_name`: Creates a new directory.
  * `mkdir -p path/to/folders`: The `-p` (parents) flag creates nested directories recursively.
* `touch file.txt`: Creates an empty file or updates the modification timestamp of an existing file.
* `cp source destination`: Copies a file.
  * `cp -r source_dir dest_dir`: The `-r` (recursive) flag is mandatory to copy directories.
* `mv source destination`: Moves or renames a file or directory.
* `rm file.txt`: Deletes a file.
  * `rm -r directory`: Recursively deletes a directory and all of its contents.
  * `rm -rf directory`: Forcefully deletes a directory without prompting. **WARNING:** Extremely dangerous if run on system folders!

---

## 1.3 Viewing & Searching Files

### Commands
* `cat file.txt`: Prints the entire contents of a file to the screen (best for small files).
* `less file.txt`: Opens the file in an interactive pager.
  * **Navigation:** Arrows to scroll, `Spacebar` for next page, `b` for previous page, `/word` to search (press `n` for next match, `N` for previous match), `q` to quit.
* `head -n 5 file.txt`: Shows the first 5 lines of a file.
* `tail -n 5 file.txt`: Shows the last 5 lines of a file.
  * `tail -f file.log`: The `-f` (follow) flag keeps the file open and streams new log entries in real-time.
* `grep "pattern" file.txt`: Searches for a word or pattern inside a file.
  * `grep -i`: Case-insensitive search.
  * `grep -n`: Shows line numbers of matches.
  * `grep -r`: Recursively searches all files inside a directory.
* `find /path -name "pattern"`: Locates files on disk by name.
  * `find . -type f -name "*.txt"`: Finds only files (`-type f`) ending in `.txt`.
* **Keyboard Shortcut:** `Ctrl + C` sends a cancel signal to stop any running process (like `tail -f`).

---

## 1.4 Users, Groups, and Permissions

Every file has an Owner (User) and a Group. Access is represented by 10 characters (e.g., `-rwxr-xr--`):
* 1st char: Type (`-` = file, `d` = directory, `l` = link).
* 2-4 chars: Owner/User permissions.
* 5-7 chars: Group permissions.
* 8-10 chars: Others (everyone else) permissions.

### Permission Types
* `r` (Read): View file content / list directory files.
* `w` (Write): Modify file content / create or delete files in directory.
* `x` (Execute): Run file as a program / enter (`cd`) directory.

### Modifying Permissions (`chmod`)
* **Symbolic:** `chmod u+x script.sh` (adds execute to owner). `chmod g-w config.txt` (removes write from group).
* **Numeric (Octal):** `r=4`, `w=2`, `x=1`, `-=0`
  * `755` = `rwxr-xr-x` (Owner: read/write/execute; Group/Others: read/execute).
  * `644` = `rw-r--r--` (Owner: read/write; Group/Others: read only).
  * `chmod 755 script.sh`

### Modifying Ownership (`chown`)
* `sudo chown root file.txt`: Changes owner to root.
* `sudo chown root:adm file.txt`: Changes owner to root and group to adm at the same time.

### Administrative privileges (`sudo`)
* `sudo` (Superuser Do) runs commands temporarily as the `root` user.

---

## Verification Questions We Covered

1. **Q:** If a file has permissions `-rwxr-xr--`, what can a user in the **Group** do? What can **Others** do?
   * **A:** Group users can read and execute (`r-x`). Others can only read (`r--`).
2. **Q:** What does `chmod 700 script.sh` translate to? Who can access it?
   * **A:** Translates to `rwx------`. Only the Owner can read, write, and execute it. Group and Others have no permissions.
3. **Q:** How do you change the owner of `application.log` to `root` and group to `adm` simultaneously?
   * **A:** `sudo chown root:adm application.log`
