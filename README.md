# Linux & DevOps Mastery Handbook

Welcome to my personal Linux and DevOps reference handbook! This repository contains comprehensive notes, commands, practical exercise logs, and troubleshooting warnings compiled during my journey from beginner to master.

These notes were created and tested in a real-world Ubuntu server environment.

---

## 🗺️ Table of Contents

Click on any section below to view the detailed commands, explanations, and key SRE warnings:

1. 📂 [**Phase 1: Linux Basics (Foundations)**](phase1_basics.md)
   * Filesystem Hierarchy Standard, Navigation (`ls`, `cd`, `pwd`), File Operations (`mkdir`, `cp`, `mv`, `rm`), soft/hard links, and detailed user permissions (`chmod`, `chown`, `sudo`).
2. ✍️ [**Phase 2: Text Manipulation & Shell Scripting**](phase2_text_scripting.md)
   * Redirections (`>`, `>>`, `<`), streams (stdin, stdout, stderr), command piping (`|`), text processing (`awk`, `sed`, `cut`, `sort`, `uniq`), and Bash scripting automation rules (variables, loops, conditionals, exit codes).
3. ⚙️ [**Phase 3: System Administration & Monitoring**](phase3_sysadmin_monitoring.md)
   * Process management (`ps`, `top`/`htop`, jobs control, signals), disk space diagnostics (`df`, `du`, mount points, and the Inode exhaustion trap), package management (`apt`, `dpkg`), and Systemd services configuration (`systemctl`, custom `.service` daemons, `journalctl`).
4. 🌐 [**Phase 4: Networking & Security**](phase4_networking_security.md)
   * Networking commands (`ip`, `ss`, `ping`, `curl`, `wget`), firewall management (UFW rules, database port security), SSH keys administration (`ssh-keygen`, `authorized_keys`), and SSH Client config shortcuts (`~/.ssh/config`).
5. 🚀 [**Phase 5: Advanced DevOps & SRE Skills**](phase5_advanced_devops.md)
   * Automated cron scheduling (`crontab`), log locations under `/var/log` (auditing `auth.log`), log rotation rules (`logrotate`), and system boot sequence troubleshooting.

---

## 🛠️ How to Use This Handbook

* **Search Commands:** Open any phase file and press `Ctrl + F` to search for specific commands.
* **Practice:** Open your terminal and copy-paste commands directly to test them.
* **Troubleshoot:** Check the **⚠️ WARNING** callouts inside the notes before modifying system configurations (like firewalls or private keys) to avoid lockouts.
