# Phase 3: System Administration & Monitoring

This file contains the commands, explanations, warnings, and verification questions we covered during Phase 3.

---

## 3.1 Process Management

A process is a running instance of a program. Every process has a unique PID (Process ID).

### Commands
* **`ps`:** Lists processes in your current terminal session.
* **`ps aux`:** Lists every running process on the entire system in detail.
  * *DevOps Tip:* Often piped to grep to find a service: `ps aux | grep nginx`.
* **`top`:** Interactive real-time process and system monitoring tool. Press `q` to exit.
* **`htop`:** A modern, color-coded, user-friendly interactive process viewer (highly recommended!).

### Background & Foreground Jobs
* **`&` (Background):** Append `&` to run a command in the background (e.g. `sleep 300 &`).
* **`jobs`:** Lists background jobs in the current terminal.
  * `jobs -l`: Lists jobs along with their Process IDs (PIDs).
* **`Ctrl + Z`:** Suspends (pauses) the active foreground process.
* **`bg %1`:** Resumes job #1 in the background.
* **`fg %1`:** Brings job #1 back to the foreground.
* **`nohup command &`:** Runs a command that ignores hangup signals. It continues running even if you close the terminal or lose SSH connection.

### Signals & Terminating Processes
* **`kill PID`:** Sends `SIGTERM` (terminate) signal. Asks the process to save state and exit cleanly.
* **`kill -9 PID`:** Sends `SIGKILL` signal. Forces the process to terminate instantly and destructively.
* **`pkill process_name`:** Kills processes by name.
* **`kill %1`:** Kills a process using its Job ID.

---

## 3.2 Disk Space & Resource Monitoring

### Commands
* **`df -h`:** Shows available disk space on all mounted filesystems in human-readable format (`-h`).
* **`du -sh /path`:** Prints the summarized (`-s`) disk usage size of a directory in human-readable format (`-h`).
  * `du -sh *` (finds sizes of all items in current directory).
* **`sudo`:** Used to run commands as root to bypass `Permission denied` errors when inspecting folders (e.g. `sudo du -sh /var/log`).

### Inodes (The Inode Trap)
An **Inode** is a file's metadata index entry on disk. Every file/folder uses 1 inode.
* Filesystems have a **fixed limit of inodes** created at format time.
* If an application creates millions of empty files, it will run out of inodes.
* **The Symptom:** You get `"No space left on device"` errors, even though `df -h` shows plenty of free Gigabytes!
* **Command to check:**
  ```bash
  df -i
  ```
  Check the `IUse%` column. If it is `100%`, you are out of inodes.

---

## 3.3 Package Management & Repository Administration

Ubuntu uses the **APT** (Advanced Package Tool) package manager to manage software packages (`.deb` format).

### Commands
* **`sudo apt update`:** Updates the **local package index** (database of what packages and versions are available in remote repositories). Does not install or update software!
* **`sudo apt upgrade`:** Actually downloads and installs updates for all packages currently on the system.
* **`sudo apt install package_name`:** Installs a package (e.g. `sudo apt install htop`).
* **`sudo apt remove package_name`:** Uninstalls a package, but keeps configuration files.
* **`sudo apt purge package_name`:** Uninstalls a package AND completely deletes its configurations.
* **`sudo apt autoremove`:** Cleans up orphaned dependencies (helper packages that are no longer needed).
* **`apt show package_name`:** Displays details (size, version, homepage) of a package.
* **`sudo dpkg -i package.deb`:** Low-level tool to install a local standalone `.deb` package file.

---

## 3.4 Systemd Services & Logs

Systemd is the system initializer (PID 1) and service manager on modern Ubuntu systems.

### Commands
* **`sudo systemctl start service`:** Starts a service.
* **`sudo systemctl stop service`:** Stops a service.
* **`sudo systemctl restart service`:** Restarts a service.
* **`systemctl status service`:** Displays service state, PID, memory, and recent logs.
* **`sudo systemctl enable service`:** Configures service to start automatically at boot.
* **`sudo systemctl disable service`:** Disables service from booting.
* **`sudo systemctl daemon-reload`:** Reloads the Systemd configuration cache. **Must be run** every time you add or edit a `.service` file.
* **`journalctl -u service -f`:** Follows (`-f`) log messages for a specific service unit (`-u`) in real-time.

### Creating a Custom Service File
Save files ending in `.service` inside the **/etc/systemd/system/** directory.
Example: `/etc/systemd/system/my-app.service`
```ini
[Unit]
Description=My Custom Service
After=network.target

[Service]
Type=simple
User=kt
ExecStart=/home/kt/my-daemon.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

---

## Verification Questions We Covered

1. **Q:** What is the difference between `kill <PID>` and `kill -9 <PID>`?
   * **A:** `kill` sends a clean SIGTERM signal allowing graceful shutdown. `kill -9` sends SIGKILL forcing immediate, non-ignorable shutdown.
2. **Q:** How do you pause a running command and send it to the background?
   * **A:** Press `Ctrl + Z` to suspend it, then run `bg %<Job_Number>`.
3. **Q:** How do you run a command in the background so it survives terminal disconnections?
   * **A:** Use `nohup command &`.
4. **Q:** If your app says "No space left on device", but `df -h` shows 40 GB free, what is the next command?
   * **A:** Run `df -i` to check inode usage. If inodes are 100% full, new files cannot be created regardless of free disk space.
5. **Q:** What is the difference between `sudo apt update` and `sudo apt upgrade`?
   * **A:** `apt update` refreshes the package lists (cache). `apt upgrade` performs the actual installations of the updates.
6. **Q:** How do you uninstall a package and completely wipe its configuration files?
   * **A:** `sudo apt purge package_name`
7. **Q:** How do you install a local `.deb` package file?
   * **A:** `sudo dpkg -i package_name.deb`
8. **Q:** What is the difference between starting a service and enabling a service?
   * **A:** Starting (`systemctl start`) starts it right now. Enabling (`systemctl enable`) schedules it to start automatically at system boot.
9. **Q:** If you modify a service file, what command must you run?
   * **A:** `sudo systemctl daemon-reload`
